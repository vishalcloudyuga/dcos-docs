---
post_title: GPU Acceleration
nav_title: GPU Acceleration
feature_maturity: experimental
menu_order: 110 
---

You can configure your DC/OS service to use the GPU (Graphics Processing Unit) of your nodes. GPUs can dramatically accelerate big data workloads. [Learn more](http://www.nvidia.com/object/what-is-gpu-computing.html).

# Configure your Cluster for GPUs

Follow these steps to enable GPU support on your cluster. 

1. Add the `enable_gpu_isolation: true` flag to your `config.yaml` when you install DC/OS on your cluster. If you are installing DC/OS from the AWS cloud template, follow [the instructions here](/docs/1.8/administration/installing/cloud/aws/advanced/aws-custom/). Otherwise, follow [the custom installation instructions](/docs/1.8/administration/installing/custom/).

1. Ensure that all machines in your cluster have [Nvidia GPU libraries](https://developer.nvidia.com/gpu-accelerated-libraries) installed on them. The machines do not need to have GPUs on them, but they will fail to start without the libraries.

# Configure your Service to Use GPUs

Once your cluster is configured to support GPUs, add the `gpus` parameter to your Marathon application definition.

# Examples

## Simple GPU Application Definition

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
## Docker-Based Application Definition

**Important:** You must use the [DC/OS Universal Container Runtime](/docs/1.8/usage/containerizers/) to run a containerized application that uses GPUs.

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

