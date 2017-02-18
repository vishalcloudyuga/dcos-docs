---
post_title: Enable GPU Acceleration
nav_title: Enable GPU Acceleration
feature_maturity: experimental
menu_order: 110
---

DC/OS now supports allocating GPUs (Graphics Processing Units) to your long-running DC/OS services. Adding GPUs to your service can dramatically accelerate big data workloads.  [Learn more](http://www.nvidia.com/object/what-is-gpu-computing.html).

# Configure your cluster for GPUs

Follow the steps in this topic to enable GPU support on your cluster. We provide instructions for both AWS cloud and custom installations.

**Important:** All machines in your cluster that have GPUs must have the [Nvidia Management Library (NVML)](https://developer.nvidia.com/nvidia-management-library-nvml) installed on them.

## Further reading

- [Mesos-specific documentation](https://github.com/apache/mesos/blob/master/docs/gpu-support.md)
- Presentation: [Supporting GPUs in Docker Containers on Apache Mesos](https://docs.google.com/presentation/d/1FnuEW2ic5d-cpSyVOUMfUSM7WxJlZtTAAWt2dZXJ52A/edit#slide=id.p)
- Presentation: [GPU Support in Apache Mesos](https://www.youtube.com/watch?v=giJ4GXFoeuA)
- Presentation: [Adding GPU Support to Mesos](https://docs.google.com/presentation/d/1Y1IUlWV6g1HzD1wYIYXy6AmbfnczWfjvvmqqpeDFBic/edit#slide=id.p)
