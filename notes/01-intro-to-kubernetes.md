# Introduction to Kubernetes

**Question: what is Kubernetes??**
- Kubernetes is an open-source system for automating deployment, scaling, and management of containerized applications.

---

**Question: list the features of Kubernetes??**
1. automatic bin packing
    - Kubernetes automatically schedule containers based on resource needs and constraints to maximise utilisation without sacrificing availability.

2. designed for extensibility
    - Kubernetes cluster can be extended with new custom features without modifying the upstream source code.
    
3. self-healing
    - Kubernetes automatically replaces and reschedules containers from failed nodes.
    - terminates and restarts containers that become unresponsive to health checks, based on existing rules/policy.
    - also prevents traffic from being routed to unresponsive containers.

4. horizontal scaling
    - Kubernetes scales applications manually or automatically based on CPU or custom metrics utilisation. 

5. service discovery and load balancing
    - containers receive IP addresses from Kubernetes, while it assigns a single Domain Name System (DNS) name to a set of containers to aid in load-balancing requests across containers of the set.

6. automated rollouts and rollbacks
    - Kubernetes seamlessly roll out and roll back application updates and configuration change, constantly monitoring the application's health to prevent any downtime.

7. secret and configuration management
    - Kubernetes manages sensitive data and configuration details for an application separately from the container image, to avoid a rebuild of the respective image. 

8. storage orchestration
    - Kubernetes automatically mounts software-defined storage (SDS) solutions to containers from local storage, external cloud providers, distributed storage or network storage systems.

9. batch execution
    - Kubernetes supports batch execution, long-running jobs, and replaces failed containers.

10. IPv4 / IPv6 dual-stack
    - Kubernetes supports both IPv4 and IPv6 addresses



Note: Kubernetes support common Platform-as-a-Service specific features such as **application deploymemnt, scaling, and load balancing**, but allows users to integrate their designed monitoring, logging, and alerting solutions through plugins. 

---

**Question: what are the advantages of Kubernetes??**
1. portability - ability to be deployable in various environments.

2. extensibility - can be supported by many 3rd party open source tools. these provide feature-rich experience for the users.

3. modular and pluggable architecture - follows decoupled microservice patterns which allows for its functionality to be extended through writing custom resources, operators, custom APIs, scheduling rules or plugins etc

4. supports various real-world systems in industries such as banking, education, finamce, gaming, information tech etc.


---

**Question: what is the purpose Cloud Native Computing Foundation (CNCF)??**

- Cloud Native Computing Foundation is an open-source vendor-neutral organization dedicated to making cloud-native computing ubiquitous.
    - fosters open-source projects
    - prevents vendor lock-in
    - standardises the industry
    - drives the cybersecurity standards
