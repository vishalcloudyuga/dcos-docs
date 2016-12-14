---
post_title: Metrics
menu_order: 3.5
---

The [metrics component](https://github.com/dcos/dcos-metrics) provides operational insight to your DC/OS cluster, providing discrete metrics about your applications and deployments. This can include charts, dashboards, and alerts based on cluster, node, container, and application-level statistics. The metrics component is natively integrated with DC/OS version 1.9 and later. No additional setup is required.  

## Overview
There are three layers of metrics identified in DC/OS: 

  * Host: metrics about the specific node which is part of the DC/OS cluster. 
  * Container: metrics about cgroup allocations from tasks running in Mesos or Docker containerizers. 
  * Application: metrics about a specific application running inside a Mesos or Docker containerizer.

The metrics component provides an HTTP API which exposes these three areas. 

All three metrics layers are aggregated by a collector which is shipped as part of the DC/OS distribution. This enables metrics to run on every host in the cluster. It is the main entry point to the metrics ecosystem, aggregating metrics sent to it by the Metrics Mesos module, or gathering host and container level metrics on the box which is runs. 

The Mesos Metrics Module is bundled with every agent in the cluster. This module enables applications to publish metrics from applications running on top of DC/OS to the collector by exposing a StatsD port and host environment variable inside every container. These metrics are appended with structured data such as agent-id, framework-id and task-id.

<!-- insert graphic -->

Per-container metrics tags enable you to arbitrarily group metrics, for example on a per-framework or per-system or agent basis. Here are the available tags:

* `agent_id`
* `container_id`
* `executor_id`
* `framework_id`
* `framework_name`
* `framework_principal`
* `hostname`
* `labels`

DC/OS applications will discover the endpoint via an environment variable (`STATSD_UDP_HOST` or `STATSD_UDP_PORT`). Applications leverage this StatsD interface to send custom profiling metrics to the system.

## Security
Because most metrics are sent to other service stacks and not consumed by DC/OS users, there is not any role based access control for them. However, the HTTP producer does expose and API endpoint, which can be consumed by DC/OS users. Because of this, Enterprise DC/OS provides coarse grained ACLs via the Admin Router proxy to ensure only DC/OS superusers have access to this HTTP API endpoint. 

## Metrics reference
These metrics are automatically collected.

### Per-container resource resource utilization

| Metric            | Description                  |
|-------------------|------------------------------|
| usage.Free        | Available capacity in bytes. |
| usage.Total       | Available capacity in bytes. |
| usage.Used        | Capacity used in bytes.      |
| usage.InodesFree  | Available Inodes in btyes.   |
| usage.InodesTotal | Available Inodes in bytes.   |
| usage.InodesUsed  | Inodes used in bytes.        |

###  Node

#### Metrics
   
| Metric            | Description                  |
|-------------------|------------------------------|
| cpu.cores         |    Percentage of cores used.     |
| cpu.idle         |     Percentage of CPUs idle.         |
| cpu.system         |    Percentage of system used.   |
| cpu.total         |   Percentage of CPUs used.  |
| cpu.user         |   Percentage of CPU used by the user.   |
| cpu.wait         |   Percentage idle while waiting for an operation to complete.    |
| load.1min         |     Load average for the past minute.       |
| load.5min         |   Load average for the past 15 minutes 5 minutes.        |
| load.15min         |    Load average for the past 15 minutes.        |
| memory.buffers         |   Number of memory buffers.     |
| memory.cached         |   Amount of cached memory.   |
| memory.free         |    Amount of free memory in bytes.   |
| memory.total         |   Total memory in bytes.   |
| processes         |  Number of processes that are running.          |
| swap.free         |  Amount of free swap space.   |
| swap.total         |  Total swap space.    |
| swap.used         |    Amount of swap space used.    |
| uptime          |   The system reliability and load average.    |
   
#### Filesystem
   
| Metric            | Description                  |
|-------------------|------------------------------|
| filesystem.{{.Name}}.capacity.free    | Amount of available capacity in bytes. |
| filesystem.{{.Name}}.capacity.total    | Total capacity in bytes. |
| filesystem.{{.Name}}.capacity.used    |  Capacity used in bytes. |
| filesystem.{{.Name}}.inodes.free    | Amount of available inodes in bytes. |
| filesystem.{{.Name}}.inodes.total    | Total inodes in bytes. |
| filesystem.{{.Name}}.inodes.used    | Inodes used in bytes.  |
      
#### NetworkInterface
   
| Metric            | Description                  |
|-------------------|------------------------------|
| network.{{.Name}}.in.bytes    | Number of bytes downloaded. |
| network.{{.Name}}.in.dropped    | Number of downloaded bytes dropped. |
| network.{{.Name}}.in.errors    | Number of downloaded bytes in error. |
| network.{{.Name}}.in.packets    | Number of packets downloaded. |
| network.{{.Name}}.out.bytes    | Number of bytes uploaded. |
| network.{{.Name}}.out.dropped    | Number of uploaded bytes dropped. |
| network.{{.Name}}.out.errors    | Number of uploaded bytes in error.  |
| network.{{.Name}}.out.packets    | Number of packets uploaded. |
   
### Container metrics

Container metrics define the structure of the response expected from Mesos when referring to container and executor metrics.

#### CPU usage info
   <!-- https://github.com/apache/mesos/blob/1.0.1/include/mesos/v1/mesos.proto -->
   
| Metric            | Description                  |
|-------------------|------------------------------|
| cpus_limit    | The number of CPUs allocated. |
| cpus_system_time_secs    | Total CPU time spent in kernal mode in seconds. |
| cpus_throttled_time_secs    | Total time, in seconds, that CPU was throttled. |
| cpus_user_time_secs    | Total CPU time spent in user mode. |

#### Disk info
   
| Metric            | Description                  |
|-------------------|------------------------------|
| disk_limit_bytes    | Hard memory limit for disk in bytes. |
| disk_used_bytes    | Hard memory used in bytes.  |
   
####  Memory info
   <!-- https://github.com/apache/mesos/blob/1.0.1/include/mesos/v1/mesos.proto -->
   
| Metric            | Description                  |
|-------------------|------------------------------|
| mem_limit_bytes    | Hard memory limit for a container. |
| mem_total_bytes    | Total memory of a process in RAM (as opposed to in Swap). |   
   
### Dimensions
   <!-- http://mesos.apache.org/documentation/latest/port-mapping-isolator -->
Dimensions are metadata about the metrics contained in a given MetricsMessage
   
| Metric            | Description                  |
|-------------------|------------------------------|
| net_rx_bytes    | Bytes received. |
| net_rx_dropped    | Packets dropped on receive.  |
| net_rx_errors    | Errors reported on receive. |
| net_rx_packets    |  Packets received.  |
| net_tx_bytes    |  Bytes sent. |
| net_tx_dropped    | Packets dropped on send  |
| net_tx_errors    | Errors reported on send. |
| net_tx_packets    | Packets sent. |
