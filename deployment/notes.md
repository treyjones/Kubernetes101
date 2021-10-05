Since Pods are mortal, there is need for abstracting them in a deployment

Create a deployment file(yaml)

# apiVersion: the version of the Kubernetes API we will be using, v1 here
# kind: A deployment has the kind Deployment
spec:
# replicas: The number of pods this deployment will create
# selector: Which pods the deployment should look at. Here <<<<<app=Mydeployment-nginx>>>>.
# template: The template of the pods to start
# metadata: The metadata for this template, here only one label <<<<<app=Mydeployment-nginx>>>>
spec: The spec of the pods
containers:
#  image: the name of the container, here we will use version "0.4.0"
# ports: The list of ports to expose internally in the Kubernetes cluster
# containerPort:  The kind of port we want to expose, here a containerPort. So our container will expose one port 9876 in the cluster.

use <<<kubectl apply -f deployment/nginxdeployment.yaml>>>

# list all Deployments
<<<kubectl get deployment>>> or <<<kubectl get deployments>>>

Add two replicas to the deployment file
# get  all pods
<<<kubectl get pods>>> 

Scale up/Down
Let's play with our deployment now. Update the number of replicas in the yaml, to a reasonable number - say 5.
<<<kubectl apply -f deployment/nginxdeployment.yaml>>>

You can also use kubectl scale:
<<<kubectl scale --replicas=5 -f deployment/nginxdeployment.yaml>>>

You can also edit the manifest in place with kubectl edit:
<<<kubectl edit deployment simple-deployment>>>

I would recommend to only use kubectl apply as it is declarative and local to your computer, so you can commit it afterwards.

Clean up
<<<kubectl delete deployment,rs,pod --all>>>