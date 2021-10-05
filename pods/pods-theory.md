# Pod :
- A group of one or more containers, with shared storage and network resources with a specification for how to run the containers.
- Contains containers that relatively coupled
> There is no need of creatng pods directly, instead create them using workload resources such as Deployment or job.

#### Uses Of a Pod in Kubernetes (K8s)
- *Pods that run a single Container* : _One container per Pod_ is common in Kubernetes use case. Kubernetes manages pods rather than managing containers directly.
-*Pods that run multiple Containers that need to work together* : A pod can abstract several containers that share resources(networking and storage). 
> for example, one container serving data stored in a shared volume to the public, while a separate sidecar container refreshes or updates those files. The Pod wraps these containers, storage resources, and an ephemeral network identity together as a single unit.

*_Replicated Pods_* are usually created and managed as a group by a workload resource and its controller. This is usually called horizontal scaling(increasing the number of pods)

## Pods and Controllers
- A controller for the resource handles replication, rollout and automatic healing in case of pod failure
> If a Node fails, a controller notices that Pods on that Node have stopped working and creates a replacement Pod. The *scheduler* places the replacement Pod onto a healthy Node