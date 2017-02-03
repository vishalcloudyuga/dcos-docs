---
post_title: Enable GPU Acceleration
nav_title: Enable GPU Acceleration
feature_maturity: experimental
menu_order: 110 
---

DC/OS now supports allocating GPUs (Graphics Processing Units) to your long-running DC/OS services. Adding GPUs to your service can dramatically accelerate big data workloads.  [Learn more](http://www.nvidia.com/object/what-is-gpu-computing.html).

By default, DC/OS is not configured to run with GPUs, so you must explicitly enable this support when first installing DC/OS on your cluster. Once configured, DC/OS treats  GPUs as another resource available to your service.

# Configure your Cluster for GPUs

Follow the steps in this topic to enable GPU support on your cluster. We provide instructions for both AWS cloud and custom installations.

**Important:** All machines in your cluster must have the [Nvidia Management Library (NVML)](https://developer.nvidia.com/nvidia-management-library-nvml) installed on them. The machines do not need to have GPUs on them, but they will fail to to come online without this library if GPU support is enabled as described below.