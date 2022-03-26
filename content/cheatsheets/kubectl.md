# Kubectl Cheatsheet

Docs: 
- https://kubernetes.io/docs/reference/kubectl/cheatsheet/
- https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands
- https://erosb.github.io/post/json-patch-vs-merge-patch/

## Viewing, finding resources

```bash
# get resource sort by name
kubectl get services --sort-by=.metadata.name
```


## Troubleshoot

#### Run a debugger pod / deployment

```bash
# run a mysql client pod to troubleshoot database issue:
kubectl run myclient --rm -i --tty --image mysql:5.7 -- bash
```

```bash
# run a postgres client pod to troubleshoot database issue:
kubectl run pgclient --rm -i --tty --image postgres:13-alpine -- bash
```

```bash
# run a mongo client pod to troubleshoot mongodb issue:
kubectl run mongoclient --rm -i --tty --image mongo:5 -- bash
```

```bash
# run a busybox pod to troubleshoot network or http issue
kubectl run debugger --rm -i --tty --image nicolaka/netshoot:latest -- bash
```

```bash
# run a netshoot pod to troubleshoot network or http issue
kubectl run debugger --rm -i --tty --image nicolaka/netshoot:latest -- bash
```

#### Temporarily Change Resource

```bash
# disable healthcheck if you want to troubleshoot healthcheck failure
# --type json, use json patch(RFC 6902), check official docs
kubectl patch deployment nginx --type json -p='[{"op": "remove", "path": "/spec/template/spec/containers/0/livenessProbe"}]'
```

```bash
# change "command" to "sleep 1000" to troubleshoot entrypoint command error
kubectl patch deployment nginx --type json -p='[{"op": "replace", "path": "/spec/template/spec/containers/0/command", "value":["sleep", "1000"]}]'
```

```bash
# change image to troubleshoot software version issue
kubectl patch deployment nginx --type='json' -p='[{"op": "replace", "path": "/spec/template/spec/containers/0/image", "value":"nginx:1.21"}]'

# or with set image sub cmd
kubectl set image deployment/nginx nginx=nginx:1.21 # {containerName}=${image}
```

```bash
# scale a deployment
kubectl scale --replicas=2 deploy/nginx
# scale a statefulset
kubectl scale --replicas=2 statefulset/nginx
# scale multiple deployments
kubectl scale --replicas=2 deploy/nginx1 deploy/nginx2 deploy/nginx3
# scale with kubctl patch
kubectl patch deploy nginx --patch '{"spec": {"replicas": 2}}'
```

```bash
# restart container inside your pod without chaging pod name and pod ip
kubectl exec -it nginx-0 -- /bin/sh -c "kill 1"
```

## Test Kubernetes Features

#### Run a test Deployment

```bash
# run a test nginx deployment
kubectl create deployment nginx --image=nginx
```

```bash
# delete
kubectl delete deploy nginx
```

```bash
# run a test busybox deployment
kubectl create deployment busybox --image=busybox -- sleep 1000
```

```bash
# delete
kubectl delete deploy busybox 
```

#### Create yaml objects from stdin

```bash
# create nginx statefulset
export APP_NAME=nginx
export IMAGE=nginx:latest

cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Service
metadata:
  name: ${APP_NAME}
  labels:
    app: ${APP_NAME}
spec:
  ports:
  - port: 80
    name: web
  clusterIP: None
  selector:
    app: ${APP_NAME}
---
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: ${APP_NAME}
spec:
  serviceName: ${APP_NAME}
  replicas: 1
  selector:
    matchLabels:
      app: ${APP_NAME}
  template:
    metadata:
      labels:
        app: ${APP_NAME}
    spec:
      containers:
      - name: ${APP_NAME}
        image: ${IMAGE}
        ports:
        - containerPort: 80
          name: web
#         volumeMounts:
#         - name: www
#           mountPath: /usr/share/nginx/html
#   volumeClaimTemplates:
#   - metadata:
#       name: www
#     spec:
#       accessModes: [ "ReadWriteOnce" ]
#       resources:
#         requests:
#           storage: 1Gi
EOF
```

## Useful resource templates / snippets
```yaml

```

## Awesome Kubernetes

#### k9s
[k9s](https://github.com/derailed/k9s): Terminal Graphical UI

#### kubectx
[kubectx](https://github.com/ahmetb/kubectx): Kube Context Switcher

#### kubectx
[kubeval](https://github.com/instrumenta/kubeval): Validate Your K8S Yaml
```bash
kubeval /tmp/a.yaml 
PASS - /tmp/a.yaml contains a valid Service (nginx)
PASS - /tmp/a.yaml contains a valid StatefulSet (nginx)
```