Since Pods are mortal, there is need for abstracting them in a deployment

Create a deployment file(nginxdeployment.yaml)
```
## apiVersion: the version of the Kubernetes API we will be using, v1 here
## kind: A deployment has the kind Deployment
spec:
## replicas: The number of pods this deployment will create
## selector: Which pods the deployment should look at. Here <<<<<app=mydeployment-nginx>>>>.
## template: The template of the pods to start
## metadata: The metadata for this template, here only one label <<<<<app=mydeployment-nginx>>>>
spec: The spec of the pods
containers:
##  image: the name of the container, here we will use version "0.4.0"
## ports: The list of ports to expose internally in the Kubernetes cluster
## containerPort:  The kind of port we want to expose, here a containerPort. So our container will expose one port 8080 in the cluster.
```
### Deployment File(nginxdeploment.yaml)
```sh
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mydeployment-nginx
spec:
  selector:
    matchLabels:
      app: mydeployment-nginx
  template:
    metadata:
      labels:
        app: mydeployment-nginx
    spec:
      containers:
      - name: mydeployment-nginx
        image: nginx:1.14.2
        resources:
          limits:
            memory: "128Mi"
            cpu: "500m"
        ports:
        - containerPort: 8080
```

## Deploy the deployment
use ```sh
$kubectl apply -f deployment/nginxdeployment.yaml```

# list all Deployments
```sh
$kubectl get deployment
``` 
or 
```sh
$kubectl get deployments
```

Add two replicas to the deployment file
```sh
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mydeployment-nginx
spec:
  replicas: 2
  selector:
    matchLabels:
      app: mydeployment-nginx
  template:
    metadata:
      labels:
        app: mydeployment-nginx
    spec:
      containers:
      - name: mydeployment-nginx
        image: nginx:1.14.2
        resources:
          limits:
            memory: "128Mi"
            cpu: "500m"
        ports:
        - containerPort: 8080
```

# get  all pods
```sh
$kubectl get pods
``` 

Scale up/Down
Let's play with our deployment now. Update the number of replicas in the yaml, to a reasonable number - say 5.
```sh
$kubectl apply -f deployment/nginxdeployment.yaml
```

You can also use kubectl scale:
```sh
$kubectl scale --replicas=5 -f deployment/nginxdeployment.yaml
```

You can also edit the manifest in place with kubectl edit:
```sh
$kubectl edit deployment simple-deployment
```

I would recommend to only use kubectl apply as it is declarative and local to your computer, so you can commit it afterwards.

Clean up
```sh
$kubectl delete deployment,rs,pod --all
```