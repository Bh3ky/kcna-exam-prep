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

## Containers Basics

- before containers existed, `chroot` was one of the earliest ways to isolate applications. 

**Question: what is chroot jail??**
- is a restricted environment that simulates a new root directory. within this jail, a process can only access in its assigned directory, effectively isolating it from the rest of the system while the files remain physically present. 

### `namespaces` and `cgroups`

- **namespaces** are used to isolate various resources e.g., the network
- a **network namespace** can be used to provide a complete abstraction of network interfaces and routing tables to allow a process to have its own IP address.
- Linux Kernel 5.6 currently provides eight namespaces:

1. `pid` - process ID which provides a process with its own set of process IDs

2. `net` - network allows the processes to have their own network stack, including the IP address.

3. `mnt` - mount abstracts the filesystem view and manages mount points.

4. `ipc` - inter-process communication provides separation of named shared memory segments.

5. `user` - provides the process with their own set of user IDs and group IDs.

6. `uts` - Unix time sharing allows processes to have their own hostname and domain name.

7. `cgroup` - a newer namespace that allows a process to have its own set of cgroup root directories.

8. `time` - the newest namespace can be used to virtualise the clock of the system


- cgroups are used to organise processes into hierachical groups and to assign them resources like memory and CPU


**Question: what is the difference between virtual machines and containers??**
- VMs emulate a complete machine which includes the OS and a kernel, while containers share the host machine's kernel and other isolated processes.

---

## Running Containers

- to run industry-standard containers, follow the OCI [runtime-spec] standard.
- runC is a low-level runtime that is used in a variety of tools to start containers, including Docker


**Question: can you write the Docker command used to start containers??**

```bash
docker run nginx
```

---

## Building Container Images

**Question: what is a Docker container image??**
- Docker container image is a lightweight, standalone, executable package of software that includes everything needed to run an application: code, runtime, system tools, system libraries, and settings. 

**Question: list three things that make up an image??**
1. layer containing files
2. image index with metadata describing the target platform
3. config file specifying the container's execution command

**Question: what is a Dockerfile??**
- a dockerfile provides the instructions for how to build container image with Docker. it includes all the commands users can make in the container.

**Note**: an image is a blueprint or a template. it contains the application code, libraries, dependencies, runtime, configuration e.g., `nginx:latest`, `ubuntu:24.04`, `python:3.12`. the image itself doesn't run
a container is a running instance of an image

- Docker uses the layer caching technique which makes builds faster, smaller, and more efficient

**Question: what is a container registry??**
- a registry stores images. 
    - repository used to store and distribute container images.

- a layer is a filesystem change created by Dockerfile instruction. 

```text
Dockerfile
    ↓
Build
    ↓
Image
    ↓
Push
    ↓
Registry
    ↓
Pull
    ↓
Kubernetes
    ↓
Container
```

**Question: what file is used to define how a container image is built??**
- a Dockerfile

**Question: what does the `COPY` instruction do??**
- copies files from the host into the image

**Question: what does the `FROM` instruction do??**
- specifies the base image

**Question: what does `WORKDIR` do??**
- sets the working directory for subsequent instructions

**Question: what does `CMD` specify??**
- the default command executed when the container starts

**Question: what command uploads an image to a registry??**
- `docker push`

**Question: what command downloads an image from a registry??**
- `docker pull`

**Question: True/False, does Kubernetes build container images??**
- False. Kubernetes pulls images from registries and runs containers from them. 

---

## Security

**Question: what are the 4Cs of Cloud Native Security??**
1. Code at the core - application code
2. Container - container images
3. Cluster - Kubernetes clusters
4. Cloud/Datacenter - underlying cloud or datacenter

- Containers provide **process isolation**, but they are **not security boundaries in the same way VMs are**. if a container is misconfigured, an attacker may:
    - access sensitive files
    - escalate privileges
    - escape the container
    - move laterally through the cluster
    - access Kubernetes APIs

- cloud-native security focuses on multiple layers of protection. 

What is Root?
- root is the superuser with full control.

- to prevent anyone from gaining access and running the application from the root, we create a non-root permissions:

```dockerfile
RUN useradd appuser

USER appuser
```

- the application now runs with limited permissions. 

- in Kubernetes we can enforce these layered permissions through the commands:

```YAML
securityContext:
    runAsNonRoot: true
```
- this tells Kubernetes not to start the container if it tries to run as root.


- use of public images poses security risks. always use trusted images from official publishers.


**The 4Cs of Cloud Native Security**
- security must be applied at every layer, not just one layer.

```text
Cloud / Datacenter
        ↓
     Cluster
        ↓
    Container
        ↓
       Code
```

1. Code
- this is the application source code

2. Container
- focuses on the container image

3. Cluster
- Kubernetes clusters
- typical cluster controls:
    - RBAC [Role-Based Access Control]
    - Network Policies (control pod-to-pod communication)
    - Admission Controllers (validate resources before they enter the cluster)

4. Cloud / Datacenter
- the outermost layer which includes physical servers, VMs, storage, networks, cloud infra etc

---

## Container Orchestration Fundamentals
