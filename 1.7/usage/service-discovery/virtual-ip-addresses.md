---
post_title: Using Virtual IP Addresses
post_excerpt: ""
layout: docs.jade
---
[Ports management][1] in Marathon is a very powerful albeit complex feature. Many configurations are possible depending on the type of container and the specific requirements.

When running Marathon in a DC/OS cluster, virtual addresses (VIPs) can be leveraged to drastically simplify inter-app communication and implement a reliable service-oriented architecture. VIPs are used to map traffic from a single virtual address to multiple IP addresses and ports.

In this tutorial you will learn how to deploy a Wordpress + MySQL installation on a DC/OS cluster and stop worrying about network management.

## Overview

Let's assume our goal is to deploy a simple Wordpress website which consists of two distinct services: a Wordpress installation, `/wordpress` and a MySQL database, `/mysql`. Obviously we want our webserver to be reachable from the outside but the database should only be accessible internally to our cluster.

[Marathon LB](https://docs.mesosphere.com/overview-3/) can help us connecting the outside world to our app, but how can we reliably enable Wordpress to communicate with the MySQL instance?

Thanks to **VIPs** we will only need to specify a virtual address for the database service and simply use it as a static configuration in our web app. The traffic will be automatically load balanced from the app to the service. Also, when instances of the database service die (due to power failures or other issues), new connections will automatically be directed to healthy instances of the service with a very short reaction time.

This incredibly powerful yet simple to use feature solves 3 of the hardest problems in running a service oriented architecture: 

 - finding where the service is running in the datacenter
 - figuring out which instance to send traffic to in order to avoid overloading any particular one
 - dealing gracefully with failures to these instances when they happen

We are now going to see how easy it is to make use of this powerful feature in DC/OS.

## Prerequisites

- A DC/OS cluster with at least 1 private agent and 1 public agent.
- The public IP address of your DCOS public agent.

## Deplyoing our apps

Let's begin by deploying our MySQL database. Here is a simple JSON definition that we can use to deploy the Docker container `mysql:5.6.12` via Marathon:

````json
{
  "id": "/mysql",
  "cpus": 1,
  "mem": 1024,
  "instances": 1,
  "container": {
    "type": "DOCKER",
    "volumes": [
      {
        "containerPath": "mysqldata",
        "mode": "RW",
        "persistent": {
          "size": 1000
        }
      }
    ],
    "docker": {
      "image": "mysql:5.7.12",
      "network": "BRIDGE",
      "portMappings": [
        {
          "containerPort": 3306,
          "hostPort": 0,
          "protocol": "tcp",
          "labels": {
            "VIP_0": "3.3.0.6:3306"
          }
        }
      ]
    }
  },
  "args": [
    "--datadir=/mnt/mesos/sandbox/mysqldata/"
  ],
  "env": {
    "MYSQL_ROOT_PASSWORD": "root",
    "MYSQL_USER": "wordpress",
    "MYSQL_PASSWORD": "secret",
    "MYSQL_DATABASE": "wordpress"
  },
  "healthChecks": [
    {
      "protocol": "TCP",
      "portIndex": 0,
      "gracePeriodSeconds": 300,
      "intervalSeconds": 60,
      "timeoutSeconds": 20,
      "maxConsecutiveFailures": 3,
      "ignoreHttp1xx": false
    }
  ],
  "upgradeStrategy": {
    "minimumHealthCapacity": 0,
    "maximumOverCapacity": 0
  }
}
````

The interesting bits here are in the `portMappings` where we actually define our `VIP_0` label. This tells DC/OS to reserve that IP:port tuple for our MySQL server which will make it reachable by other services in the cluster via the VIP `3.3.0.6:3306`. Note how the index-based label would allow us to specify multiple VIPs per port. If we wanted to add a second VIP, this would be valid:

````json
"portMappings": [
        {
          "containerPort": 3306,
          "hostPort": 0,
          "protocol": "tcp",
          "labels": {
            "VIP_0": "3.3.0.6:3306",
            "VIP_0": "4.4.0.7:3306"
          }
        }
      ]

````

Next, we are going to make use of our VIP to instruct Wordpress on how to reach the database, using environment variables:

````json
{
  "id": "/wordpress",
  "cmd": null,
  "cpus": 1,
  "mem": 1024,
  "disk": 0,
  "instances": 1,
  "acceptedResourceRoles": [
    "slave_public"
  ],
  "container": {
    "type": "DOCKER",
    "volumes": [],
    "docker": {
      "image": "wordpress:",
      "network": "BRIDGE",
      "portMappings": [
        {
          "containerPort": 80,
          "hostPort": 0,
          "protocol": "tcp",
          "labels": {}
        }
      ],
      "privileged": false,
      "parameters": [],
      "forcePullImage": false
    }
  },
  "env": {
    "WORDPRESS_DB_HOST": "3.3.0.6:3306",
    "WORDPRESS_DB_USER": "wordpress",
    "WORDPRESS_DB_PASSWORD": "secret",
    "WORDPRESS_DB_NAME": "wordpress"
  },
  "healthChecks": [
    {
      "path": "/",
      "protocol": "HTTP",
      "portIndex": 0,
      "gracePeriodSeconds": 300,
      "intervalSeconds": 60,
      "timeoutSeconds": 20,
      "maxConsecutiveFailures": 3,
      "ignoreHttp1xx": false
    }
  ],
  "portDefinitions": [
    {
      "port": 10000,
      "protocol": "tcp",
      "labels": {}
    }
  ]
}
````

Note the line with `"WORDPRESS_DB_HOST": "3.3.0.6:3306"` which is used to configure the database host by the `wordpress` Docker container.

Also, we've used the `acceptedRoles` array to specify the `slave_public` role which will ensure Wordpress will be installed on a public node. For more information on this topic, refer to the [Deploying a Containerized App on a Public Node](https://docs.mesosphere.com/usage/tutorials/containerized-app/) tutorial.

Now that we have our application definitions ready, let's fire up the Marathon web UI and deploy them.

## Deploying the Web App via Marathon

From the DC/OS web interface, click on the **Services** tab and select **marathon**.

- To create a new application, click **Create Application** and switch to the `JSON mode` by clicking on the toggle in the upper right corner.
- Let's erase the contents of the textarea and replace them with the JSON app definition for MySQL above.
- Once the deployment of `/mysql` has started, let's repeat the same steps for the Wordpress application definition.

Please note you can also assign a VIP to your application by using the DC/OS Marathon web interface. The values you enter in these fields are translated into the appropriate `portMapping` (for Docker containers in Bridge mode) or `portDefinitions` entry in your application definition. Toggle to `JSON mode` as you create your app to see and edit your application definition.

![Marathon Ports](/docs/1.7/overview/img/ui-marathon-ports.gif)


For more information on port configuration, see the [ports documentation][1].

[1]: http://mesosphere.github.io/marathon/docs/ports.html


## Install Wordpress

Once the deployments have finished and our applications appear to be running, Marathon will start performing its [health checks](https://mesosphere.github.io/marathon/docs/health-checks.html). Provided that our cluster has enough resources, we should be seeing both services as running and healthy after a few minutes.

![Web App Running](/docs/1.7/usage/service-discovery/img/ui-marathon-running-services.png)

At this point, click on the `/wordpress` app and inspect the Tasks list. We will use the public slave node's public IP address in combination with the running instance's host port to access the Wordpress installation URL.

![Wordpress Task](/docs/1.7/usage/service-discovery/img/ui-marathon-wordpress-task.png)

Assuming our public node's IP address is 192.168.1.1, then we'd open our browser to point to `http://192.168.1.1:31094`, provided that `31094` is the host port assigned to our service as shown in the screenshot. 

At this point we will be greeted by the Wordpress setup page and from there we'll be able to finish the installation against our MySQL database.

![Wordpress up and running](/docs/1.7/usage/service-discovery/img/wordpress-running.png)

## Conclusions

**Please note**: for the purpose of this tutorial we haven't touched on how to actually ensure that the MySQL data can be persisted. Head over to the [Cassandra](https://docs.mesosphere.com/cassandra-1-7/) tutorial to learn how to deploy stateful applications in DC/OS.
