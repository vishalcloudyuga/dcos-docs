---
post_title: Debugging
feature_maturity: experimental
menu_order: 115
---

DC/OS offers several tools to debug your services when they have failed to deploy or are not behaving as you expect. This topic goes over how to debug from both the DC/OS CLI and the DC/OS web interface.

There are several reasons why your service may fail to deploy. Some possibilities include:

- Marathon isn't getting the resource offers it needs to launch the app.
  Use the [CLI](/docs/1.9/usage/debugging/cli-debugging) or the [debugging page in the DC/OS web interface](/docs/1.9/usage/debugging/gui-debugging) to troubleshoot unmatched or unaccepted resource offers from Mesos. You can also [consult the service and task logs](/docs/1.9/administration/logging/).

- The service's health check is failing.
  If a service has a health check, deployment does not complete until the health check passes. You can see the health of a service with Marathon health checks from [the DC/OS web interface](/docs/1.9/usage/debugging/gui-debugging). To see more information about the health of a service with Marathon health checks, run `dcos marathon app list --json` from the DC/OS CLI.

- `docker pull` is failing.
  If your app runs in a Docker image, the Mesos agent node will first have to pull the Docker image. If this fails, your app could get stuck in a "deploying" state. The Mesos agent logs (`<dcos-url>/mesos/#/agents/`) will contain this information.
