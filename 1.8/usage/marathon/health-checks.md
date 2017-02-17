---
post_title: Health Checks
menu_order: 1
---

You can define health checks for your DC/OS services. Health checks are defined on a per application basis, and are run against that application's tasks.

- The default health check leverages Mesos' knowledge of the task state `TASK_RUNNING => healthy`.
- Marathon provides a `health` member of the task resource via the [REST API](/docs/1.8/usage/marathon/rest-api/) that you can add to your application definition.

A health check is considered passing if both of these conditions are met:
 
- The HTTP response code is between 200 and 399 inclusive
- The response is received within the `timeoutSeconds` period. If a task fails more than `maxConsecutiveFailures` health checks consecutively, that task is killed.

# Anatomy of a health check

In general, health checks consist of these parameters:

- **Grace period** Specifies the amount of time that is allowed for the task to become healthy.
- **Interval** Specifies the amount of time to wait between health checks.
- **Max failures** Specifies the number of consecutive health check failures that are allowed until the task is terminated.
- **Path** Specifies the endpoint that provides health status. 
- **Port** Specifies the port number to use for health checks.
- **Port index** Specifies an index in the app’s ports or portDefinitions array that is used for health requests. 
- **Protocol** Specifies the type of protocol.
- **Timeout** Specifies the amount of time that is allowed until a health check is considered a failure, regardless of the response.

You can define health checks in your JSON application definition or by using the DC/OS web interface **Services** tab. For example, here is the health check portion of the [Cassandra](docs.mesosphere.com/usage/service-guides/cassandra/) DC/OS service JSON application definition.

```json
"healthChecks": [
  {
    "gracePeriodSeconds": 120,
    "intervalSeconds": 30,
    "maxConsecutiveFailures": 0,
    "path": "/admin/healthcheck",
    "portIndex": 0,
    "protocol": "HTTP",
    "timeoutSeconds": 5
  }
```

