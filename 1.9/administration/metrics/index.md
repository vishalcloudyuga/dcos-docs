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

<!-- ![architecture diagram](https://www.lucidchart.com/publicSegments/view/30f4c23-b2f9-4db3-9954-a947f395eae5/image.png) -->

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

*  Per-container resource resource utilization (metrics named `usage.*`). Sends various stats in Metrics Avro format to the provided Collector endpoint.

| Metric            | Description                  |
|-------------------|------------------------------|
| usage.Free        | Available capacity in bytes. |
| usage.InodesFree  | Available Inodes in btyes.   |
| usage.InodesTotal | Available Inodes in bytes.   |
| usage.InodesUsed  | Inodes used in bytes.        |
| usage.Total       | Available capacity in bytes. |
| usage.Used        | Capacity used in bytes.      |

*  Agent and system-level resource utilization (metrics named `node.*`, not tied to a specific container, so only tagged with `agent_id`)

   Node Metrics
   
| Metric            | Description                  |
|-------------------|------------------------------|
| cpu.cores         |    Number of cores.     |
| cpu.idle         |                               |
| cpu.system         |                               |
| cpu.total         |   Percent of CPU available.     |
| cpu.user         |   CPU used by user.   |
| cpu.wait         |                               |
| filesystems         |                               |
| load.1min         |     System load average during the last minute.       |
| load.5min         |   System load average during the last 5 minutes.        |
| load.15min         |    System load average during the last 15 minutes.        |
| memory.buffers         |                               |
| memory.cached         |                               |
| memory.free         |                               |
| memory.total         |                               |
| network_interfaces         |                               |
| processes         |  Number of processes that are running.          |
| swap.free         |                               |
| swap.total         |                               |
| swap.used         |                               |
| uptime          |   The system reliability and load average.     |
   
   Node Filesystem
   
| Metric            | Description                  |
|-------------------|------------------------------|
| filesystem.{{.Name}}.capacity.free    |    |
| filesystem.{{.Name}}.capacity.total    |    |
| filesystem.{{.Name}}.capacity.used    |    |
| filesystem.{{.Name}}.inodes.free    |    |
| filesystem.{{.Name}}.inodes.total    |    |
| filesystem.{{.Name}}.inodes.used    |    |
      
   Node NetworkInterface
   
| Metric            | Description                  |
|-------------------|------------------------------|
| network.{{.Name}}.in.bytes    |    |
| network.{{.Name}}.in.dropped    |    |
| network.{{.Name}}.in.errors    |    |
| network.{{.Name}}.in.packets    |    |
| network.{{.Name}}.out.bytes    |    |
| network.{{.Name}}.out.dropped    |    |
| network.{{.Name}}.out.errors    |    |
| network.{{.Name}}.out.packets    |    |
   
   Agent resourceStatistics 
   
| Metric            | Description                  |
|-------------------|------------------------------|
| cpus_limit    |    |
| cpus_system_time_secs    |    |
| cpus_throttled_time_secs    |    |
| cpus_user_time_secs    |    |

   
   Memory info
   
| Metric            | Description                  |
|-------------------|------------------------------|
| mem_limit_bytes,omitempty    |    |
| mem_total_bytes,omitempty    |    |   
   
   Disk info
   
| Metric            | Description                  |
|-------------------|------------------------------|
| disk_limit_bytes    |    |
| disk_used_bytes    |    |
   
   Network info
   
| Metric            | Description                  |
|-------------------|------------------------------|
| net_rx_bytes    |    |
| net_rx_dropped    |    |
| net_rx_errors    |    |
| net_rx_packets    |    |
| net_tx_bytes    |    |
| net_tx_dropped    |    |
| net_tx_errors    |    |
| net_tx_packets    |    |