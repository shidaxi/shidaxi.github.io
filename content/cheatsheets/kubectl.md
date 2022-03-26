# Kubectl Cheatsheet

Official Docs: 
- https://kubernetes.io/docs/reference/kubectl/cheatsheet/
- https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands
## Troubleshoot

### Run a debugger pod / deployment

run a mysql client pod to troubleshoot database issue:
```bash
# mysql
kubectl run mysql --rm -i --tty --image mysql:5.7 -- bash
```

run a busybox or netshoot pod to troubleshoot network or http issue:
```bash
# busybox
kubectl run debugger --rm -i --tty --image nicolaka/netshoot:latest -- bash

# netshoot
kubectl run debugger --rm -i --tty --image nicolaka/netshoot:latest -- bash
```

## Test Kubernetes Features

### Run a test deployment

```bash
# run a test nginx deployment
kubectl create deployment nginx --image=nginx
# delete
kubectl delete deploy nginx

# run a test busybox deployment
kubectl create deployment busybox --image=busybox -- sleep 1000
# delete
kubectl delete deploy busybox 
```

### Create yaml objects from stdin

```bash

```

## Awesome Kubernetes

* [k9s](https://github.com/derailed/k9s): Terminal Graphical UI
* [kubectx](https://github.com/ahmetb/kubectx): Kube Context Switcher