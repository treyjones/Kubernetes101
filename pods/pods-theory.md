# Pod :
- A group of one or more containers, with shared storage and network resources with a specification for how to run the containers.
- Contains containers that relatively coupled
> There is no need of creating pods directly, instead create them using workload resources such as **Deployment** or **job**.

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
- Specifications for creating pods, included in workloads like deployments, Jobs, Deamonsets
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

### Questions??
1. Why would you or in what scenario would you choose to run static Pods?
2. How are static Pods created?

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

### Pod Phase
1. **Pending** - The Pod has been accepted by the Kubernetes cluster, but one or more of the containers has not been set up and made ready to run. 
- This includes time a Pod spends waiting to be scheduled as well as the time spent downloading container images over the network.

2. **Running** - The Pod has been bound to a node, and all of the containers have been created. At least one container is still running, or is in the process of starting or restarting.

3. **Succeeded** - All containers in the Pod have terminated in success, and will not be restarted.

4. **Failed** - All containers in the Pod have terminated, and at least one container has terminated in failure. That is, the container either exited with non-zero status or was terminated by the system

5.  Unknown	For some reason the state of the Pod could not be obtained. This phase typically occurs due to an error in communicating with the node where the pod should be running 

>Note: When a Pod is being deleted, it is shown as Terminating by some kubectl commands. This Terminating status is not one of the Pod phases. A Pod is granted a term to terminate gracefully, which defaults to 30 seconds. You can use the flag --force to terminate a Pod by force. 

### Container State
- Once the scheduler assigns a Pod to a Node, the kubelet starts creating containers for that Pod using a container runtime. There are three possible container states: **Waiting, Running**, and **Terminated**

> To check the state of a Pod's containers, you can use kubectl describe pod <name-of-pod>. The output shows the state for each container within that Pod

1. Waiting
- If a container is not in either the Running or Terminated state, it is Waiting. 
- A container in the Waiting state is still running the operations it requires in order to complete start up: 
- for example, pulling the container image from a container image registry, or applying Secret data. - When you use kubectl to query a Pod with a container that is Waiting, you also see a Reason field to summarize why the container is in that state.

2. Running
The Running status indicates that a container is executing without issues. If there was a postStart hook configured, it has already executed and finished.
- When you use kubectl to query a Pod with a container that is Running, you also see information about when the container entered the Running state

3. Terminated
- A container in the Terminated state began execution and then either ran to completion or failed for some reason. 
- When you use kubectl to query a Pod with a container that is Terminated, you see a reason, an exit code, and the start and finish time for that container's period of execution

### Pod conditions 
> A Pod has a PodStatus, which has an array of PodConditions through which the Pod has or has not passed:

- **PodScheduled:** the Pod has been scheduled to a node.
-**ContainersReady:** all containers in the Pod are ready.
-**Initialized:** all init containers have started successfully.
- **Ready:** the Pod is able to serve requests and should be added to the load balancing pools of all matching Services

### Container probes
A Probe is a diagnostic performed periodically by the kubelet on a Container. To perform a diagnostic, the kubelet calls a Handler implemented by the container. 

#### There are three types of handlers:
1. ExecAction: Executes a specified command inside the container. The diagnostic is considered successful if the command exits with a status code of 0.

2. TCPSocketAction: Performs a TCP check against the Pod's IP address on a specified port. The diagnostic is considered successful if the port is open.

3. HTTPGetAction: Performs an HTTP GET request against the Pod's IP address on a specified port and path. The diagnostic is considered successful if the response has a status code greater than or equal to 200 and less than 400.

#### The kubelet can optionally perform and react to three kinds of probes on running containers:

1. livenessProbe: Indicates whether the container is running. If the liveness probe fails, the kubelet kills the container, and the container is subjected to its restart policy. 
> If a Container does not provide a liveness probe, the default state is Success.

2. readinessProbe: Indicates whether the container is ready to respond to requests. If the readiness probe fails, the endpoints controller removes the Pod's IP address from the endpoints of all Services that match the Pod. The default state of readiness before the initial delay is Failure.
> If a Container does not provide a readiness probe, the default state is Success.

3. startupProbe: Indicates whether the application within the container is started. All other probes are disabled if a startup probe is provided, until it succeeds. If the startup probe fails, the kubelet kills the container, and the container is subjected to its restart policy. 
>If a Container does not provide a startup probe, the default state is Success