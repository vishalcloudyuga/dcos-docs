---
post_title: GPU Acceleration
nav_title: GPU Acceleration
feature_maturity: experimental
menu_order: 110 
---

DC/OS now supports allocating GPUs (Graphics Processing Units) to your long-running DC/OS services. Adding GPUs to your service can dramatically accelerate big data workloads.  [Learn more](http://www.nvidia.com/object/what-is-gpu-computing.html).

By default, DC/OS is not configured to run with GPUs, so you must explicitly enable this support when first installing DC/OS on your cluster. Once configured, DC/OS treats  GPUs as another resource available to your service.

# Configure your Cluster for GPUs

Follow these steps to enable GPU support on your cluster. Below are instructions for AWS cloud and custom installations.

All machines in your cluster must have the [Nvidia Management Library (NVML)](https://developer.nvidia.com/nvidia-management-library-nvml) installed on them. The machines do not need to have GPUs on them, but they will fail to to come online without this library if GPU support is enabled as described below.

## Configure your AWS Cloud Cluster

- Prerequisite: An AWS S3 bucket.

1. Generate [a custom AWS CF template](https://dcos.io/docs/1.8/administration/installing/cloud/aws/advanced/aws-custom/), adding the `enable_gpu_isolation: true` flag to your `config.yaml` file.

        enable_gpu_isolation: true
        aws_template_storage_bucket: <s3-bucket-name>
        aws_template_storage_bucket_path: templates/dcos
        aws_template_upload: true
        aws_template_storage_access_key_id: <your-access-key-id>
        aws_template_storage_secret_access_key: <your-secret-access_key>

1. When you create your stack on [CloudFormation](https://console.aws.amazon.com/cloudformation/home), choose one of these three regions: US West (Oregon), US East (N. Virginia), or Asia Pacific (Sydney).

1. In your S3 bucket, navigate to `<s3-bucket-name>/templates/`. Then right-click on `dcos` and select **Make Public**.

1. On the **Specify Details** page, specify one of the following AMIs. These AMIs have the required [Nvidia Management Library (NVML)](https://developer.nvidia.com/nvidia-management-library-nvml) installed on them. Choose the AMI that corresponds to the region you chose in the previous step.

        us-west-2: ami-9b5d97fb
        us-east-1: ami-e10e50f6
        ap-southeast-2: ami-37b28f54

1. Also on the **Specify Details** page, make sure that you choose GPU-type machines for your `PrivateAgentInstanceType` and `PublicAgentInstanceType`.

## Configure a Non-AWS Cluster

1. Install the [Nvidia Management Library (NVML)](https://developer.nvidia.com/nvidia-management-library-nvml) on each node of your cluster, unless it is already installed. Find detailed installation instructions [here](https://github.com/apache/mesos/blob/master/docs/gpu-support.md#external-dependencies).

1. Add the `enable_gpu_isolation: true` flag to your `config.yaml` when you [install DC/OS on your cluster](/docs/1.8/administration/installing/custom/).

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

Once your service has deployed, check the contents of `stdout` to verify that the service is producing the proper output from the `nvidia-smi` command. You should see something like the following, repeated once every 5 seconds. Access the log [via the DC/OS CLI](https://dcos.io/docs/1.9/administration/logging/quickstart/) or from the **Health** page for your service on the DC/OS dashboard.
```
+------------------------------------------------------+
| NVIDIA-SMI 352.79     Driver Version: 352.79         |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  Tesla M60           Off  | 0000:04:00.0     Off |                    0 |
| N/A   34C    P0    39W / 150W |     34MiB /  7679MiB |      0%      Default |
+-------------------------------+----------------------+----------------------+
```

## Docker-Based Application Definition

**Important:** You must use the [DC/OS Universal Container Runtime](/docs/1.8/usage/containerizers/) to run a containerized application that uses GPUs. To use the Universal Container Runtime, set the container type to `MESOS`.
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

Once your service has deployed, check the contents of `stdout` to verify that the service is producing the proper output from the `nvidia-smi` command. You should see something like the following, repeated once every 5 seconds. Access the log [via the DC/OS CLI](https://dcos.io/docs/1.9/administration/logging/quickstart/) or from the **Health** page for your service on the DC/OS dashboard.
```
+------------------------------------------------------+
| NVIDIA-SMI 352.79     Driver Version: 352.79         |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  Tesla M60           Off  | 0000:04:00.0     Off |                    0 |
| N/A   34C    P0    39W / 150W |     34MiB /  7679MiB |      0%      Default |
+-------------------------------+----------------------+----------------------+
```
