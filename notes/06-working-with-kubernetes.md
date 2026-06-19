# Working with Kubernetes

## Kubernetes Objects

**Question: what are objects in Kubernetes??**
- objects define how workloads should run and be managed. 

- Kubernetes objects fall into two categories:
    1. workload objects - these define and manage containerized applications.
    2. infrastructure - provide supporting functionality such as networking, access control, and configuration management.

- Kubernetes objects are defined using YAML, which is a common data serialization format
- the YAML file is submitted to the Kubernetes API server where the defintions are validated and then used to create or modify resource in the cluster.

1. `apiVersion` - specifies which version of the object's schema is being used.
2. `kind` - defines the type of Kubernetes object we want to create.
3. `metadata` - contains identifying information such as the object's name. NB: each object must have a unique name within its scope; namespaces allow objects within the same name to coexist in different logical groups. 
4. `spec` - describes the desired state of the object. NB: the structure of this field depends on the object type and may vary across versions. 

---

## Interacting with Kubernetes

- to interact with the Kubernetes API, we use the official command-line tool `kubectl`.

**Question: what is the command for listing available objects in a cluster??**

```bash
kubectl api-resources
```

- we can use `kubectl to learn more about a specific object:

```bash
kubectl explain pod
```

- to learn more about the pod spec:

```bash
kubectl explain pod.spec
```

- to display built-in help documentation:

```bash
kubectl --help
```

- to create resources defined in a YAML manifest file:

```bash
kubectl create -f pod.yaml
```

```bash
kubectl apply -f deployment.yaml
```
- creates the resource if it doesn't exist or updates the existing resource if it already exists.

```bash
YAML file
    ↓
kubectl
    ↓
API Server
    ↓
etcd (stores desired state)
    ↓
Controllers
    ↓
Worker Nodes
```

**Question: what is Helm??**
- it is a package manager for Kubernetes that simplifies deploying, updating, and managing related group of resources. 

**Question: what does `kubectl create -f pod.yaml` do??**
- creates the resources defined in `pod.yaml`.

**Question: what does the -f flag mean??**
- `-filename`; specifies the manifest file or directory to read.

**Question: which Kubernetes component receives the resource definition from `kubectl`??**
- the API Server

**Question: what happens if we run `kubectl create -f deployment.yaml` twice??**
- the second command fails because the resource already exists.

**Question: where is the desired state ultimately stored??**
- etcd

---

## Pod

**Question: what is a Pod??
- a Pod is the smallest deployable unit in Kubernetes.
OR
- basically it is one or more containers grouped together as a single sharing the same network and storage.

- workflow

```text
Kubernetes → runs Pods → Pods run containers
```

**Question: how do containers communicate inside a Pod??**
- since the containers share the same IP address, they communicate over localhost.

**Question: how do containers inside a Pod share storage??**
- through a common filesystem

- NOTE: Pods are not permanent. a Pod scales as a unit thus containers inside it cannot be scaled independently. 

**Question: what is a sidecar container??**
- it is a container that provides supporting functionality to the primary application. 

- `initContainers` - used to run certain processes before the main application starts. the containers will run sequentially and must complete successfully before the main application containers begin. 

**Question: what settings are used to customise containers inside Pods??**
1. resources- define the CPU and memory requests and limits, which ensures proper scheduling and prevents a container from consuming more resources than allowed.
2. livenessProbe - configures health checks to verify that the application is still running; if a probe fails, Kubernetes can automatically restart the container.
3. securityContext - specifies security settings such as user and group IDs, filesystem permissions, and kernel capabilities. 

---

## Pod Lifecycle

**Question: describe the lifecycle phases of the Pods??**
1. pending - phase which includes time spend waiting for scheduling and downloading container images.
2. running - the Pod is now assigned to a node, and all of its containers have been created. at least one container is still running or is in the process of starting or restarting.
3. succeeded - all containers in the pod have exited successfully and will not be restarted
4. failed - all containers have stopped, and at least one exited with a non-zero exit code or was terminated by the system.
5. unknown - the pod's state cannot be retrieved, typically due to communication issues with the node where the pod is scheduled.


- NOTE: a Pod typically starts in the **Pending** phase, transitions to **Running** once its containers start successfully, and eventually ends in either **Succeeded or Failed**, depending on how its containers terminate.

---