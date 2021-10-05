Hands-on <<<<<<<PODS>>>>>>>>>:
Touch simple-one.yaml
Nano simple-one.yaml >>
apiVersion: v1
kind: Pod
metadata:
  name: simple-pod
spec:
  containers:
  - name: simple-pod
    image: nginx

kubectl get pods
kubectl logs simple-pod
Use of Labels:
Labels can be used to query object eg
kubectl get pods -l env=production
