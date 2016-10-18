---
post_title: Running a Containerized Service on a Public Node
nav_title: Running a Containerized Service on a Public Node 
menu_order: 10.5 
---

This tutorial shows you how to run a containerized DC/OS service that serves a web site. Specifically, you will use Docker image that contains NGINX, which serves the dcos.io site.

### Prerequisites
- [A DC/OS cluster with at least one public agent](/docs/1.8/administration/installing/)
- [DC/OS CLI installed](/docs/1.8/usage/cli/install/)

# Configure Your Cluster to Use a Virtual Host

1. Install the [Marathon-LB](/docs/1.8/usage/service-discovery/marathon-lb/) service from Universe. Marathon-lb allows DC/OS services to appear on public-facing nodes.

    ```bash
    $ dcos package add marathon-lb
    ```

**Note:** You can also install Marathon-LB from the DC/OS web interface. Go to the **Universe** tab and navigate to **marathon-lb**. Click **Install** > **Install Package**.

# Configure and Run a Containerized Service on a Public Node

1. Go to the [mesosphere/dcos Docker Hub repository](https://hub.docker.com/r/mesosphere/dcos-website/tags/) and copy the latest image tag.

    ![Mesosphere Docker Hub](/docs/1.8/usage/tutorials/img/dockerhub.png)

1. Locate and note down the IP of [your public agent node](/docs/1.8/administration/locate-public-agent/).
1. Copy `dcos-website.json` from the [dcos-website repository](https://github.com/dcos/dcos-website/blob/develop/dcos-website.json). Replace `<image-tag>` in the `docker:image` field with the tag you copied in step 1.
1. In the labels field, add an entry for `HAPROXY_0_VHOST` and assign it the value of your public agent IP. Remove the leading `http://` and the trailing `/` from the IP. Remember to add a comma after the preceding field.

    ```json
    "labels":{
            "HAPROXY_DEPLOYMENT_GROUP":"dcos-website",
            "HAPROXY_DEPLOYMENT_ALT_PORT":"10005",
            "HAPROXY_GROUP":"external",
            "HAPROXY_0_REDIRECT_TO_HTTPS":"true",
            "HAPROXY_0_VHOST": "<public-agent-ip>"
          }
    ```

    The complete JSON service definition file looks like:

    ```json
    {
      "id": "dcos-website",
      "container": {
        "type": "DOCKER",
        "docker": {
          "image": "mesosphere/dcos-website:<image-tag>",
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
        "HAPROXY_0_REDIRECT_TO_HTTPS":"true",
        "HAPROXY_0_VHOST": "suzanne-g-publicsl-10vofc2b8hopv-206775975.us-west-2.elb.amazonaws.com"
      }
    }
    ```

1. Run the service from the DC/OS CLI using the following command:
    ```
    $ dcos marathon app add dcos-website.json
    ```

1. Go to the **Services** tab of the DC/OS web interface to verify that your application is healthy.

    ![Healthy Service](/docs/1.8/usage/tutorials/img/healthy-dcos-website.png)

1. Go to your public agent to see the site running.