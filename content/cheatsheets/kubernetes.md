---
title: Kubernetes Cheatsheet
date: "2021-01-23T00:00:00+08:00"
cover: "images/cover-kubernetes-resources.png"
tags: 
  - kubectl
  - cheatsheet
  - kubernetes
  - k8s
keywords: 
  - kubectl
  - cheatsheet
  - kubernetes
  - k8s
description: ""
showFullContent: false
readingTime: false
---
# Kubernetes

## Useful Resource Templates / Snippets

```yaml
# deployment
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  namespace: default
  labels:
    app: nginx
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.21
        ports:
        - containerPort: 80
```


```yaml
# statefulset
apiVersion: apps/v1
kind: Statefulset
metadata:
  name: nginx-statefulset
  namespace: default
  labels:
    app: nginx
spec:
  serviceName: nginx
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.21
        ports:
        - containerPort: 80
        volumeMounts:
        - name: www
          mountPath: /usr/share/nginx/html
  volumeClaimTemplates:
  - metadata:
      name: www
    spec:
      accessModes: [ "ReadWriteOnce" ]
      resources:
        requests:
          storage: 1Gi
```

```yaml
# service
apiVersion: v1
kind: Service
metadata:
  name: nginx
  namespace: default
  labels:
    app: nginx
spec:
  ports:
  - port: 80
    name: web
  selector:
    app: nginx
```

```yaml
# ingress
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  annotations: {}
  name: nginx
  namespace: default
  labels:
    app: nginx
spec:
  rules:
  - host: www.example.com
    http:
      paths:
      - backend:
          service:
            name: nginx
            port:
              number: 80
        path: /
        pathType: ImplementationSpecific
```

```yaml
# configmap
apiVersion: v1
kind: ConfigMap
metadata:
  name: nginx
  namespace: default
data:
  key1: value1
  nginx.conf: |
    events {
      worker_connections  1024;
    }
    worker_processes auto;
    error_log /dev/stdout notice;
    pid /var/run/nginx.pid;
    http {
      server {
        listen 80 default_server;
        server_name _;
        return 301 https://$host$request_uri;
      }
    }
    stream {
      server {
        listen 443;
        resolver 172.20.0.10;
        proxy_pass 127.0.0.1:443;
      }
    }
    
```

```yaml
# secret
apiVersion: v1
kind: Secret
metadata:
  name: nginx
  namespace: default
type: Opaque
data:
  username: YWRtaW4=
  password: MWYyZDFlMmU2N2Rm
```

```yaml
# role
```

```yaml
# rolebinding
```

```yaml
# clusterrole
```

```yaml
# clusterrolebinding
```

```yaml
# livenessProbe and readinessProbe with http protocol
spec:
  containers:
  - name: app
    livenessProbe:
      httpGet:
        path: /healthz
        port: 8080
        httpHeaders:
        - name: Custom-Header
          value: Awesome
      initialDelaySeconds: 3
      periodSeconds: 3
    readinessProbe:
      httpGet:
        path: /healthz
        port: 8080
        httpHeaders:
        - name: Custom-Header
          value: Awesome
      initialDelaySeconds: 3
      periodSeconds: 3
```

```yaml
# livenessProbe and readinessProbe with TCP protocol
spec:
  containers:
  - name: app
    readinessProbe:
      tcpSocket:
        port: 8080
      initialDelaySeconds: 5
      periodSeconds: 10
    livenessProbe:
      tcpSocket:
        port: 8080
      initialDelaySeconds: 15
      periodSeconds: 20
```

```yaml
# mount a configmap
```

```yaml
# env from
```