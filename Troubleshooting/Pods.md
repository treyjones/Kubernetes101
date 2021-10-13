```sh
kubectl get nodes -o json | jq '.items[].spec.taints'
```
Output the result to a file...

A sample of Node spec.taints values
```sh [
  {
    "effect": "NoSchedule",
    "key": "node-role.kubernetes.io/master"
  }
]
[
  {
    "effect": "NoSchedule",
    "key": "disktype",
    "value": "slow"
  }
]
```
## QUIZ??
What does the command below do?
```sh
kubectl uncordon node01
```
> When you want to run an upgrade on the node. **Then at this point it cannot accept pods**. uncordon restores the node back to be able to receive pods or so.

### Why cant I access my service?
First, lets get a look at whats happening within the cluster, specifically to make sure that their container is running.
```sh
kubectl get pods
```

then run 
```sh 
kubectl get services
```

Run  the command below to check endpoint status
```sh
kubectl get endpoints kuard
```
> You notice in the manifest that the pod has a label of app: kuard. The service manifest found in the second half of the manifest is trying to select pods with a label of application: kuard. This may seem like a small thing, but these need to match for the service to be associated with the pods running in the cluster.

```sh
kubectl run curl --generator=run-pod/v1 --image=radial/busyboxplus:curl -n prodapps -i --tty --rm
```
> Afterwards you teach your co-workers that services accessed from other namespaces must include a DNS suffix for the service entry:

[Service Name].[Namespace Name].[svc.cluster.local]

