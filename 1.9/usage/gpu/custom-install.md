---
post_title: Configure Your Cluster
nav_title: Configure Your Cluster
feature_maturity: experimental
menu_order: 1 
---

Follow the steps below to configure a custom DC/OS installation.

1. Install the [Nvidia Management Library (NVML)](https://developer.nvidia.com/nvidia-management-library-nvml) on each node of your cluster, unless it is already installed. Find detailed installation instructions [here](https://github.com/apache/mesos/blob/master/docs/gpu-support.md#external-dependencies).

1. Add the `enable_gpu_isolation: true` flag to your `config.yaml` when you [install DC/OS on your cluster](/docs/1.8/administration/installing/custom/).