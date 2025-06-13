# 2.5

## values.yaml

```
app1:
  name: app1
  image:
    repository: nginx
    tag: "latest"
  replicaCount: 1

app2:
  name: app2
  image:
    repository: httpd
    tag: "latest"
  replicaCount: 1

```

## app1-deployment.yaml

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ .Release.Name }}-{{ .Values.app1.name }}
spec:
  replicas: {{ .Values.app1.replicaCount }}
  selector:
    matchLabels:
      app: {{ .Release.Name }}-{{ .Values.app1.name }}
  template:
    metadata:
      labels:
        app: {{ .Release.Name }}-{{ .Values.app1.name }}
    spec:
      containers:
      - name: {{ .Values.app1.name }}
        image: {{ .Values.app1.image.repository }}:{{ .Values.app1.image.tag }}
        ports:
        - containerPort: 80

```

## app2-deployment.yaml

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ .Release.Name }}-{{ .Values.app1.name }}
spec:
  replicas: {{ .Values.app1.replicaCount }}
  selector:
    matchLabels:
      app: {{ .Release.Name }}-{{ .Values.app1.name }}
  template:
    metadata:
      labels:
        app: {{ .Release.Name }}-{{ .Values.app1.name }}
    spec:
      containers:
      - name: {{ .Values.app1.name }}
        image: {{ .Values.app1.image.repository }}:{{ .Values.app1.image.tag }}
        ports:
        - containerPort: 80

```

```
root@kubernetes:/home/veer/kubernetes/25/myapp-chart# helm list -A
NAME                            NAMESPACE       REVISION        UPDATED                                 STATUS          CHART                                     APP VERSION
app1-v1                         app1            1               2025-06-13 09:12:25.796599832 +0300 MSK deployed        myapp-chart-0.1.0                         1.16.0
app1-v2                         app1            1               2025-06-13 09:12:29.166741224 +0300 MSK deployed        myapp-chart-0.1.0                         1.16.0
app2-v1                         app2            1               2025-06-13 09:12:32.311608381 +0300 MSK deployed        myapp-chart-0.1.0                         1.16.0
myapp                           default         1               2025-06-13 09:03:44.317661338 +0300 MSK deployed        myapp-0.1.0                               1.0.0
nfs-subdir-external-provisioner default         1               2025-05-18 11:04:54.160818445 +0300 MSK deployed        nfs-subdir-external-provisioner-4.0.18    4.0.2
root@kubernetes:/home/veer/kubernetes/25/myapp-chart# kubectl get pods -n app1
NAME                            READY   STATUS    RESTARTS   AGE
app1-v1-app1-786c7fc8d9-xt2m4   1/1     Running   0          3m43s
app1-v1-app2-85fb4b7f4f-f9jzv   1/1     Running   0          3m43s
app1-v2-app1-67c99886b8-wlgzn   1/1     Running   0          3m39s
app1-v2-app2-5df4659588-cs44b   1/1     Running   0          3m39s
root@kubernetes:/home/veer/kubernetes/25/myapp-chart# kubectl get pods -n app2
NAME                            READY   STATUS    RESTARTS   AGE
app2-v1-app1-7cb64896d6-mfmwj   1/1     Running   0          3m38s
app2-v1-app2-7cb7c48b96-lrnqq   1/1     Running   0          3m38s
```
