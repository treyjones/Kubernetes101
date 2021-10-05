We will create the pod using a yaml file
Hands-on PODS:

## create a file by the name simple-one.yaml
Touch simple-one.yaml

## Edit the file
Nano simple-one.yaml 

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
