---
post_title: Quick Start
menu_order: 0
---

# Prerequisites: A DC/OS Cluster, DC/OS CLI installed

# Launch a Pod from the DC/OS CLI

1. Create a JSON application definition with the following code. In this example, we will call the file `simple-pod.json`.

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
  "networks": [ { "mode": "host" }, "name": "my-virtual-network-name" ]
}
```

1. Launch the pod on DC/OS with the following DC/OS CLI command: `dcos marathon pod add simple-pod.json`.

Alternatively, you can launch a pod from the **Services** tab of the DC/OS web interface. Click **Deploy Service”, then toggle to JSON mode and paste in the application definition supplied above.

Once you launch your pod, you’ll see your new pod on the **Services** tab of the DC/OS web interface. Click the pod to see information about the status of the containers in your pod.