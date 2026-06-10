# Container Orchestration

**Question: what is a Container??**
- a container is a lightweight, isolated environment that packages:
    - application code
    - runtime
    - libraries
    - dependencies
    - configuration
- into a single deployable unit

---

**Question: what is a microservice??**
- a small independent application focused on a specific business function

---

- container image containes code, dependencies, and runtime but does not run, while the container is the running instance of an image. 

**Question: what is the difference between a container and a container image??**
- an image is a template; a container is a running instance of that template. 

---

## Container Orchestration

- the automated deployment, scheduling, scaling, networking, and management of containers across a cluster of machines. 

---

**Question: what is a microservice??**
- a small independent application focused on a specific business function.

---

**Question: why are containers ideal for microservices??**
- they package each service and its dependencies independently.

Why do we need a lot of containers for enterprise applications??
- production systems need:
    1. fault tolerance i.e if one server fails, the application can still run
    2. scalability - need more containers automatically
    3. resource optimization - run workloads where resources exist.
    4. service discovery - services must find each other automatically without hardcoded IP addresses.

---

**Question: what does an orchestrator actually do??**
- creates clusters ie multiple nodes are bundled up together into a single cluster.
- it schedules/ decides where containers run.
- networking ie allows containers to communicate.
- load balancing ie distributes requests.
- self-healing ie recovering from container crashes.
- scaling 

---

**Orchestration Tools**

| Tool | Company |
| ---  |  ---    |
| Kubernetes | Originally Google |
| Docker Swarm | Docker |
| Nomad | HashiCorp |
| Amazon ECS | Amazon Web Services |
| Azure Service Fabric | Microsoft |

---

**Question: what are the advantages of cluster systems??**
- increased performance
- cost efficiency
- reliability
- workload distribution
- reduced latency

---

**Question: which is the security mechanism in Kubernetes is best best suited for providing an identity to processes running in Pods to interact with the API server??**
- Service Accounts.
    - these are Kubernetes security mechanism used to provide identities to processes running inside Pods so they can authenticate and interact with the Kubernetes API server. their permissions are typically governed through RBAC roles and role bindings. 

---

**Question: what is the primary purpose of a Service Mesh like Istio or Linkerd??**
- managing inter-service communications. they add observability, security, and reliability features to communication between microservices (service-to-service).

---

**Question: how do we securely store sensitive information like passwords or API keys in Kubernetes??**
- secrets. 
    - these are Kubernetes objects specifically designed to hold small amounts of sensitive data. 

---

**Question: what is a ConfigMap??**
- Kubernetes object used to store non-sensitive configuration data separately from application code and container images.

---

**Question: why is container orchestration essential for managing microservices at scale??**
- handles service discovery, scaling, and fault tolerance.

---

**Question: what is the main purpose of a ServiceAccount in Kubernetes when interacting with the API server??**
- to provide an identity for Pods to authenticate to the API. 

---

**Question: what Kubernetes object provides a stable IP address and DNS name for accessing a set of Pods??**
- service
    - defines a logical set of Pods and a policy by which to access them, providing load balancing and service discovery.

---

**Question: which Kubernetes API object is used to manage external access to services in a cluster, typically HTTP/S?**
- Ingress
    - Ingress provides HTTP/S routing rules to manage external users' access to services within the cluster based on hostnames or paths.

---

**Question: which Kubernetes object defines rules about how Pods are allowed to communicate with each other and network endpoints?**
- NetworkPolicy
    - NetworkPolicies allow specifying traffic flow rules at the IP address or port level (OSI layer 3 or 4).

---
