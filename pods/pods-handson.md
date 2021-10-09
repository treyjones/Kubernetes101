We will create the pod using a yaml file
Hands-on PODS:

## create a file by the name simple-one.yaml
```sh 
Touch simple-one.yaml
```

## Edit the file
```sh 
Nano simple-one.yaml 
```

## paste the content below
```sh
apiVersion: v1
kind: Pod
metadata:
  name: nginx4
spec:
  containers:
  - name: simple-pod
    image: nginx
```

## save the file and run the command 
```sh
$kubectl apply -f simple-pod.yaml
```

## Use get commands to list the pods running
```sh
$kubectl get pods
```

## Get pod logs
```sh
$kubectl logs nginx4
```

## Get more pod details
```sh
$kubectl describe pod nginx4
```

### Use of Labels:
Labels can be used to query object eg

```sh
$kubectl get pods -l env=production
```

### Creating a pod using deployment workload
- **Create a pod-deployment-nginx.yaml file**
- Paste code below:

```sh
apiVersion: apps/v1
kind: Deployment
metadata:
  name: pod-deployment-nginx
spec:
  selector:
    matchLabels:
      app: pod-deployment-nginx
  template:
    metadata:
      labels:
        app: pod-deployment-nginx
    spec:
      containers:
      - name: pod-deployment-nginx
        image: nginx:1.14.2
        resources:
          limits:
            memory: "128Mi"
            cpu: "500m"
        ports:
        - containerPort: 8080
```
- Run Command on cmd/bash
```sh
$kubectl apply -f pod-deployment-nginx.yaml
```
> check the pod status, age, logs and describe as explained above

### Creating a pod using Job workload
- **Create a pod-job-busybox.yaml file**
- Paste code below:

```sh
apiVersion: apps/v1
kind: Deployment
metadata:
  name: pod-deployment-nginx
spec:
  selector:
    matchLabels:
      app: pod-deployment-nginx
  template:
    metadata:
      labels:
        app: pod-deployment-nginx
    spec:
      containers:
      - name: pod-deployment-nginx
        image: nginx:1.14.2
        resources:
          limits:
            memory: "128Mi"
            cpu: "500m"
        ports:
        - containerPort: 8080
```
- Run Command on cmd/bash
```sh
$kubectl apply -f pod-deployment-nginx.yaml
```
> check the pod status, age, logs and describe as explained above

