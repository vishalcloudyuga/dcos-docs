---
post_title: Limitations and Caveats
nav_title: Limitations
feature_maturity: experimental
menu_order: 3
---

- Unlike other resources, like  CPUs, memory, and disk, you can only specify whole numbers of GPUs in your application definition. If a fractional amount is selected, launching the task will result in a `TASK_ERROR`.

- Nvidia GPU support is only available for tasks launched through the [DC/OS Universal container runtime](/docs/1.9/usage/containerizers/). No support exists for launching GPU-capable tasks through the Docker containerizer. However, the DC/OS Universal container runtime supports running Docker images natively.

- While GPU resources are advertised to the Mesos master alongside other resources like CPUs, memory, and disk, the master will only forward offers that contain GPUs to DC/OS services that have explicitly enabled the GPU_RESOURCES capability. Below is an example of setting this capability in a C++-based service.
  ```c++
  FrameworkInfo framework;
framework.add_capabilities()->set_type(
      FrameworkInfo::Capability::GPU_RESOURCES);

GpuScheduler scheduler;

driver = new MesosSchedulerDriver(
    &scheduler,
    framework,
    127.0.0.1:5050);

driver->run();
  ```
