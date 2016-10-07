---
post_title: Create and Run a Service on DC/OS
nav_title: Create and Run a Service 
menu_order: 4.5 
---

#  DC/OS Services

You can run DC/OS services you create or install a package from the [Universe package repository](/docs/1.8/usage/webinterface/#-a-name-universe-a-universe). Both services you create and those you install from Universe appear on the **Services** tab of the DC/OS web interface when they are running.

Services you create yourself are administered by Marathon and can be configured and run [from the DC/OS CLI](/docs/1.8/usage/cli/command-reference/) with `dcos marathon` subcommands (eg `dcos marathon app add myapp.json`) or via the DC/OS web interface.

# Universe Package Repository
Packaged DC/OS services created by Mesosphere or the community, like Spark or Kafka, appear on the **Universe** tab of the DC/OS web interface, or you can search for a package from [the DC/OS CLI](/docs/1.8/usage/cli/command-reference/). You can configure and run Universe services from the DC/OS web interface or via the DC/OS CLI with the `dcos package install <package-name>` command. Visit the [Managing Services](/docs/1.8/usage/managing-services/) section to learn more about installing, configuring, and uninstalling services in Universe.

# Create and Run Your Own DC/OS Service
In this tutorial, we'll go over creating a simple one-command service and a containerized service using both the DC/OS web interface and the CLI.

### Prerequisites
- A DC/OS cluster with at least one public agent
- DC/OS CLI installed

## Create and Run a Simple Service from the DC/OS Web Interface

1. Click the **Services** tab of the DC/OS web interface, then click the **Deploy Service**.
1. Enter a name for your service in the **ID** field. In the **Command** field, enter `sleep 10`.
1. Click **Deploy**.
1. That's it! Click the name of your service in the **Services** view to see it running and monitor health.

## Create and Run a Simple Service from the DC/OS CLI

1. Create a JSON file called `my-app-cli.json` with the following contents:

    ```json
    {
      "id": "/my-app-cli",
      "cmd": "sleep 10",
      "instances": 1,
      "cpus": 1,
      "mem": 128,
      "disk": 0,
      "gpus": 0,
      "backoffSeconds": 1,
      "backoffFactor": 1.15,
      "maxLaunchDelaySeconds": 3600,
      "upgradeStrategy": {
        "minimumHealthCapacity": 1,
        "maximumOverCapacity": 1
      },
      "portDefinitions": [
        {
          "protocol": "tcp",
          "port": 10000
        }
      ],
      "requirePorts": false
    }
    ```

1. Run the service with the following commandt.
    ```bash
    dcos marathon app add my-app.json
    ```

1. Run the following command to verify that your service is running:
    ```bash
    $ dcos marathon app list
    ```
    You can also click the name of your service in the **Services** view of the DC/OS web interface to see it running and monitor health.

## Create a Run a Containerized Service

This portion of the tutorial runs a service that uses a Docker image. This Docker image contains NGINX, which serves the dcos.io site.

### Create and Run a Containerized Service from the DC/OS Web Interface

1. Click the **Services** tab of the DC/OS web interface, then click the **Deploy Service**.
1. Enter a name for your service in the **ID** field.
1. Click the **Container Settings** tab and enter the following in the **Container Image** field: `mesosphere/dcos-website`.
1. Click **Deploy**.
1. Click the name of your service in the **Services** view to see it running and monitor health.
1. Go to your public agent to see the site running. <!-- is this right? -->

### Create and Run a Containerized Service from the DC/OS CLI

1. Copy `dcos-website.json` from the [dcos-website repository](https://github.com/dcos/dcos-website/blob/develop/dcos-website.json). The JSON service definition file looks like:

    ```json
    {
      "id": "dcos-website",
      "container": {
        "type": "DOCKER",
        "docker": {
          "image": "mesosphere/dcos-website",
          "network": "BRIDGE",
          "portMappings": [
            { "hostPort": 0, "containerPort": 80, "servicePort": 10004 }
          ]
        }
      },
      "instances": 3,
      "cpus": 0.25,
      "mem": 100,
      "healthChecks": [{
          "protocol": "HTTP",
          "path": "/",
          "portIndex": 0,
          "timeoutSeconds": 2,
          "gracePeriodSeconds": 15,
          "intervalSeconds": 3,
          "maxConsecutiveFailures": 2
      }],
      "labels":{
        "HAPROXY_DEPLOYMENT_GROUP":"dcos-website",
        "HAPROXY_DEPLOYMENT_ALT_PORT":"10005",
        "HAPROXY_GROUP":"external",
        "HAPROXY_0_REDIRECT_TO_HTTPS":"true"
      }
    }
    ```
    Notice the `docker:image` field, which specifies the location of the container image.

1. Run the service using the following command:
    ```
    $ dcos marathon app add dcos-website.json
    ```

## Scale Your Service

### Scale Your Service from the DC/OS Web Interface

1. From the **Services** tab of the DC/OS CLI, put your cursor over the name of the service you want to scale to reveal a gear symbol.
1. Click the gear symbol and choose **Scale**.
1. Enter the number of instances you would like, then click **Scale Service**.
1. Click the name of your service to see the number of instances you specified.

### Scale Your Service from the DC/OS CLI

1. Enter the following command from the CLI:
    ```bash
    $ dcos marathon app update <app-id> instances=<number_of_desired_instances>
    ```
1. Enter the following command to see information about your services. The `TASKS` column will show the number of instances you specified.
    ```bash
    $ dcos marathon app list
    ```
