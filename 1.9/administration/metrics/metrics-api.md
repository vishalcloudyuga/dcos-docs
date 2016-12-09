---
post_title: Metrics API 
menu_order: 1
---

You can use your custom built tools to get any metric, either from running Mesos tasks or the hosts which run my DC/OS cluster. 

The HTTP producer is enabled by default and exposes a JSON-formatted HTTP API on each node in the cluster. These APIs include both metrics datapoints as well as dimensions, or key/value pairs with relevant node and cluster metadata.

## Base Structure

```
http://<hostname>:<port>/system/v<version>/metrics/v<version>/<request>
```

## Node Endpoints

```
/system/metrics/v0/node
```

## Containers and App Endpoints

* `/system/metrics/v0/ping` - Basic health check
* `/system/metrics/v0/node` - Node metrics (CPU, memory, storage, networks, etc) 
* `/system/metrics/v0/containers` - An array of container IDs running on the node
* `/system/metrics/v0/containers/<id>` - Resource allocation and usage for the given container ID
* `/system/metrics/v0/containers/<id>/app` - Application-level metrics from the container (shipped in [DogStatsD format](http://docs.datadoghq.com/guides/dogstatsd/) using the `STATSD_UDP_HOST` and `STATSD_UDP_PORT` environment variables)
* `/system/metrics/v0/containers/<id>/app/<metric-id>` - Similar to `/v0/containers/{id}/app`, but only contains datapoints for a single metric ID

<div class="swagger-section">
  <div id="message-bar" class="swagger-ui-wrap message-success" data-sw-translate=""></div>
  <div id="swagger-ui-container" class="swagger-ui-wrap" data-api="/1.9/api/metrics.yaml">

  <div class="info" id="api_info">
    <div class="info_title">Loading docs...</div>
  <div class="info_description markdown"></div>
</div>