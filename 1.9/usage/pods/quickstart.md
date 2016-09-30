---
post_title: Quick Start
menu_order: 0
---

### Prerequisites
- DC/OS [installed](/docs/1.9/administration/installing/)
- DC/OS CLI [installed](/docs/1.9/usage/cli/install/)

# Launching a pod from the DC/OS CLI

1. Create a JSON application definition with contents similar to this example. In this example, we will call the file `simple-pod.json`.

  ```json
  {
    "id": "/simplepod",
    "scaling": { "kind": "fixed", "instances": 1 },
    "containers": [
      {
        "name": "sleep1",
        "exec": { "command": { "shell": "sleep 1000" } },
        "resources": { "cpus": 0.1, "mem": 32 }
      }
    ],
    "networks": [ { "mode": "host", "name": "my-virtual-network-name" } ]
  }
  ```

1. Launch the pod on DC/OS with the following DC/OS CLI command:

   ```bash
   dcos marathon pod add simple-pod.json
   ```
  

# Launching a pod from the DC/OS UI

You can also launch a pod from the [**Services**](/docs/1.9/usage/webinterface) tab of the DC/OS web interface. Select **Services** -> **Deploy Service**, then toggle to JSON mode and paste in the application definition supplied above.

After you launch your pod, you’ll see your new pod on the **Services** tab of the DC/OS web interface. Click the pod to see information about the status of the containers in your pod.
