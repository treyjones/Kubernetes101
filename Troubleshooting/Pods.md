```sh
kubectl get nodes -o json | jq '.items[].spec.taints'
```
Output the result to a file...

A sample of Node spec.tain values
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

```sh
kubectl uncordon node01
```

