# Kubernetes Architecture


**Question: what is Kubernetes architecture??**
- Kubernetes architecture is the structure and design of how Kubernetes manages containerized applications. It's built around a **cluster model** that separates responsibilities into two main parts:
    - **control plane**
    - **worker notes**

---

**Question: explain the two clusters of the Kubernetes architecture??**
- Kubernetes architecture is made up of two main clusters i.e., the control plane (which is the master components) and the workers nodes (containers).

---

**Question: describe the different components of the control plane??**
- the control plabe is the brian behind all the  operations inside the cluster. it made up of the following components:
1. API server (`kube-apiserver`) - is the entry point for all commands and communicates with all other components. 
2. scheduler (`kube-scheduler`) - assigns new workload objects such as pods encapsulating containers to nodes (typically worker nodes). also considers resources, constraints, policies etc. 
3. controller manager (`kube-controller-manager`) - maintains the desired state of the cluster e.g, ensures correct number of pods running.
4. etcd - distributed key-value store. stores all cluster data (configuration, state). 
5. cloud-controller-manager (optional) - used to interact with API of cloud providers, to create external resources like load balancers, storage or security groups


**Question: what is the function of the API server??**
- intercepts RESTful calls from users, administrators, developers, operators, and external agents then validates and processes them.
- serves as the middle interface for any control plane agent inquiring about the cluster's state.
- highly configurable and customisable, hence can be used to support additional custom secondary API servers (configurations to transform primary API server into a proxy to all secondary custom API servers). 

    - **Question: how does the API server obtain the Kubernetes cluster's current state??**
        - reads the state from the key-value store

    **Question: which only component can talk directly to the key-value store??**
        - API sever

**Admission Controllers**
- these are built-in models that enforce cluster-wide policies before requests persist into the cluster's datastore. for example:

1. LimitRanger controller - enforces resource usage limits on Pods and Containers.
2. NamespaceLifecycle controller - prevents creation of objects in non-existent or terminating namespaces.
3. PodSecurity controller - ensures that Pods comply with the cluster's defined standards.


**Question: what is the function of the scheduler??**
- assigns new workload objects as pods encapsulating containers, to nodes (typically worker nodes).
- responsible for gathering the cluster data (Quality of Service (QoS) requirements, data locality, anti-affinity etc).

- two phases for schedulign process: filtering phase & scoring phase

    - pod affinity improves performance and reduces latency
    - anti-affinity increases fault tolerance
    - taints & tolerations...

**Question: what is the function of controller managers??**
- responsible for running controllers or operator processes to regulate the state of the Kubernetes cluster.
- kube-controller-manager runs controllers or operators responsible to act when nodes become unavailable to ensure container pod counts are expected to create endpoints, service accounts, and API access tokens.
- also runs controllers or operators responsible for interacting with a cloud provider's underlying infrastructure.

---

**Question: define etcd??**
- is a distributed key-value data store used to persist a Kubernetes cluster's state.

**Question: how does etcd work??**
- by writting new data to the data store only by appending it. data is never replaced in the data store.
- obsolete data is compacted (or shredded) periodically to mimimise the size of the data store.

- NOTE: etcd is used to store configuration details (such as subnets, ConfigMaps, Secrets etc).

- etcd stores data in simple hierarchical structure using key-value pairs. each key represents a Kubernetes object or configuration path, and the corresponding value contains the serialised data in JSON or Protocol Buffers format.

```text
Key:
/registry/pods/default/nginx

Value:
Pod configuration
```

**Question: which of the control plane components is able to communicate directly with the etcd data store??**
- API Server

**Question: why are other components of the control plane not allowed to interact with the etcd??**
- to ensure data integrity, consistency, and security. 

---

**Question: what is the main feature of Kubeadm??**
- provides stacked etcd control plane nodes where data store runs alongside and shares resources with other control plane components on the same control plane node.

---

***Question: which algorithm is the etcd based on??**
- Raft Consensus Algorithm 
    - allows collection of machines to work as a coherent group that can survice the failures of some of its members.

**Question: which programming language is etcd written in??**
- Go programming language. 

---

**Question: describe the different components of the worker node??**
1. Container runtime
2. Node agent (kubelet)
3. kubelet - CRI shims
4. Proxy (kube-proxy)

- **container runtime** is responsible for running containers on the worker node e.g., containerd, CRI-O, Docker (via cri-dockerd)
    - container runtime communicates with kubelet using the Container Runtime Interface (CRI)

    **Question: what is Container Runtime Interface (CRI)??**
    - is it the standardised gRPC-based protocol that enables Kubernetes to support multiple runtimes without modifications. 

- **kubelet** is a small agent that runs on every worker node in the cluster. the kubelet talks to the api-server and container runtime to handle the final stage of starting containers.

- **kube-proxy** is a network proxy that handles inside and outside communication of the cluster. instead of managing traffic flow on its own, the kube-proxy tries to rely on the networking capabilities of underlying operating system.
    - kube-proxy handles **TCP, UDP, and SCTP traffic and uses the OS's networking layer such as iptables, ipvs, or nftables on Linux to manage and enforce packet forwarding rules**. 

---

**Question: what is a worker node??**
- it is the machine, either virtual or physical where the actual application workloads run. it provides a running environment for client applications.

---

**Question: where are application containers encapsulated in Kubernetes??**
- Pods

---

> The Control Plane makes decisions, but the Worker Node does the execution.

- **Kubernetes namespaces** allow us to **organise cluster into logical groups**, which makes it **easier to support multi-tenancy when multiple teams share the same cluster**.

**What Do Namespaces Do??**
- resource organization (group related resource together).
- name isolation (resource names only need to be unique within a namespace e.g., `production/frontend`, `development/production`). 
- access control (RBAC) (allow administrators to control who can access what). 
- resource quotas (can limit how much CPU, memory, and storage a namespace can consume).
- environment separation (allow the same application to run in multiple environments inside the same cluster). 

**Default Namespaces**

1. default - which is the namespace where resources are created if no specifications are made

```bash
kubectl get pods
```

2. kube-system - contains Kubernetes system components e.g., CoreDNS, kube-proxy, network plugins

3. kube-public - contains resources that should be publicly accessible across the cluster

4. kube-node-lease - stores node lease objects used by nodes to send heartbeats to the control plane. this helps Kubernetes detect node failures efficiently.

**Question: command for viewing all namespaces??**

```bash
kubectl get namespaces
```

OR

```bash
kubectl get ns
```

**Question: command for creating a namespace??**

```bash
kubectl get ns
```

**Question: deploying into a namespace??**

```bash
kubectl create deployment nginx \
    --image=nginx \
    -n dev
```
- NB: the -n flag specifies the namespace

**Question: viewing resources in a namespace??**

1. view pods in dev

```bash
kubectl get pods -n dev
```

2. view deployments in dev

```bash
kubectl get deployments -n dev
```

**Question: what do namespaces provide??**
- organization of cluster resources
- isolation between teams and environments
- resource quota enforcement
- access control through RBAC

> Namespaces provide logical isolation, but all namespaces still share the same cluster. 

---

**Question: what is kernel namespaces??**
- a kernel namespace is a Linux kernel feature that provides isolation between processes by giving them their own view of certain system resources.

**Types of Linux Namespaces**

1. PID Namespace - isolates process IDs

2. Network Namespace - provides isolated networking. each container gets its own IP address, routing table, network interfaces, and port space

3. Mount Namespace - isolates filesystem mount points e.g.,:

- container A

```text
/
├── app
└── logs
```

- container B

```text
/
├── nginx
└── html
```

4. UTS Namespace - UTS (Unix Time-sharing System) isolates hostname and domain name.

5. IPC Namespace - IPC (Inter-Process Communication) isolates shared memory, message queues, and semaphores.

6. User Namespace - maps users inside a container to different users on the host. 

7. Cgroup Namespace - works with Linux control groups (cgroups) and it provides visibility only into the container's resource limits.


**Question: why are kernel namespaces important for containers??**
- they provide isolation, making containers appear as independent systems

**Question: what is the difference between namespaces and cgroups??**
- namespaces provide isolation, while cgroups provide resource control.
---

**authentication services** e.g., accounts, client certificates or bearer tokens

**authorisation** e.g., RBAC, webhook-based authorization.

---


**Question: what is a Pod??**
- a Pod is the smallest scheduling work unit in Kubernetes.
    - it is a logical collection of one or more containers scheduled together, and the collection can be started, stopped or rescheduled as a single unit of work.

> A Pod is the smallest deployable and schedulable unit in Kubernetes.

---

**Question: list some examples of container runtimes??**
- CRI-O 
- containerd
- docker engine
- mirantis container runtime

---

**Question: explain the functionality of a container runtime in Kubernetes??**
- Container runtime runs the containers which mostly involves:
    - pulling container images from registries
    - creating containers
    - starting and stopping the containers
    - managing container lifecycle
    - providing isolation - use namespaces and cgroups to isolate containers and control resource usage
    - and reporting status - sending information about running containers back to the kubelet

---

**Question: what is the function of node agent (kubelet)??**
- ensures that containers assigned to the node run correctly. 
- it communicates with the Control Plane (watches the Kubernetes API server for Pod specifications that have been assigned to its node).
- starts and manages Pods
- monitors container health

---

The kubelet connects to container runtimes through a plugin interface called Container Runtime Interface (CRI).

**Question: what is CRI mainly used for??**
- mainly used by Kubernetes so that kubelet can talk to different container runtimes in a consistent way.

---

**Question: what is a CRI shim??**
- a CRI shim is a compatible layer or adapter that allows kubelet to communicate with a container runtime using the CRI standard.

**CRI-Containerd** allows containers to be directly created and managed with containerd at kubelet's request.

**CRI-O** enables the use of any Open Container Initiative (OCI) compatible runtime with Kubernetes such as runC

---

**Question: which part of the Container Runtime Interface (CRI)specification is containerd primarily responsible for implementing??**
- low-level container execution and lifecycle management
    - containerd is a core container runtime that manages the complete container lifecycle on a single host - image transfer, storage, container execution, supervision, and network attachements. 

--

**Question: what is a [proxy] kube-proxy??**
- it is a network agent which runs on each node (control plane & workers) responsible for dynamic updates and maintainance of all network rules on the node [watches the Kubernete API for Service and Endpoint changes].
- function: abstracting the details of Pods networking and forwarding connection requests to the containers in the Pods. 

- TCP: HTTP, HTTPS, databases
- UDP: DNS, streaming, gaming
- SCTP: specialized telecom and networking applications

> Scheduler chooses where Pods run; kubelet runs Pods; kube-proxy routes network to Pods.

---

**Question: what is the purpose of Kubernetes networking??**
- solving communication problems:
    1. container to container (inside a Pod)
    2. pod to pod (inside a cluster)
    3. service to pod (inside the cluster)
    4. external user to service (outside the cluster)


1. Container-to-Container communication inside a Pod
- containers are normally isolated. when a container starts it gets its own: processes, filesystem, network stack. 
- this isolation comes from Linux namespaces.

**Question: what is network namespace made up of??**
- IP addresses
- routing tables
- networking interfaces
- ports

- since containers are pretty much isolated and they cannot communicate via localhost, Kubernetes creates a **Pause Container** which essentially creates "Pod Network Namespace".
    - containers inside this Pod can join the namespace allowing them to share the same IP address, network interfaces, and localhost. 

---

**Question: what Linux feature creates isolated network environments??**
- Network namespaces

---

**Question: what special container creates the Pod's network namespaces??**
- Pause Container

---

**Question: do containers inside the same Pod have different IP addresses??**
- No. They share the Pod IP.

---

**Question: why can containers inside the same Pod communicate through localhost??**
- because they share the same network namespace created by the Pause container. 

---

2. Pod-to-Pod Communication
- every Pod must communicate directly with every other Pods. how do we achieve this??
- using "Kubernetes Network Model". Pods are treated as virtual machines (VMs) which allows every Pod to get its own IP address, thus making communication easier as Pod A can communicate with Pod B directly using IP addresses. 

- NOTE: we don't use NAT (Network Address Translation) because it changes addresses during transmission. 
- without NAT:
    - simpler routing, better observability, and easier troubleshooting. 

> every Pod IP should be routable throughout the cluster.


**Question: what networking model does Kubernetes use??**
- IP-per-Pod


**Question: what is a fundamental Kubernetes networking requirement??**
- all Pods must be able to communicate with all other Pods without NAT


**Question: Kubernetes treats Pods similarly to what infrastructure component??**
- virtual machines (VMs)

---

3. What is CNI?
- is a standard that networking plugins follow.
- examples: calico, flannel, cilium etc


**Question: what is the pupose of CNI??**
- to provide a standard interface for configuring container networking and assigning Pod IP addresses.

---

4. Service-to-Pod communication


**Question: why do Services exist??**
- Pods are ephemeral and their IPs can change

**Question: what does a Service provide??**
- stable network endpoint and load balancing.

**Question: which component forwards Service traffic to Pods??**
- kube-proxy

---

5. External-to-Service communication

- tools used to communicate with the Kubernetes application from the user side:
    - NodePort
    - LoadBalancer
    - Ingress


**Question: which Kubernetes object typically provides HTTP routing into a cluster??**
- Ingress


**Question: which Service type exposes an application on every node's port??**
- NodePort


**Question: which Service type integrates with cloud load balancers??**
- LoadBalancer

---

## Kubernetes API

**Question: what is Kubernetes API??**
- it is the central component of a Kubernetes cluster responsible for enabling all communication with the cluster. every user, tool, and internal component relies on the API server to interact with and manage the system.


**Question: what are the three stages of processing requests in Kubernetes??**
1. authentication
2. authorisation
3. admission control

- Service Accounts - responsible for authenticating workloads

**Question: explain how containers run on Kubernetes??**
- in the container creation workflow, the kubelet first interacts with the containerd through the CRI, and in turn containerd launches containers using an OCI-compliant runtime.

---

## Networking

**Question: list four communication paths for Kubernetes??**
1. container-to-container communication
2. pod-to-pod communication
3. pod-to-service communication
4. external-to-service communication

**Container-to-Container Communication**
- containers inside the same Pod share the **same network namespace**.
- they share: IP address, network interfaces, port space
- containers can communicate using `localhost` or `127.0.0.1`.

**Pod-to-Pod Communication**
- Pods musy be able to communicate directly with other Pods, regardless of which node they run on. 
- Pod A should be able to send packets directly to Pod B, usually implemented by a CNI plugin such as Calico, Cilium, Weave Net etc..
    - implemented by cluster's CNI network plugin, which may use overlay networking or routing-based networking. 

**Pod-to-Service Communication**
- a Pod usually talks to a Service instead of directly talking to another Pod. 
- why??? the service provides stable IP, stable DNS name, and load balancing. this is typically implemented using kube-proxy, iptables, IPVS, or eBPF mechanisms.

**External-to-Service Communication**
- traffic originating outside the cluster reaches Services through mechanisms such as NodePort, LoadBalancer, and Ingress
- traffic is then routed to the appropriate Pods.

**Question: what are the core Kubernetes networking requirements??**
- every Pod can communicate with every other Pod even when running on different nodes.
- every Node can communicate with every Pod. this allows components such as kubelet, monitoring agents, control plane components to reach Pods when necessary
- no NAT between Pods


- every Pod gets its own unique IP address. 
- most Kubernetes clusters run CoreDNS which provides **service discovery, name resolution** i.e., the ability to convert names into IP addresses (which allows application to remain independent of changing IP addresses).

**Question: what are Network Policies??**
- these are internal firewalls for the cluster which let's define which Pods or namespaces can communicate by using label selectors & also include IP-based rules using CIDR ranges.
- NB: network policies are enforced by the cluster's network plugin [CNI]
- also they **control traffic**. 

**Question: compare the roles of kube-proxy and CNI plugin??**
- CNI plugin creates and manages cluster networking (Pod IPs, routing, connectivity), while kube-proxy implements Service networking and load balancing to Pods. 

---

## Scheduling

**Question: what is scheduling??**
- scheduling refers to the process of automatically selecting the most appropriate worker node on which to run a containerized workload. 

- kube-scheduler is responsible for making scheduling decisions.
- because Kubernetes use a declarative model, the Pod is first defined, and then the scheduler decides which node it should run on. the kubelet on that node, along with the container runtime handles actually starting the containers.

---

### Revision Questions

**Question: which architectural component ensures that the actual state of cluster resources converges towards the desired state defined in their specifications??**
- kub-controller.
- controllers implement reconciliation loops. They watch the desired state via the API server and interact with resources to make the current state match the desired state (e.g., ReplicaSet controller ensuring
the right number of Pods exist).

**Question: A Pod specification includes two containers: one web server and one log shipper. How do these containers communicate efficiently over the network within the Pod??**
- containers within the same Pod share the same network namespace, meaning they can communicate using localhost and standard inter-process communication mechanisms as if they were on the same machine.

**Question: what is the primary role of kubelet on a worker node??**
- the kubelet is the primary node agent. it receives PodSpecs from the API server and interacts with the container runtime (e.g., containerd) to start, stop, and manage the containers defined in those Pods, reporting their status back to the control plane.

**Question: which Kubernetes resource provides a mechanism for grouping API objects and providing a scope for names??**
- Namespaces create logical partitions within a cluster, allowing teams or applications to operate independently. Resource names must be unique within a Namespace, but not necessarily across the entire cluster. 

**Question: when interacting with the Kubernetes API server using kubectl, what is the typical format used for defining resource manifests??**
- while the API server can accept JSON, YAML is the overwhelmingly conventional and human-readable format used in configuration files (manifests) passed to kubectl for creating or updating resources.

**Question: what core Kubernetes concept allows Deployments to perform rolling updates with zero downtime??**
- Deployments manage ReplicaSets. During a rolling update, a Deployment creates a new ReplicaSet with the updated Pod template and gradually scales it up while scaling down the old ReplicaSet, ensuring service availability throughout the process.

**Question: which component is responsible for making the ultimate decision about which node a newly created Pod should run on??**
- The kube-scheduler watches for Pods without an assigned node and selects the most suitable node based on filtering (resource requests, affinity rules, etc.) and scoring algorithms.

**Question: how does Kubernetes typically handle the failure of a container within a Pod??**
-  The kubelet monitors container health. If a container fails, the kubelet restarts it according to the Pod’s restartPolicy (Always, OnFailure, Never). The Pod itself continues to exist on the same node.

--- 
