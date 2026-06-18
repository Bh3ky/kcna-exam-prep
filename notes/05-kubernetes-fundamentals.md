# Kubernetes Fundamentals

## Kubernetes Architecture

- Kubernetes is typically deployed as a cluster i.e., it runs across multiple servers that share workloads, improve reliability, and scale applications efficiently.
- Kubernetes has horizontal scaling capabilities which allows it to support clusters with thousands of nodes spread across data centers and even regions. 

**Question: what are the two server node types that make up a cluster in Kubernetes??**
1. control plane node(s) - which are nodes that act as the "brain" of the cluster. they run components that manage the overall system, including workload scheduling, deployments, self-healing behaviours.
2. worker nodes - responsible for running applications. they follow instructions from the control plane such as when to start or stop containers

- NOTE: just like in microservice architecture, Kubernetes incorporates multiple smaller services that need to be installed on the nodes.

**Question: explain the architecture of Kubernetes??**
- Kubernetes architecture is based on a distributed cluster model consisting of Control Plane and multiple Worker Nodes. the Control Plane acts as the brain of the cluster and is responsible for managing the desired state of the system, scheduling workloads, handling networking decisions, and maintaining cluster health. On the other hand, Worker Nodes are the machines that actually run containerized applications inside Pods.

**Question: what services makes up the control plane??**
1. 