---
post_title: Metrics
menu_order: 3.5
---

This section provides information on metrics supported by DC/OS.


# Gathering metrics from Mesos tasks
<!-- Enable Metrics Gathering from Mesos Tasks (App Level) (OSS) -->
You can configure DC/OS to automatically send metrics from your Mesos tasks by using the `/v0/node` endpoint.  

# Gathering metrics from cgroup allocations
<!-- Enable Metrics Gathering from CGroup Allocations (Container Level) (OSS) -->
You can get individual container resource metrics for each container deployed by DC/OS by using the `/v0/containers/{id}` endpoint.

# Gathering metrics from DC/OS host
<!-- Enable Metrics Gathering from DC/OS Host (Host Level) (OSS) -->
You can get per-host data regarding host-level resources in your DC/OS cluster by using the `/v0/containers/{id}/app/{metric-id}` endpoint.


# Using the metrics HTTP API
<!-- Expose All Metrics with a HTTP API (OSS) -->
You can use your custom built tools to get any metric, either from running Mesos tasks or the hosts which run my DC/OS cluster. 

The HTTP producer is enabled by default and exposes a JSON-formatted HTTP API on each node in the cluster. These APIs include both metrics datapoints as well as dimensions, or key/value pairs with relevant node and cluster metadata.

The path to this API is `/system/v1/metrics/`. The available endpoints include:

* `/v0/ping` - Basic health check
* `/v0/node` - Node metrics (CPU, memory, storage, networks, etc)
* `/v0/containers` - An array of container IDs running on the node
* `/v0/containers/{id}` - Resource allocation and usage for the given container ID
* `/v0/containers/{id}/app` - Application-level metrics from the container (shipped in [DogStatsD format](http://docs.datadoghq.com/guides/dogstatsd/) using the `STATSD_UDP_HOST` and `STATSD_UDP_PORT` environment variables)
* `/v0/containers/{id}/app/{metric-id}` - Similar to `/v0/containers/{id}/app`, but only contains datapoints for a single metric ID