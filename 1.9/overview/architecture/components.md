---
post_title: Components
nav_title: Components
menu_order: 4
---

DC/OS is composed of many open source microservice components meticulously tuned and configured to work together.

![DC/OS Components](/docs/1.9/overview/architecture/img/dcos-component-diagram.png)

From the top, DC/OS is a batteries-included container platform that handles container orchestration, package management, and security.

From the bottom, DC/OS is an operating system built on top of [Apache Mesos](http://mesos.apache.org/) that handles cluster management and software defined networking while simplifying logging and metrics collection.


## Cluster Management

DC/OS provides a way to view and operate a large number of individual machine-level systems as a single cluster-level system. It hides the complexity of Mesos, the distributed systems kernel, with higher level abstractions, interfaces, and tools. Cluster management is the core of that functionality, including the kernel, its dependencies, and its user interfaces.

### Apache Mesos

Mesos manages resources and tasks as a distributed systems kernel.

Mesos Master exposes scheduler, executor, and operator interfaces to facilitate cluster management.

Mesos Agent manages individual executors, tasks, and resources on each [DC/OS agent node](/docs/1.9/overview/concepts/#dcos-agent-node).

Mesos Agent Public is a Mesos Agent configured to run on [DC/OS public agent nodes](/docs/1.9/overview/concepts/#public-agent-node).

System Service(s): dcos-mesos-master.service, dcos-mesos-slave.service, dcos-mesos-slave-public.service

Dependencies: Zookeeper

[Docs](http://mesos.apache.org/), [Source](https://github.com/apache/mesos)

### Exhibitor &amp; Apache Zookeeper

Zookeeper stores cluster state.

Exhibitor manages Zookeeper and provides a management web interface.

System Service(s): dcos-exhibitor.service

Dependencies: N/A

[Exhibitor Source](https://github.com/Netflix/exhibitor), [Zookeeper Docs](https://zookeeper.apache.org/), [Zookeeper Source](https://github.com/apache/mesos)

### DC/OS Installer

The DC/OS Installer (dcos_generate_config.sh) generates install artifacts and installs DC/OS.

As part of the install process on each node, the DC/OS Download service downloads the install artifacts from the bootstrap machine and the DC/OS Setup service installs components using PkgPanda.

System Service(s): dcos-download.service, dcos-setup.service

Dependencies: N/A

[Docs](/docs/1.9/administration/installing/), [Source](https://github.com/dcos/dcos)

### DC/OS GUI

The DC/OS GUI (web interface) is a browser-based system dashboard and control center.

System Service(s): N/A - The GUI is served by Admin Router.

Dependencies: TODO

[Docs](/docs/1.9/usage/webinterface/), [Source](https://github.com/dcos/dcos-ui)

### DC/OS CLI

The DC/OS CLI is a terminal-based remote client.

System Service(s): N/A - The CLI is a user downloadable binary.

Dependencies: DC/OS

[Docs](/docs/1.9/usage/cli/), [Source](https://github.com/dcos/dcos-cli)


## Container Orchestration

While Mesos enables the utilization of custom schedulers, most containerized tasks have very similar scheduling and lifecycle management needs. So DC/OS includes a couple built-in schedulers to orchestrate the most common of these higher level abstractions: jobs and services.

### Marathon

Marathon orchestrates long-lived containerized services (apps &amp; pods).

System Service(s): dcos-marathon.service

Dependencies: TODO

[Docs](https://mesosphere.github.io/marathon/), [Source](https://github.com/mesosphere/marathon)

### Metronome

Metronome orchestrates short-lived, scheduled or immediate, containerized jobs.

System Service(s): dcos-metronome.service

Dependencies: TODO

[Docs](/docs/1.9/usage/jobs/), [Source](https://github.com/dcos/metronome)

### Docker GC

**NEW IN 1.9.0**

Docker GC garbage collects Docker containers and images.

System Service(s): dcos-docker-gc.service, dcos-docker-gc.timer

Dependencies: TODO

[Source](https://github.com/spotify/docker-gc)


## Logging &amp; Metrics

No software runs perfectly, especially not the first time. Distribute tasks across a cluster and the normal patterns of analyzing and debugging these services become tedious and painful. So DC/OS includes several components to help ease the pain of debugging distributed systems by aggregating, caching, and streaming logs, metrics, and cluster state metadata.

### 3DT

The DC/OS Distributed Diagnostics Tool (3DT) aggregates and exposes system component health.

API: `/system/health/v1/`

System Service(s): dcos-3dt.service, dcos-3dt.socket

Dependencies: TODO

[Source](https://github.com/dcos/3dt)

### Log Service

**NEW IN 1.9.0**

Log Service exposes component, container, and task logs.

System Service(s): dcos-log-master.service, dcos-log-master.socket, dcos-logrotate-master.service, dcos-logrotate-master.timer, dcos-log-agent.service, dcos-log-agent.socket

Dependencies: TODO

[Source](https://github.com/dcos/dcos-log)

### Logrotate

Logrotate manages rotation, compression, and deletion of historical log files.

[Docs](http://www.linuxcommand.org/man_pages/logrotate8.html), [Source](https://github.com/logrotate/logrotate)

### Metrics Service

**NEW IN 1.9.0**

Metrics Service exposes host, container, and task metrics.

System Service(s): dcos-metrics-master.service, dcos-metrics-master.socket, dcos-metrics-agent.service, dcos-metrics-agent.socket

Dependencies: TODO

[Source](https://github.com/dcos/dcos-metrics)

### Signal

Signal reports cluster telemetry and analytics to help improve DC/OS. Administrators can [opt-out of telemetry](/docs/1.9/administration/installing/opt-out/#telemetry) at install time.

System Service(s): dcos-signal.service, dcos-signal.timer

Dependencies: TODO

[Source](https://github.com/dcos/dcos-signal)

### History Service

History Service caches and exposes historical system state to facilitate cluster usage statistics in the GUI.

System Service(s): dcos-history.service

Dependencies: TODO

[Source](https://github.com/dcos/dcos/tree/master/packages/dcos-history/extra)


## Networking

In a world where machines are are given numbers instead of names, tasks are scheduled automatically, dependencies are declaratively defined, and services run in distributed sets, network administration also needs to be elevated from plugging in cables to configuring software-defined networks. To accomplish this, DC/OS includes a fleet of networking components for routing, proxying, name resolution, virtual IPs, load balancing, and distributed reconfiguration.

### Admin Router

Admin Router exposes a unified control plane proxy for components and services using [NGINX](https://www.nginx.com/).

Admin Router Agent proxies node-specific health, logs, metrics, and package management internal endpoints.

System Service(s): dcos-adminrouter.service, dcos-adminrouter-reload.service, dcos-adminrouter-reload.timer, dcos-adminrouter-agent.service, dcos-adminrouter-agent-reload.service, dcos-adminrouter-agent-reload.timer

Dependencies: TODO

[Source](https://github.com/dcos/adminrouter)

### Mesos-DNS

Mesos-DNS provides domain name based service discovery within the cluster.

System Service(s): dcos-mesos-dns.service

Dependencies: TODO

[Docs](http://mesosphere.github.io/mesos-dns/), [Source](https://github.com/mesosphere/mesos-dns)

### Spartan

Spartan forwards DNS requests to multiple DNS servers.

Spartan Watchdog restarts Spartan when it is unhealthy.

System Service(s): dcos-spartan.service, dcos-spartan-watchdog.service, dcos-spartan-watchdog.timer

Dependencies: TODO

[Source](https://github.com/dcos/spartan)

### Generate resolv.conf

Generate resolv.conf manages DNS resolution configuration in `/etc/resolv.conf` to facilitate DC/OS's software defined networking.

System Service(s): dcos-gen-resolvconf.service, dcos-gen-resolvconf.timer

Dependencies: TODO

[Source](https://github.com/dcos/dcos/blob/master/packages/spartan/extra/gen_resolvconf.py)

### Minuteman

Minuteman provides distributed [Layer 4](https://en.wikipedia.org/wiki/Transport_layer) virtual IP load balancing.

System Service(s): dcos-minuteman.service

Dependencies: TODO

[Docs](/docs/1.9/usage/service-discovery/load-balancing-vips/), [Source](https://github.com/dcos/minuteman)

### Navstar

Navstar orchestrates virtual overlay networks using [VXLAN](https://en.wikipedia.org/wiki/Virtual_Extensible_LAN).

System Service(s): dcos-navstar.service

Dependencies: TODO

[Source](https://github.com/dcos/navstar)

### Erlang Port Mapper

Erlang Port Mapper (EPMD) maps symbolic names to machine addresses, facilitating named virtual IPs.

System Service(s): dcos-epmd.service

Dependencies: TODO

[Source](https://github.com/erlang/epmd)


## Package Management

Just as machine operating systems need package management to install, upgrade, configure, and remove individual applications and services, a datacenter operating system needs package management to do the same for distributed services. In DC/OS there are two levels of package management: machine-level for components; and cluster-level for user services.

### Cosmos

Cosmos installs and manages DC/OS packages from [DC/OS package repositories](/docs/1.9/usage/repo/), such as [Mesosphere Universe](https://github.com/mesosphere/universe).

System Service(s): dcos-cosmos.service

Dependencies: TODO

[Source](https://github.com/dcos/cosmos)

### PkgPanda

PkgPanda installs and manages DC/OS components.

System Service(s): dcos-pkgpanda-api.service, dcos-pkgpanda-api.socket

Dependencies: TODO

[Source](https://github.com/dcos/dcos/tree/master/pkgpanda)


## IAM & Security

Identity management in DC/OS is delegated to external identity providers, taking advantage of existing infrastructure to reduce the cost and time to market. Security is provided via OAuth authentication and enforced at the edge by Admin Router's reverse proxy.

### OAuth Service

OAuth Service authenticates users using [OAuth](https://oauth.net/) and [Auth0](https://auth0.com/).

System Service(s): dcos-oauth.service

Dependencies: TODO

[Source](https://github.com/dcos/dcos-oauth)


## Storage

DC/OS provides multiple different ways to provision and allocate disk space and volumes to tasks. One of those methods, external persistent volumes, is managed by its own component.

### REX-Ray

REX-Ray orchestrates provisioning, attachment, and mounting of external persistent volumes.

System Service(s): dcos-rexray.service

Dependencies: TODO

[Docs](http://rexray.readthedocs.io/), [Source](https://github.com/codedellemc/rexray)


## Legacy Component Changes

The **Cluster ID service** was removed in DC/OS 1.9.0. The universally unique identifier (UUID) for each cluster is now generated by the DC/OS Setup service.

The **Mesos Persistent Volume Discovery service** was removed in DC/OS 1.9.0. Detection of [mounted disk resources](https://dcos.io/docs/1.8/administration/storage/mount-disk-resources/) is now handled by the DC/OS Setup service.


## Sockets &amp; Timers

Several components are configured to use [systemd sockets](https://www.freedesktop.org/software/systemd/man/systemd.socket.html) which allows them to be started on-demand when a request comes in, rather than running continuously and consuming resources unnecessarily. While these sockets are separate [systemd units](https://www.freedesktop.org/software/systemd/man/systemd.unit.html) they are not considered separate components.

Several components are configured to use [systemd timers](https://www.freedesktop.org/software/systemd/man/systemd.timer.html) which allows them to be periodically executed or restarted. Periodic execution avoids continuous execution and consuming resources unnecessarily. Periodic restarting allows for picking up new configurations from downstream dependencies, like time-based DNS cache expiration. While these timers are separate [systemd units](https://www.freedesktop.org/software/systemd/man/systemd.unit.html) they are not considered separate components.


## Component Management

TODO: GUI components page

TODO: CLI component list/logs


## Component Installation

DC/OS components are installed, upgraded, and managed by [pkgpanda](https://github.com/dcos/dcos/tree/master/pkgpanda), a package manager for systemd units.

To see the full list of packages managed by the DC/OS installer, see the [packages directory of the DC/OS source repository](https://github.com/dcos/dcos/tree/master/packages).


## Systemd Services

Most DC/OS components run as [systemd services](/docs/1.9/overview/concepts/#systemd-service) on the DC/OS nodes.

To see a list of the systemd components running on any particular node, list the contents of the `/etc/systemd/system/dcos.target.wants/` directory or execute `systemctl | grep dcos-` to see their current status.

On a master node:

```
[vagrant@m1 ~]$ ls /etc/systemd/system/dcos.target.wants/
dcos-3dt.service                 dcos-log-master.service        dcos-minuteman.service
dcos-adminrouter-reload.service  dcos-log-master.socket         dcos-navstar.service
dcos-adminrouter-reload.timer    dcos-logrotate-master.service  dcos-oauth.service
dcos-adminrouter.service         dcos-logrotate-master.timer    dcos-pkgpanda-api.service
dcos-cosmos.service              dcos-marathon.service          dcos-pkgpanda-api.socket
dcos-epmd.service                dcos-mesos-dns.service         dcos-signal.service
dcos-exhibitor.service           dcos-mesos-master.service      dcos-signal.timer
dcos-gen-resolvconf.service      dcos-metrics-master.service    dcos-spartan.service
dcos-gen-resolvconf.timer        dcos-metrics-master.socket     dcos-spartan-watchdog.service
dcos-history.service             dcos-metronome.service         dcos-spartan-watchdog.timer
```

On a private agent node:

```
[vagrant@a1 ~]$ ls /etc/systemd/system/dcos.target.wants/
dcos-3dt.service                       dcos-gen-resolvconf.timer     dcos-navstar.service
dcos-3dt.socket                        dcos-log-agent.service        dcos-pkgpanda-api.service
dcos-adminrouter-agent-reload.service  dcos-log-agent.socket         dcos-pkgpanda-api.socket
dcos-adminrouter-agent-reload.timer    dcos-logrotate-agent.service  dcos-rexray.service
dcos-adminrouter-agent.service         dcos-logrotate-agent.timer    dcos-signal.timer
dcos-docker-gc.service                 dcos-mesos-slave.service      dcos-spartan.service
dcos-docker-gc.timer                   dcos-metrics-agent.service    dcos-spartan-watchdog.service
dcos-epmd.service                      dcos-metrics-agent.socket     dcos-spartan-watchdog.timer
dcos-gen-resolvconf.service            dcos-minuteman.service
```

On a public agent node:

[vagrant@p1 ~]$ ls /etc/systemd/system/dcos.target.wants/
dcos-3dt.service                       dcos-gen-resolvconf.timer        dcos-navstar.service
dcos-3dt.socket                        dcos-log-agent.service           dcos-pkgpanda-api.service
dcos-adminrouter-agent-reload.service  dcos-log-agent.socket            dcos-pkgpanda-api.socket
dcos-adminrouter-agent-reload.timer    dcos-logrotate-agent.service     dcos-rexray.service
dcos-adminrouter-agent.service         dcos-logrotate-agent.timer       dcos-signal.timer
dcos-docker-gc.service                 dcos-mesos-slave-public.service  dcos-spartan.service
dcos-docker-gc.timer                   dcos-metrics-agent.service       dcos-spartan-watchdog.service
dcos-epmd.service                      dcos-metrics-agent.socket        dcos-spartan-watchdog.timer
dcos-gen-resolvconf.service            dcos-minuteman.service
