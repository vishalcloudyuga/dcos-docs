---
post_title: Health Checks
menu_order: 1
---

Health checks are specified per application and are run against that application's tasks.

- The default health check leverages Mesos' knowledge of the task state `TASK_RUNNING => healthy`.
- Marathon provides a `health` member of the task resource via the [REST API](/docs/1.8/usage/marathon/rest-api/) that you can add to your application definition.

A health check is considered passing if both of these conditions are met:
 
- The HTTP response code is between 200 and 399 inclusive
- The response is received within the `timeoutSeconds` period. If a task fails more than `maxConsecutiveFailures` health checks consecutively, that task is killed.
