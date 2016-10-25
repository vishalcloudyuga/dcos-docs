---
post_title: DC/OS Architecture Concepts
nav_title: Concepts
menu_order: 5
---
This page contains terms and definitions for DC/OS.

Because several of the core DC/OS components predate and continue to be developed independently of DC/OS itself, the pursuit of consistent terminology is an ongoing process. Concepts in DC/OS sometimes match the underlying component terminology directly and sometimes require mapping to alternate terms. Some terms are mature and well entrenched while others are newer and may not be thoroughly adopted and integrated. This document is designed both to help educate and to document intended direction for the near future.

## <a name="dcos-concepts"></a>DC/OS Concepts

DC/OS is an abstraction around pre-existing components. These terms are sometimes similar to component terms and sometimes they’re new terms meant to hide organically grown component terms.

### <a name="cluster"></a>Cluster

A DC/OS cluster is a set of networked DC/OS nodes with a quorum of master nodes and any number of public and/or private agent nodes.

### <a name="network"></a>Network

DC/OS has two types of networks: infrastructure networks and virtual networks.

#### <a name="infrastructure-network"></a>Infrastructure Network

An infrastructure network is a physical or virtual network provided by the infrastructure on which DC/OS runs. DC/OS does not manage or control this networking layer, but requires it to exist in order for DC/OS nodes to communicate.

#### <a name="virtual-network"></a>Virtual Network

A DC/OS virtual network is specifically an overlay network internal to the cluster that connects DC/OS components and containerized tasks running on DC/OS.

- The overlay network provided by DC/OS is VXLAN managed by Navstar.
- Virtual networks must be configured by an administrator before being used by tasks
- Tasks on DC/OS may opt-in to being placed on a specific virtual network and given a container-specific IP.
- Virtual networks allow logical subdivision of the tasks running on DC/OS
- Each task on a virtual network may be configured with optional address groups that virtually isolate communication to tasks on the same network and address group.

### <a name="node"></a>Node

A DC/OS node is a virtual or physical machine on which a Mesos agent and/or Mesos master process runs. DC/OS nodes are networked together to form a DC/OS cluster.

#### <a name="master-node"></a>Master Node

A DC/OS master node is a virtual or physical machine that runs a collection of DC/OS components that work together to manage the rest of the cluster.

- Each master node contains (among other things) a Mesos master process.
- Master nodes work in a quorum to provide consistency of cluster coordination. Three master nodes allows one to be down. Five master nodes allows two to be down (e.g. to allow for failure during a rolling update).
- A cluster with only one master node is usable for development, but is not highly available and may not be able to recover from failure.

#### <a name="agent-node"></a>Agent Node

A DC/OS agent node is a virtual or physical machine on which Mesos tasks are run.

- Each agent node contains (among other things) a Mesos agent process.
- Agent nodes may be [private](#private-agent-node) or [public](#public-agent-node), depending on agent and network configuration.
- For more information, see [Network Security](/docs/1.8/administration/securing-your-cluster/) and [Adding Agent Nodes](/docs/1.8/administration/installing/custom/add-a-node/).

##### <a name="private-agent-node"></a>Private Agent Node

A private agent node is an agent node that is on a network that** ***does not* have ingress access from outside of the cluster via the cluster’s infrastructure networking.

- The Mesos agent on each private agent node is (by default) configured with none of its resources allocated to any specific Mesos roles (`*`).
- Most service packages install by default on private agent nodes.
- Clusters are usually made up of mostly private agent nodes.

##### <a name="public-agent-node"></a>Public Agent Node

A public agent node is an agent node that is on a network that *does* have ingress access from outside of the cluster via the cluster’s infrastructure networking.

- The Mesos agent on each public agent node is configured with the "public_ip:true" attribute and all of its resources allocated to the `slave_public` role (`agent_public` in the future).
- Public agent nodes are used primarily for externally facing reverse proxy load balancers, like Marathon-LB.
- Clusters usually only have a few public agent nodes, because a single load balancer can handle proxying multiple services.
- For more information, see [Converting Agent Node Types](/docs/1.8/administration/installing/custom/convert-agent-type/).

### <a name="bootstrap-machine"></a>Bootstrap Machine

A bootstrap machine is the machine on which the DC/OS installer artifacts are configured, built, and distributed.

- The bootstrap machine is not technically considered part of the cluster, in that it does not have DC/OS installed on it (this may change in the future), but it does need to be accessible to/from (depending on install method) the machines in the cluster via infrastructure networking in order to perform remote installation.
- The bootstrap machine is sometimes used as a jumpbox to control SSH access into other nodes in the cluster for added security/logging.
- One method of allowing master nodes to change IPs involves running Exhibitor/ZooKeeper on the bootstrap machine. Other alternatives include using S3, DNS, or static IPs, with various tradeoffs.
- If not required for managing master node IP changes or as an SSH jumpbox, the bootstrap machine may be shut down after bootstrapping and spun up on demand when new nodes need to be added to the cluster.
- For more information, see the [system requirements](/docs/1.8/administration/installing/custom/system-requirements/#bootstrap-node).

### <a name="service"></a>Service

A DC/OS service is a set of one or more service instances that can be started and stopped as a group and restarted automatically if they exit before being stopped.

- Services is currently just a UI abstraction that translates to Marathon apps and pods in the CLI and API. This distinction will change over time as the name "service" is pushed upstream into component APIs.
- Sometimes "service" may also refer to a systemd service on the host OS. These are generally considered components and don’t actually run on Marathon or Mesos.
- A service may be either a system service or a user service. This distinction is new and still evolving as namespacing is transformed into a system-wide first class pattern.

#### <a name="marathon-service"></a>Marathon Service

A Marathon service consists of zero or more containerized service instances. Each service instance consists of one or more containerized Mesos tasks.

- Marathon apps and pods are both considered services.
    - Marathon app instances map 1 to 1 with tasks
    - Marathon pod instances map 1 to many with tasks
- Service instances are restarted as a new Mesos Task when they exit prematurely.
- Service instances may be re-scheduled onto another agent node if they exit prematurely and the agent is down or does not have enough resources any more.
- Services may be installed directly via the DC/OS API (Marathon) or indirectly via the DC/OS package manager (Cosmos) from a package repository (e.g. [Mesosphere Universe](#mesosphere-universe))
- A Marathon service may be a DC/OS scheduler, but not all services are schedulers.
- A Marathon service is an abstraction around Marathon service instances which are an abstraction around Mesos tasks. Other schedulers (e.g. Metronome, Jenkins) have their own names for abstractions around Mesos tasks.
- Examples: Cassandra (scheduler), Marathon-on-Marathon, Kafka (scheduler), Nginx, Tweeter

#### <a name="systemd-service"></a>Systemd Service

A systemd service is a service that consists of a single (optionally containerized) machine operating system process, running on the master or agent nodes, managed by systemd, owned by DC/OS itself.
- All systemd service are currently either host OS service, DC/OS dependencies, DC/OS components, or services manually managed by the system administrator.
- Examples: Most DC/OS components, (system) Marathon

#### <a name="system-service"></a>System Service

A system service is a service that implements or enhances the functionality of DC/OS itself, run as either a Marathon service or a systemd service, owned by the system (admin) user or DC/OS itself.
- A system service may require special permissions to interact with other system services.
- Permission to operate as a system service on an Enterprise DC/OS cluster requires specific fine-grained permissions, while on open DC/OS all logged in users have the same administrative permissions.
- Examples: All DC/OS components

#### <a name="user-service"></a>User Service

A user service is a Marathon service that is not a system service, owned by a user of the system.

- This distinction is new and still evolving as namespacing is transformed into a system-wide first class pattern and mapped to fine-grained user and user group permissions.
- Examples: Jenkins, Cassandra, Kafka, Tweeter

### <a name="service-group"></a>Service Group

A DC/OS service group is a hierarchical (path-like) set of DC/OS services for namespacing and organization.

- Service groups are currently only available for Marathon services, not systemd services.
- This distinction may change as namespacing is transformed into a system-wide first class pattern.

### <a name="job"></a>Job

A DC/OS job is a set of similar short-lived job instances, running as Mesos tasks, managed by the Jobs service scheduler (Metronome)

- A job can be created to run only once, or may run regularly on a schedule.

### <a name="scheduler"></a>Scheduler

A DC/OS scheduler is a Mesos scheduler that runs as a systemd service (on master nodes) or Mesos task (on agent nodes).

- The key differences between a DC/OS scheduler and Mesos scheduler are where it runs and how it is installed.
- Some schedulers come pre-installed as DC/OS components (e.g. Marathon, Metronome).
- Some schedulers can be installed by users as user services (e.g Kafka, Cassandra)
- Some schedulers run as multiple service instances to provide high availability (e.g. Marathon).
- In certain security modes within Enterprise DC/OS, a DC/OS scheduler must authenticate and be authorized using a service account in order to register with Mesos as a framework.

### <a name="scheduler-service"></a>Scheduler Service

A DC/OS scheduler service is a long-running DC/OS scheduler that runs as a DC/OS service (Marathon or systemd).

- Since DC/OS schedulers can also be run as short-lived tasks, not all schedulers are services.

### <a name="component"></a>Component

A DC/OS component is a DC/OS system service that is distributed with DC/OS.

- Components may be systemd services or Marathon services
- Components may be deployed in a high availability configuration
- Most components run on the master nodes, but some (e.g. mesos-agent) run on the agent nodes
- Examples: Mesos, Marathon, Mesos-DNS, Bouncer, Admin Router, Cosmos, Minuteman, History Service, etc.

### <a name="package"></a>Package

A DC/OS package is a bundle of metadata that describe how to configure and install one or more Marathon services.

- Packages can be installed and uninstalled

### <a name="package-registry"></a>Package Registry

A package registry is a repository of package metadata.

- DC/OS (Cosmos) may be configured to install packages from one or more package registries.

### <a name="mesosphere-universe"></a>Mesosphere Universe

The Mesosphere Universe is a public package registry, managed by Mesosphere. For more information, see the [Universe repository](https://github.com/mesosphere/universe) on GitHub.

### <a name="cloud-template"></a>Cloud Template

A cloud template is an infrastructure-specific method of decoratively describing a DC/OS cluster. For more information, see [Cloud Installation Options](/docs/1.8/administration/installing/cloud/).

## <a name="mesos-concepts"></a>Mesos Concepts

The following terms are contextually correct when talking about Mesos, but may be hidden by other abstraction within DC/OS.

### <a name="mesos-master"></a>Master

A Mesos master is a process that runs on master nodes to coordinate cluster resource management and facilitate orchestration of tasks.

- The Mesos masters form a quorum and elect a leader
- The lead Mesos master collects resources reported by Mesos agents and makes resource offers to Mesos schedulers. Schedulers then may accept resource offers and place tasks on their corresponding nodes.

### <a name="mesos-agent"></a>Agent

A Mesos agent is a process that run on agent nodes to manage the executors, tasks, and resources of that node.

- The Mesos agent registers some or all of the node’s resources, which allows the lead Mesos master to offer those resources to schedulers, which decide on which node to run tasks.
- The Mesos agent reports task status updates to the lead Mesos master, which in turn reports them to the appropriate scheduler.

### <a name="mesos-task"></a>Task

A Mesos task is an abstract unit of work, lifecycle managed by a Mesos executor, that runs on a DC/OS agent node.

- Tasks are often processes or threads, but could even just be inline code or items in a single-threaded queue, depending on how their executor is designed.
- The Mesos built-in command executor runs each task as a process that can be containerized by one of several containerizers (e.g. docker containerizer, mesos containerizer)

### <a name="mesos-executor"></a>Executor

A Mesos executor is a method by which Mesos agents launch tasks.

- Executor processes are launched and managed by Mesos agents on the agent nodes.
- Mesos tasks are defined by their scheduler to be run by a specific executor (or the default executor).
- Each executor runs in its own container.
- For more information about framework schedulers and executors, see the [Application Framework development guide](http://mesos.apache.org/documentation/latest/app-framework-development-guide/).

### <a name="mesos-scheduler"></a>Scheduler

A Mesos scheduler is a program that defines new Mesos tasks and assigns resources to them (placing them on specific nodes).

- A scheduler receives resource offers describing CPU, RAM, etc., and allocates them for discrete tasks that can be launched by Mesos agents.
- A scheduler must register with Mesos as a framework.
- Examples: Kafka, Marathon, Cassandra

### <a name="mesos-framework"></a>Framework

A Mesos framework consists of a scheduler, its tasks, and optionally custom executors.

- The term framework and scheduler are sometimes used interchangeably. Prefer scheduler within the context of DC/OS.
- For more information about framework schedulers and executors, see the [Application Framework development guide](http://mesos.apache.org/documentation/latest/app-framework-development-guide/).

### <a name="mesos-role"></a>Role

A Mesos role is a group of Mesos Frameworks that share reserved resources, persistent volumes, and quota. These frameworks are also grouped together in Mesos' hierarchical Dominant Resource Fairness (DRF) share calculations.

- Roles are often confused as groups of resources, because of they way they can be statically configured on the agents. The assignment is actually the inverse: resources are assigned to roles.
- Role resource allocation can be configured statically on the Mesos agent or changed at runtime using the Mesos API.

### <a name="resource-offer"></a>Resource Offer

A Mesos resource offer provides a set of unallocated resources (e.g. cpu, disk, memory) from an agent to a scheduler so that the scheduler may allocate those resources to one or more tasks. Resource offers are constructed by the leading Mesos master, but the resources themselves are reported by the individual agents.

### <a name="mesos-containerizer"></a>Containerizer

A Mesos containerizer is a containerization and resource isolation abstraction around a specific container runtime, namely the Mesos Universal Container Runtime and the Docker Runtime

#### <a name="mesos-universal-container-runtime"></a>Mesos Universal Container Runtime

The Mesos Universal Container Runtime is a containerizer that supports traditional Mesos containers around binary executables and also Mesos containers launched from Docker images. Containers managed by the Mesos Universal Container Runtime do not use [Docker-Engine](https://www.docker.com/products/docker-engine), even if launched from a Docker image.

#### <a name="mesos-docker-runtime"></a>Docker Runtime

The Docker Runtime is a containerizer that supports launching Docker containers from Docker images with [Docker-Engine](https://www.docker.com/products/docker-engine).

### <a name="mesos-exhibitor-zookeeper"></a>Exhibitor &amp; Zookeeper

Mesos depends on ZooKeeper, a high-performance coordination service to manage the cluster state. Exhibitor automatically configures and manages ZooKeeper on the [master nodes](#master-node).

### <a name="mesos-exhibitor-zookeeper"></a>Mesos-DNS

Mesos-DNS is a DC/OS component that provides service discovery within the cluster. Mesos-DNS allows applications and services that are running on Mesos to find each other by using the domain name system (DNS), similar to how services discover each other throughout the Internet.

- For more information, see the [Mesos-DNS repository](https://github.com/mesosphere/mesos-dns) on GitHub.

## <a name="marathon-concepts"></a>Marathon Concepts

The following terms are contextually correct when talking about Marathon, but may be hidden by other abstraction within DC/OS.

### <a name="marathon-application"></a>Application

A Marathon application is a long-running service that may have one or more instances that map one to one with Mesos tasks.

- The user creates an application by providing Marathon with an application definition (json). Marathon then schedules one or more application instances as Mesos tasks, depending on how many the definition specified.
- Applications currently support the use of either the [Mesos Universal Container Runtime](#mesos-universal-container-runtime) or the [Docker Runtime](#mesos-docker-runtime).

### <a name="marathon-pod"></a>Pod

A Marathon pod is a long-running service that may have one or more instances that map one to many with colocated Mesos tasks.

- The user creates a pod by providing Marathon with a pod definition (json). Marathon then schedules one or more pod instances as Mesos tasks, depending on how many the definition specified.
- Pod instances may include one or more tasks that share certain resources (e.g. IPs, ports, volumes)
- Pods currently require the use of the [Mesos Universal Container Runtime](#mesos-universal-container-runtime).

### <a name="marathon-group"></a>Group

A Marathon group is a hierarchical (path-like) set of services (applications and/or pods) for namespacing and organization.
