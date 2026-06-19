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

## Workload Objects

- to ensure that a specific number of Pod replicas are always running, Kubernetes uses **controller objects** to manage Pods on its behalf.

**Workload controllers**
1. ReplicaSet & ReplicaSets - handles stateless, scalable applications
2. StatefulSets - manage persistent ordered workloads
3. DaemonSets - ensure node-level services
4. Jobs & CronJobs - automate finite or periodic tasks together forming foundation of reliable, automated workload orchestration in Kubernetes.


## Kubernetes Objects

1. ReplicaSet

**Question: what is a ReplicaSet??**
- it is a Kubernetes controller that maintains a stable set of identical Pods at any given time.
- primary function = ensuring that specified number of Pod replicas defined in the `replicas` field are always active. 

- ideal for **high availability and horizontal scaling of stateless workloads**. 
- NB: managed indirectly through higher-level controllers like Deployments (which handle versioning and updates).

2. Deployment

**Question: what is the functionality of Deployment??**
- it manages the full lifecycle or stateless applications, providing declarative updates to Pods and ReplicaSets
- it automatically creates a new ReplicaSet when an applicatio is updated. also replaces old Pods with new ones while **ensuring zero downtime**.
- also, rolls back to previous stable versions if errors are introduced during updates.
    - NB: very ideal for microservices, APIs, and web applications that require seamless updates,scaling, and rollback.

3. StatefulSet

**Question: what is the purpose of StatefulSet??**
- it is designed for managing stateful applications that require stable identities, persistent storage, and predictable Pod ordering.
-  also assigns each Pod a unique, persistent identifier such as `web-0` and ensures that the same identity is retained even if the Pod is rescheduled. why?? allow applications such as databases, message queues, and distributed storage systems to maintain consistent data relationships. 
- works closely with PersistentVolumeClaims (PVCs) to provide durable storage that persists across Pod restarts.
- scaling and updates occur sequentially ensuring orderly startup, shutdown, and version transitions, crucial for workloads like PostgreSQL, Cassandra, or Kafka.

4. DaemonSet

**Question: what is the functionality of DaemonSet??**
- it ensures that a specific Pod runs on every node (or on a subset of nodes labeled with a particular label) in a Kubernetes cluster.
- as new nodes are added, the DaemonSet automatically schedules the required Pods onto them; when nodes are removed, the Pods are cleaned up
- this controller is ideal for infra-level workloads that must run globally such as log collection agents e.g., Fluentd or Filebeat, monitoring tools e.g, Prometheus Node Exporter, or networking components e.g., Cilium or Calico. 
- NB: DaemonSet guarantee consistent background services across the cluster, ensuring that each node participates in monitoring, security, or data collection uniformly. 

5. Job

- Kubernetee controller designed to run finite, one-time tasks to completion. 
    - creates one or more Pods to perform a specific operation such as data processing, database migration or maintainance scripts, and ensures that tasks run successfully at least once.
    - when the Pod complete successfully, the Job is marked as finished and doesn't restart unless configured to retry on failure.
- jobs are useful for batch operations, where the taks is not long-running but still requires reliability and completion tracking

6. CronJob

- extend the Job concept by adding time-based scheduling, which is similar to Linux cron utility.
    - allows administrators and developers to run Jobs periodically according to a defined schedule
- CronJobs are ideal for recurring tasks such as automated backups, report generation, data synchronization or log cleanup.
- each scheduled execution creates a new job, which in turn manages the Pods responsible for completing the task.
- NB: Kubernetes ensures that missed executions due to downtime are handled gracefully, making CronJobs an essential component for time-sensitive automation. 

---

## Networking Objects

**Question: what is a Service??**
- a Service exposes one or more Pods as a network endpoint.

- types:

1. ClusterIP - is the default service which creates a virtual IP inside the cluster that acts as a single connection point for a group of Pods, often used for internal round-robin load balancing. 

2. NodePort - allows basic external traffic to reach the cluster through any node. the NodePort service type builds on ClusterIP by opening port on every node and mapping it to the Service.

3. LoadBalancer - service type that extends NodePort by provisioning an external load balancer. OR  a Service type that exposes an application externally and distributes incoming traffic across the Pods behind the Service.

4. ExternalName - is a special Service type that has no routing whatsoever. it creates a DNS alias rather than routing traffic. the Service maps a name inside the cluster to an external hostname, which is useful when internal workloads need to access external services without handling complex DNS names.


**Question: what is a headless Service??**
- a headless Service allows applications to handle services discovery independently rather than relying on Kubernetes' built-in mechanisms

**Question: how do we create a headless Service??**
- setting `.spec.clusterIP = None`


**Ingress**
- an Ingress is a Kubernetes API object that manages **HTTP and HTTPS traffic into the cluster**, providing **Layer 7 (application layer) routing** to Services. 
- use of routing rules which are enforced by an Ingress Controller, which implements the actaul networking logic.

**Question: what are the features of ingress controller??**
- LoadBalancing
- TLS offloading or termination
- Name-based virtual hosting
- Path-based routing

---

## Kubernetes Service Exposure: Ingress API vs Gateway API


**Question: what is the difference between Ingress API and Gateway API??**
- Ingress API provides simple HTTP/HPTTPS routing through a single resource, whereas Gateway API provides a richer, more extensible framework with separate resources for gateway and routes, enabling advanced traffic management and clear separation of responsibilities.

---

## NetworkPolicy

- NetworkPolicy i a simple IP firewall (OSI Layer 3 or 4) that control traffic based on rules

**Question: what is a Kubernetes NetworkPolicy??**
- it is a declarative resource that defines how Pods are allowed to communicate with each other and with external endpoints inside and outside a cluster.
    - acts as a pod-level firewall, enforcing fine-grained network access control in line with zero-trust security principles. 

**Question: why do we have to implement NetworkPolicy??**
- provides benefits such as least-priviledge, networking, and also limits the attack surface
- also make it possible to secure multi-tenant or production-grade Kubernetes clusters thereby promoting in-depth defense and compliance

---

## Volume & Storage Objects

- volumes allow data to be shared both between multiple containers in the same Pod and depending on configuration, between Pod across the cluster.
- volumes help prevent data loss when a Pod restarts on the same node.

**Question: what is Container Storage Interface (CSI) ??**
- allows vendors to supply storage drivers as plugins

1. PersistentVolumes (PV)
- represents a piece of storage in the cluster that has been provisioned by an administrator (static provisioning) or dynamically through a StorageClass (an API object that lets administrators define and abstract the underlying storage provided to an application). 
- key properties of PV object:
    - capacity e.g., 10Gi
    - access modes RWO, ROX, RWX
    - volume mode
    - reclaim policy i.e., retain, delete, and recycle
    - storage backend information
    - node affinity

- a PersistentVolume's lifecycle is independent of any individual Pod that uses it i.e., the data stored persists even if the Pods using it gets deleted or rescheduled e.g., PV (NFS-based).

2. PersistentVolumeClaims (PVC)
- is a user's request for storage. it is similar to how a Pod requests CPU and memory.
- it allows developers to specify the amount of storage, access mode, and optionally StorageClass that describe the type or performance tier of the desired storage.
- how it works??
    - when a PVC is created, Kubernetes looks for an available PV that matches its request (based on size, access mode, and storage class). if such a PV exists, it is bound to the new PVC. if not, and if a StorageClass is defined, Kubernetes dynamically provisions a new PV that satisfies the claim. once bound, the PV is dedicated exclusively to that PVC until it is released.

**Question: what is a StorageClass??**
- StorageClass defines how dynamic volume provisioning works within a cluster.

- allows us to automatically provision PVs on demand when a PVC is created. benefits?? scalability and reduction in manual intervention.

***Question: list the details included in each StorageClass??**
1. provisioner - which is the driver or plugin that actually creates the volumes
2. parameters - which are key-value pairs that define characteristics such as storage type, performance level or filesystem.
3. reclaim policy - determines what happens to a volume after the PVC is deleted [retain, delete, or recycle]
4. volume binding mode - controls when the volume is provisioned and bound ( `Immediate` or `WaitForFirstConsumer`). 


**Question: describe the dynamic provisioning flow??**
1. user creates a StorageClass (defines "how" to provision storage)
2. user creates a PVC referencing that StorageClass
3. Kubernetes dynamically provisions a PV using the parameters and binds it to the PVC
4. the Pod mounts the PVC and uses the volume
5. when the PVC is deleted, the PV is handled according to the reclaim policy.

---

## Configuration Objects

**Question: what are configuration objects??**
- configuration objects are Kubernetes resources used to store configuration data separately from the application code or container image. 

**Question: what are ConfigMaps??**
- ConfigMaps let's store configuration as key-value pairs or full configuration files independent of the Pods.

**Question: what are the two ways of consuming ConfigMaps??**
1. mounting as files inside a Pod using a volume
2. mapping to environment variables that the container can read at run time

**Question: what are Secrets??**
- these are built-in objects for storing sensitive values such as passwords, API keys, and authentication credentials
    - secrets are stored in an encoded format to prevent accidental exposure.

- examples of secret-management systems:
    - HashiCorp Vault, AWS Secret Manager, and Google Secret Manager

---

## Autoscaling Objects

**Question: what are the mechanisms supported by Kubernetes for auto-scaling workloads??**
1. horizontal Pod autoscaler (HPA)
2. cluster autoscaler
3. vertical Pod autoscaler
