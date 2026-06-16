# Cloud Native Architure

## Cloud Native Architecture Fundamentals

**Question: what is cloud native architecture??**
- Cloud native archicture is an approach to building, deploying, and managing applications that takes full advantage of cloud computing characteristics such as scalability, resilience, automation, and elasticity. it commonly uses microservices, containers, Kubernetes, declarative APIs, and DevOps practices.

**Question: what are the main goals of cloud native architecture??**
- cost efficiency
- reliability
- scalability
- faster time-to-market
- agility and rapid innovation

**Question: why do organizations prefer cloud native architecture over traditional architecture??**
- traditional architecture becomes difficult to scale, maintain, and update as applications grow. on the other hand, cloud native architectures enable:
    - faster deployments
    - better scalability
    - improved resilience
    - independent development by teams
    - more efficient use of cloud resources

**Question: what does "cloud native" mean??**
- cloud native means designing applications specifically for cloud environments rather than simply hosting traditional applications in the cloud. 

---

## Traditional Monolithic Architecture

**Question: what is a monolithic application??**
- a monolithic application is an application where all functionality exists within a single codebase and is deployed as one unit. 

**Question: describe the structure of monolithic application??**
- user interface (UI), business logic, and data access layer.

**Question: what challenges arise as a monolithic application grows??**
- increasing code complexity
- slower development
- difficult collaboration across teams
- longer deployment cycles
- scaling inefficiencies
- greater risk when making changes

---

## Microservices Architecture

**Question: what is the core idea behind microservices??**
- breaking a large application into smaller, independent services that each perform specific business function and communicate over a network. 

**Question: what is a microservice??**
- a small, independent application focused on a single business capability that can be developed, deployed, and scaled independently.

**Question: what are some of the benefits that microservice architecture offers??**
- independent scaling
- faster deployment per service
- better fault isolation
- team autonomy


**Question: what is the key difference between monolithic and microservice architecture??**

| Monolithic   | Microservices  |
|    ---       |    ---         |
| single deployable unit | multiple deployable services |
| single codebase  | multiple codebases |
| tightly coupled | loosely coupled |
| scaled as one unit | scaled independently |
| one failure can affect entire application | failures can be isolated |


**Question: how does data management differ between monoliths and microservices??**
- monoliths typically use a single shared database, while microservices often have their own dedicated databases or storage systems.


**Question: why is networking more important in microservices??**
- services communicate over the network rather than through local function calls, introducing latency and potential communication failures.

**Question: what additional capabilities are required to successfully run microservices??**
- strong DevOps practices
- observability
- monitoring
- logging
- distributed tracing
- automation

**Question: a company's checkout service receives heavy traffic during sales events. why is a microservice architecture advantageous??**
- the checkout services can be scaled independently without scaling the entire application, reducing infrastructure costs and improving efficiency.

**Question: a company wants multiple teams to release features independently. which architecture is more suitable and why??**
- microservice architecture because teams can own and deploy their services independently.

---

## Characteristics of Cloud Native Architecture

- cloud-native architecture is designed to make applications **faster to develop, easier to operate, more resilient, and more cost-effective**.


**1. High Level of Automation**
- in cloud-native systems, most operational tasks are automated instead of being performed manually. this means:
    - infrastructure is defined in code
    - deployments happen automatically
    - scaling happens automatically
    - recovery procedures can be automated
- here everything becomes repeatable, predictable, and reproducible.
- a reliable automated system also **allows for much easier disaster recovery** if we have to rebuild the whole system. 

The entire infrastructure is described as code e.g.,

```YAML
replicas: 3
image: nginx
```

- Kubernetes reads the configuration and creates the environment automatically.
- CI/CD pipeplines automate building, testing, and deploying e.g.,

```text
Developer pushes code
      ↓
Tests run automatically
      ↓
Application deploys automatically
```

- infrastructure is also written as code using tools such as Terraform, CloudFormation, Pulumi etc. 
- example:

```text
Instead of manually creating servers,
you write code that creates servers.
```

- we declare the desired state in our config files for example specifying `replicas: 3`.
- why is all this important???
    - if the whole system disappears infra code, application code, and config files can rebuild everything automatically. _this makes disaster recovery much easier_.

Question: what does "high level of automation" mean??
- operational tasks such as provisioning infra, deployments, scaling, and recovery are performed automatically rather than manually.

Question: why is manual infrastructure management discouraged??
- it is slow, error-prone, difficult to reproduce, and doesn't scale well.

Question: what is Infrastructure as Code (IaC)??
- the practice of defining and managing infrastructure through code rather than manual configuration e.g., Terraform, AWS, CloudFormation, Pulumi etc

Question: what is a declarative configuration??
- a configuration that describes the desired end state rather than the steps needed to achieve it.


**2. Self-Healing**
- cloud-native systems assume "failures WILL happen" and automatically recover from them. for example: a pod crashes and Kubernetes detects

```text
Desired state = 3 pods
Current state = 2 pods
```

- Kubernetes immediately creates a replacement pod.
- applications continuously report their health. Kubernetes uses:
    - liveness probe
    - readiness probe
- why self-healing matters??
    - with self-healing:

```text
Application crashes
↓
Platform detects failure
↓
Replacement starts automatically
```

Question: what is a liveness probe??
    - a health check that determines whether a container is alive.
        -  failure result in container restarting.

Question: what is a readiness probe??
    - a health check that determines whether a container can receive traffic
        - failure results in removing from service endpoints until healthy


**3. Scalability**
- scaling means handling more workload while maintaining performance e.g.,

```text
100 users → application works
10,000 users → application still works
```

- _horizontal scaling -is a technique where a system's capacity is increased by adding more nodes versus adding more compute resources to individual nodes_. [adding more instances (pods/servers)]
- instead of making one server bigger like 1 server with let's say 4 CPUs, we create more copies 4, 5, 10 pods etc where traffic is distributed across them.

Question: CPU > 80%, more pods created. what is the process??
- autoscaling

Question: server upgraded from 4 CPUs to 16 CPUs. which scaling??
- vertical scaling

**4. Cost Efficiency**
- cloud-native systems try to use only the resources they actually need. thus during high traffic more resources are used and during low traffic unused resources are removed "scaling down".

**5. Easy to Maintain**
-  use of microservice architecture which allows easier testing, deployment, team ownership, and updates etc.

**6. Secure by Default**


**Twelve-Factor App** - is a set of best practices for building cloud-native applications.
- helps promote:
    - version-controlled code
    - configuration through environment variables
    - stateless applications
    - scalability
    - portability
    - automation

**Question: a cloud-native application automatically deploys through CI/CD, scales based on CPU usage, recreates failed pods, and requires authentication between services. which cloud-native characteristics are present??**
- automation
- scalability
- self-healing
- secure by default

---

## Autoscaling

**Question: what is a autoscaling pattern??
- autoscaling pattern describes the dynamic adjustment of resources in response to current demand.
- two things to consider: CPU and memory usage

**Question: what is the difference between vertical scaling and horizontal scaling??**
- vertical scaling adds more resources (CPU, memory, storage) to a server to increase its capacity, while horizontal scaling adds multiple servers (Server A, B, C, D) to distribute workload and improve performance across systems.

**Question: how is autoscaling configured??**
- by running many near-production load tests and analyse the behaviour and load balancing as the application scales.

---

## Serverless

**Question: what is serverless computing??**
- serverless computing is an application development model that allows developers to build, deploy and run applications without managing servers or back-end infrastructure.

**Questio: what is Function as a Service (FaaS)??**
- Function as a service (FaaS) is a cloud computing model where one develop small, modular pieces of code called functions, and a cloud provider executes them in response to specific events.

- container images are a great standardised way of packaging software for FaaS. 
- CloudEvents = specification for commonly describing event data.
    - OR an open-source specification created by the Cloud Native Computing Foundation (CNCF) designed to standardize how event data is described and transmitted across different cloud services, platforms, and programming languages
- NB: events are the basis for scaling serverless workloads or triggering corresponding functions. 

**Question: what are the advantages of serverless computing??**
- lower cost
- simplified scalability
- simplified backend code
- quicker turnaround

**Question: how does serverless compare to other cloud backend models??**
- Backend-as-a-Service (BaaS) - service model where cloud provider offers backend services such as data storage, so that developers can focus on writing front-end code.
- Platform-as-a-Service (PaaS)- model where developers essentially rent all the necessary tools to develop and deploy applications from a cloud provider, including things like OS and middleware.
- Infrastructure-a-a-Service (IaaS) - catch all term for cloud vendors hosting infra on behalf of their customers. NB: IaaS providers might offer serverless functionality.

- serverless computing addresses backend services and handle server-side logic, data processing, and infra management that users don't directly use. 

**Question: what is edge computing, and how does it relate to serverless computing??**
- edge computing in serverless means that functions can be deployed closer to end users, reducing latency and improving application performance. 

**Question: what is cold start mitigation in serverless platforms??**
- cold start mitigation refers to techniques that reduce the delay when starting dormant serverless functions.
- serverless providers with cold start mitigation features in place spin up functions at the edge in advance, helping to eliminate delays in execution. 

---

## Open Standards

- image-spec - defines how to build and package container images
- runtime-spec - specifies the configuration, execution environment, and lifecycle of containers.
- distribution-spec - provides a standard for distributing content, including container images. 

1. OCI Spec - image, runtime, distribution specification on how to run, build, and distribute containers

2. Container Network Interface (CNI) - specification on how to implement networking for Linux containers. defines how container runtimes and orchestrators (such as Kubernetes) should assign IP addresses, connect containers to networks, and remove network configurations when containers terminate.

3. Container Runtime Interface (CRI) - is a Kubernetes API that allows Kubernetes to communicate with different container runtimes e.g., containerd, CRI-O.

4. Container Storage Interface (CSI) - is a standard for exposing storage systems to container orchestration platforms. OR specification for integrating storage systems with container orchestration platforms such as Kubernetes. 

5. Service Mesh Interface (SMI) - specification that provides a standard interface for service mesh capabilities. focuses on traffic policies, traffic splitting, access control, observability. 
    - specification that standardises how service meshes expose common functionality to Kubernetes users and tools. 

**Question: which specification handles container networking??**
- container network interface (CNI)

**Question: which specification allows Kubernetes to work with different runtimes??**
- container runtime interface (CRI)

**Question: which specification allows Kubernetes to work with different storage systems??**
- container storage interface (CSI)

**Question: which specification standardizes container images and runtimes??**
- OCI

**Question: which specification standardizes service mesh capabilities??**
- service mesh interfaces (SMI)

---

## Cloud Native Roles & Site Reliability Engineering

- Site Reliability Engineering uses three main metrics:

1. **Service Level Objectives (SLO)** - specify a target level for the reliability of the service e.g., reaching a service latency of less than 100ms.

2. **Service Level Indicators (SLI)** - carefully defined quantitative measure of some aspect of level of service that is provided e.g., how long a request actually needs to be answered.

3. **Service Level Agreements (SLA)** - an explicit or implicit contract with the users that include consequences of meeting or missing the SLOs they contain. 