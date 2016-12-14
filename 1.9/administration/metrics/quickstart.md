---
post_title: Quick Start
menu_order: 0
---

Use this guide to get started with the DC/OS metrics component. 

**Prerequisites:** 

- At least one [DC/OS service](/docs/1.9/usage/marathon/application-basics/) is deployed.
- Optional: the CLI JSON processor [jq](https://github.com/stedolan/jq/wiki/Installation).

The metrics component is natively integrated with DC/OS and no additional setup is required.  

# Gather metrics for all containers running on a host

1.  SSH to the agent node running the service.

    ```
    $ dcos node ssh --master-proxy --mesos-id=<node-id>
    ```

1.  Run this command to to show all containers that are deployed on the agent node. 

    ```bash
    $ curl -s http://localhost:61001/system/v1/metrics/v0/containers | jq
    ```
    
    The output should resemble this:
    
    ```bash
    ["121f82df-b0a0-424c-aa4b-81626fb2e369","87b10e5e-6d2e-499e-ae30-1692980e669a"]
    ```

# Gather metrics for a container
    
1.  SSH to the agent node running the service.

    ```
    $ dcos node ssh --master-proxy --mesos-id=<node-id>
    ```

1.  Run this command to view the metrics for a particular container (`<container-id>`):

    ```bash
    $ curl -s http://localhost:61001/system/v1/metrics/v0/containers/<container-id>/app | jq
    ```
    
    The output should resemble:
    
    ```bash
    {"datapoints":[{"name":"dcos.metrics.module.container_received_bytes_per_sec","value":0,"unit":"","timestamp":"2016-12-13T23:06:26Z"},{"name":"dcos.metrics.module.container_throttled_bytes_per_sec","value":0,"unit":"","timestamp":"2016-12-13T23:06:26Z"}],"dimensions":{"mesos_id":"","container_id":"6972ad7c-1701-4970-ae14-4372f76eda37","executor_id":"confluent-kafka.7aff271b-c182-11e6-a88f-22e5385a5fd7","framework_id":"a29070cd-2583-4c1a-969a-3e07d77ee665-0001","hostname":""}}
    ```
    
# Gather metrics from container-level cgroup allocations

1.  SSH to the agent node running the service.

    ```
    $ dcos node ssh --master-proxy --mesos-id=<node-id>
    ```

1.  Run this command to view cgroup allocations for a container. 

    ```bash
    $ /system/v1/metrics/v0/containers/<container-id> | jq
    ```
    
    The output will contain a `datapoints` array that contains information about container resource allocation and utilization provided by Mesos. For example:
    
    ```bash
    {
      "datapoints": [
        {
          "name": "cpus_system_time_secs",
          "value": 0.68,
          "unit": "",
          "timestamp": "2016-12-13T23:15:19Z"
        },
        {
          "name": "cpus_limit",
          "value": 1.1,
          "unit": "",
          "timestamp": "2016-12-13T23:15:19Z"
        },
        {
          "name": "cpus_throttled_time_secs",
          "value": 23.12437475,
          "unit": "",
          "timestamp": "2016-12-13T23:15:19Z"
        },
        {
          "name": "mem_total_bytes",
          "value": 327262208,
          "unit": "",
          "timestamp": "2016-12-13T23:15:19Z"
        },
    ```
    
    The output will also contain an object named `dimensions` that contains metadata about the `cluster/node/app`.
        
    ```bash
    ...
    "dimensions": {
        "mesos_id": "a29070cd-2583-4c1a-969a-3e07d77ee665-S0",
        "container_id": "6972ad7c-1701-4970-ae14-4372f76eda37",
        "executor_id": "confluent-kafka.7aff271b-c182-11e6-a88f-22e5385a5fd7",
        "framework_name": "marathon",
        "framework_id": "a29070cd-2583-4c1a-969a-3e07d77ee665-0001",
        "framework_role": "slave_public",
        "hostname": "",
        "labels": {
          "DCOS_MIGRATION_API_PATH": "/v1/plan",
          "DCOS_MIGRATION_API_VERSION": "v1",
          "DCOS_PACKAGE_COMMAND": "eyJwaXAiOlsiaHR0cHM6Ly9kb3dubG9hZHMubWVzb3NwaGVyZS5jb20va2Fma2EvYX...
          "DCOS_PACKAGE_FRAMEWORK_NAME": "confluent-kafka",
          "DCOS_PACKAGE_IS_FRAMEWORK": "true",
          "DCOS_PACKAGE_METADATA": "eyJwYWNrYWdpbmdWZXJzaW9uIjoiMy4wIi...
          "DCOS_PACKAGE_NAME": "confluent-kafka",
          "DCOS_PACKAGE_REGISTRY_VERSION": "3.0",
          "DCOS_PACKAGE_RELEASE": "10",
          "DCOS_PACKAGE_SOURCE": "https://universe.mesosphere.com/repo",
          "DCOS_PACKAGE_VERSION": "1.1.16-3.1.1",
          "DCOS_SERVICE_NAME": "confluent-kafka",
          "DCOS_SERVICE_PORT_INDEX": "1",
          "DCOS_SERVICE_SCHEME": "http",
          "MARATHON_SINGLE_INSTANCE_APP": "true"
        }
      }
    }       
    ...
    ```

# Gather host level metrics

1.  SSH to the agent node running the service.

    ```
    $ dcos node ssh --master-proxy --mesos-id=<node-id>
    ```

1.  Run this command to view host-level metrics:

    ```bash
    $ curl -s http://localhost:61001/system/v1/metrics/v0/node | jq
    ```
    
    The output will contain a `datapoints` array about resource allocation and utilization. For example:
    
    ```bash
    ...
    "datapoints": [
        {
          "name": "uptime",
          "value": 23631,
          "unit": "",
          "timestamp": "2016-12-14T01:00:19Z"
        },
        {
          "name": "processes",
          "value": 209,
          "unit": "",
          "timestamp": "2016-12-14T01:00:19Z"
        },
        {
          "name": "cpu.cores",
          "value": 4,
          "unit": "",
          "timestamp": "2016-12-14T01:00:19Z"
        }
    ...    
    ```
    
    The output will contain an object named `dimensions` that contains metadata about the cluster and node. For example:
    
    ```bash
    ...
    "dimensions": {
        "mesos_id": "a29070cd-2583-4c1a-969a-3e07d77ee665-S0",
        "hostname": "10.0.2.255"
      }
    ...  
    ```