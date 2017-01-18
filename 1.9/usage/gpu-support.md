---
post_title: GPU Acceleration
nav_title: GPU Acceleration
feature_maturity: experimental
menu_order: 110 
---

DC/OS now supports allocating GPUs (Graphics Processing Units) to your long-running DC/OS services. Adding GPUs to your service can dramatically accelerate big data workloads.  [Learn more](http://www.nvidia.com/object/what-is-gpu-computing.html).

By default, DC/OS is not configured to run with GPUs, so you must explicitly enable this support when first installing DC/OS on your cluster. Once configured, DC/OS treats the GPUs as another resource available to your app.

# Configure your Cluster for GPUs

Follow these steps to enable GPU support on your cluster. 

1. Ensure that all machines in your cluster have [Nvidia GPU libraries](https://developer.nvidia.com/nvidia-management-library-nvml) installed on them. The machines do not need to have GPUs on them, but they will fail to start without the libraries.

    If you are using AWS, [generate a custom AWS CF template](https://dcos.io/docs/1.8/administration/installing/cloud/aws/advanced/aws-custom/) that uses one of the following AMIs. 
    
        us-west-2: ami-9b5d97fb
        us-east-1: ami-e10e50f6
        ap-southeast-2: ami-37b28f54

    

    If you are not using AWS, find detailed installation instructions [here](https://github.com/apache/mesos/blob/master/docs/gpu-support.md#external-dependencies).

1. Add the `enable_gpu_isolation: true` flag to your `config.yaml` when you install DC/OS on your cluster. If you are installing DC/OS from the AWS cloud template, follow [the instructions here](/docs/1.8/administration/installing/cloud/aws/advanced/aws-custom/). Otherwise, follow [the custom installation instructions](/docs/1.8/administration/installing/custom/).

# Configure your Service to Use GPUs

Once your cluster is configured to support GPUs, add the `gpus` parameter to your [Marathon application definition](/docs/1.8/usage/marathon/application-basics/).

# Examples

## Simple GPU Application Definition
```
{
     "id": "simple-gpu-test",
     "acceptedResourceRoles":["slave_public", "*"],
     "cmd": "while [ true ] ; do nvidia-smi; sleep 5; done",
     "cpus": 1,
     "mem": 128,
     "disk": 0,
     "gpus": 1,
     "instances": 1
}
```

## Docker-Based Application Definition

**Important:** You must use the [DC/OS Universal Container Runtime](/docs/1.8/usage/containerizers/) to run a containerized application that uses GPUs. To use the Universal Containerizer, set the container type to `MESOS`.
```
{
    "id": "docker-gpu-test",
    "acceptedResourceRoles":["slave_public", "*"],
    "cmd": "while [ true ] ; do nvidia-smi; sleep 5; done",
    "cpus": 1,
    "mem": 128,
    "disk": 0,
    "gpus": 1,
    "instances": 1,
    "container": {
      "type": "MESOS",
      "docker": {
        "network": "HOST",
        "image": "nvidia/cuda"
      }
    }
}
```
