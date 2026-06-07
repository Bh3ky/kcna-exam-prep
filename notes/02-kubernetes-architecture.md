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
2. scheduler (`kube-scheduler`) - assigns new workload objects such as pods encapsulating containers to noes (typically worker nodes). also considers resources, constraints, policies etc. 
3. controller manager (`kube-controller-manager`) - maintains the desired state e.g, ensures correct number of pods running.
4. etcd - distributed key-value store. stores all cluster data (configuration, state). 


**Question: what is the function of the API server??**
- intercepts RESTful calls from users, administrators, developers, operators, and external agents then validates and processes them.
- serves as the middle interface for any control plane agent inquiring about the cluster's state.
- highly configurable and customisable, hence can be used to support additional custom secondary API servers (configurations to transform primary API server into a proxy to all secondary custom API servers). 

    - **Question: how does the API server obtain the Kubernetes cluster's current state??**
        - reads the state from the key-value store

    **Question: which only component can talk directly to the key-value store??**
        - API sever


**Question: what is the function of the scheduler??**
- assigns new workload objects as pods encapsulating containers, to nodes (typically worker nodes).
- responsible for gathering the cluster data (Quality of Service (QoS) requirements, data locality, anti-affinity etc).


**Question: what is the function of controller managers??**
- responsible for running controllers or operator processes to regulate the state of the K8s cluster.
- kube-controller-manager runs controllers or operators responsible to act when nodes become unavailable to ensure container pod counts are expected to create endpoints, service accounts, and API access tokens.
- also runs controllers or operators responsible for interacting with a cloud provider's underlying infrastructure.

---

**Question: define etcd??**
- is a distributed key-value data store used to persist a Kubernetes cluster's state.

**Question: how does etcd work??**
- by writting new data to the data store only by appending it. data is nenver replaced in the data store.
- obsolete data is compacted (or shredded) periodically to mimimise the size of the data store.

- NOTE: etcd is used to store configuration details (such as subnets, ConfigMaps, Secrets etc).

**Question: which of the control plane components is able to communicate directly with the etcd data store??**
- API Server

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

---

**Question: what is a worker node??**
- it is the machine, either virtual or physical where the actual application workloads run. it provides a running environment for client applications.

---

**Question: where are application containers encapsulated in Kubernetes??**
- Pods


---

> The Control Plane makes decisions, but the Worker Node does the execution.

---

**Question: what is a Pod??
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
- ensures that containers assignee to the node run correctly. 
- it communicates with the Control Plane (watches the Kubernetes API server for Pod specifications that have been assigned to its node).
- starts and manages Pods
- monitors container health

---

The kubelet connects to container runtimes through a plugin interface called Container Runtime Interface (CRI).

**Question: what is CRI mainly used for??**
- 


---