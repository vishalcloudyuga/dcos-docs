---
post_title: Boot Sequence
nav_title: Boot Sequence
menu_order: 6
---

During DC/OS installation, the components come online in a particular sequence due to interdependencies.

## Master nodes

On each master node the following happens, in chronological order:

1. [Exhibitor](https://github.com/mesosphere/exhibitor-dcos) starts up, creates ZooKeeper configuration and launches ZooKeeper.
1. The Mesos masters are launched, register with the local ZooKeeper (127.0.0.1), and discover the other masters.
1. Mesos-DNS is launched on every master node.
1. Mesos-DNS keeps hitting `leader.mesos:5050/master/state.json`. The DNS entry `leader.mesos` points to the currently leading Mesos master.
1. The Distributed DNS Proxy runs on all master and agent nodes, and forwards DNS lookups to Mesos-DNS.
1. DC/OS Marathon is launched and starts on every master node.
1. DC/OS Marathon connects to the local ZooKeeper (127.0.0.1), discovers the leading Mesos master (`leader.mesos`) and registers as a framework.
1. Admin Router depends on the Mesos master, Mesos-DNS, and the Distributed DNS Proxy. It runs on each of the master nodes. The admin router is what serves the DC/OS UI and proxies external admin connections into the cluster.
1. DC/OS UI, Mesos UI, and Exhibitor UI become externally accessible through the Admin Router.
1. [Auth](/docs/1.9/administration/id-and-access-mgt/) is managed on the master nodes.
1. The history service provides the data for the graphs in the DC/OS UI dashboard. This data is obtained from the masters.
1. DC/OS diagnostics and systemd service on every node.

## Agent nodes

On each agent node the following happens, in chronological order:

1. The Mesos agents wait until they can ping `leader.mesos`.
1. The Mesos agents start up and discover the leading Mesos master (`leader.mesos`) by using ZooKeeper.
1. The Mesos agents register as agent with the leading Mesos master (`leader.mesos`).
1. The Mesos master attempts to connect back to the agent node by using the IP address the agent registered with.
1. The Mesos agents becomes available for launching new tasks.
1. DC/OS nodes become visible in the DC/OS UI.

## Services

After DC/OS installation completes, you can install DC/OS services. For each installed DC/OS service, the following happens:

- Service scheduler starts up and discovers the leading Mesos master via ZooKeeper.
- Service scheduler registers with leading Mesos master (`leader.mesos`).
- Mesos master confirms and scheduler stores the service ID in ZooKeeper.
- After a service is registered, the resource offer cycle between the Mesos master and scheduler is started (details in next section).
