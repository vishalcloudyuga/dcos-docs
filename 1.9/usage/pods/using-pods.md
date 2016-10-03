---
post_title: Using Pods
menu_order: 10
---

DC/OS handles and represents pods as single services. From the perspective of Marathon, pods are members of [groups](http://mesosphere.github.io/marathon/docs/application-groups.html). Containers in pods share networking namespace and ephemeral volumes.

You can create and manage pods with the [DC/OS CLI](/docs/1.9/usage/pods/pods-cli/) or via the /v2/pods/ endpoint of the [Marathon REST API](http://mesosphere.github.io/marathon/docs/generated/api.html).

# Pod Definitions
Pods are configured via a JSON pod definition, which is similar to a [Marathon application definition](http://mesosphere.github.io/marathon/docs/application-basics.html). See the [Examples](/docs/1.9/usage/pods/examples/) section for more information. You must declare the resources required by each container in the pod, even if Mesos is isolating at a higher (pod) level. 

**Note:** DC/OS reserves 32 MB and .1 CPUs per pod for overhead. Take this overhead into account when declaring resource needs for the containers in your pod.

# Networking
Pods only support the [DC/OS Universal containerizer runtime](https://dcos.io/docs/1.9/usage/containerizers/), which simplifies networking among containers. The containers of each pod instance share a network namespace and can communicate over localhost. 

Containers in pods are created with endpoints. Other applications communicate with pods by addressing those endpoints. You can specify a default container network by using the CLI `dcos marathon` command. If you specify a container network without a name in a pod definition, it will be assigned to this default network.

In your pod definition you can declare a `host` or `container` network type. Pods created with `host` type share the network namespace of the host. Pods created with `container` type use virtual networking. If you specify the `container` network type, you must also declare a virtual network name in the `name` field. See the [Examples](/docs/1.9/usage/pods/examples/) section for the full JSON.

# Ephemeral Storage
Containers within a pod share ephemeral storage. You can mount volumes by different names on each container in the pod.
