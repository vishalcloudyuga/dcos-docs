---
post_title: Configure Your Cluster
nav_title: Configure Your Cluster
feature_maturity: experimental
menu_order: 1
---

Follow the steps below to configure a custom DC/OS installation.

1. Install the [Nvidia Management Library (NVML)](https://developer.nvidia.com/nvidia-management-library-nvml) on each node of your cluster that has GPUs, unless it is already installed. Find detailed installation instructions [here](https://github.com/apache/mesos/blob/master/docs/gpu-support.md#external-dependencies).

1. Create [an application definition that employs GPUs](/docs/1.9/usage/gpu/gpu-app-definition).
