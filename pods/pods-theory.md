# Pod :
- A group of one or more containers, with shared storage and network resources with a specification for how to run the containers.
- Contains containers that relatively coupled
> There is no need of creatng pods directly, instead create them using workload resources such as Deployment or job.

#### Uses Of a Pod in Kubernetes (K8s)
**Pods that run a single Container** : _One container per Pod_ is common in Kubernetes use case. Kubernetes manages pods rather than managing containers directly.
**Pods that run multiple Containers that need to work together** : A pod can abstract several containers that share resources(networking and storage). 
> for example, one container serving data stored in a shared volume to the public, while a separate sidecar container refreshes or updates those files. The Pod wraps these containers, storage resources, and an ephemeral network identity together as a single unit.

**_Replicated Pods_** are usually created and managed as a group by a workload resource and its controller. This is usually called horizontal scaling(increasing the number of pods)

## Pods and Controllers
- A controller for the resource handles replication, rollout and automatic healing in case of pod failure
> If a Node fails, a controller notices that Pods on that Node have stopped working and creates a replacement Pod. The *scheduler* places the replacement Pod onto a healthy Node

#### Resources that manages Pods
1. Deployment
2. StatefulSet
3. DaemonSet
4. Jobs

### Pod Template
- Specifications for creating pods, include in workloads like deployments, Jobs, Deamonsets
- it is part of the desired state of the workload resource used

### Pod Storage
- A Pod can specify a set of shared storage volumes. All containers in the Pod can access the shared volumes, allowing those containers to share data. Volumes also allow persistent data in a Pod to survive in case one of the containers within needs to be restarted

### Pod networking
- Each Pod is assigned a unique IP address for each address family. 
- Every container in a Pod shares the network namespace, including the IP address and network ports. - Inside a Pod (and only then), the containers that belong to the Pod can communicate with one another using localhost.
- The containers in a Pod can also communicate with each other using standard inter-process communications like SystemV semaphores or POSIX shared memory

### Static Pods
- managed directly by the kubelet daemon on a specific node
- The main use for static pods is to run a self-hosted control plane
- Pods running on a node are visible on the API server but cannot be controlled from there.
>The spec of a static Pod cannot refer to other API objects (e.g., ServiceAccount, ConfigMap, Secret, etc)

### Container Probes
A **Probe** is a diagnosis performed periodically by the Kubelet on a container.
- Kubelet invoke the actions:
1. **ExecAction** - performed with the help of the container runtime
2. TCPSocketAction - Checked directly by the kubelet
3. HTTPGetAction - checked directly by kubelet

### Pod Lifecycle
- Pods follow a define lifecycle, starting with _Pending phase_ to _Running Phase_
- Kubelet is able to restart containers to handle some kind of faults
- In Kubernetes API, pods have both a specification and an atual status
- Pods are only scheduled once in their lifetime.

 #### Pod Lifetime
-Pods are considered to be relatively ephemeral (rather than durable) entities
- Pods are created, assigned a unique ID (UID), and scheduled to nodes where they remain until termination (according to restart policy) or deletion
- Pods do not self heal by themselves
- Pod won't survive an eviction due to a lack of resources or Node maintenance
- A given Pod (as defined by a UID) is never "rescheduled" to a different node; instead, that Pod can be replaced by a new, near-identical Pod, with even the same name if desired, but with a different UID

### Questions??
1. Why would you or in what scenario would you choose to run static Pods?
2. How are static Pods created?

