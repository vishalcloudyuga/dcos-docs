---
post_title: Pods CLI
feature_maturity: experimental
menu_order: 20
---

You can create and manage your pods via the DC/OS CLI. The following commands are available:

* `dcos marathon pod add [<pod-resource>]`
* `dcos marathon pod list [--json]`
* `dcos marathon pod remove [--force] <pod-id>`
* `dcos marathon pod show <pod-id>`
* `dcos marathon pod update [--force] <pod-id>`

# Add a Pod

To add a pod, first create a JSON pod definition. Then, run the following command:
```
$ dcos marathon pod add <pod-json-file>
```

# List Pods
List pods and the number of containers they have with the following command:
```
$ dcos marathon pod list
```

# Remove a Pod
Remove a pod with the following command:
```
$ dcos marathon pod remove <pod-id>
```

If the pod is currently deploying, you will not be able to remove the pod. To remove the pod anyway, run the command with the `--force` flag.

# Show Pod JSON
To see the pod definition, run the following command:
```
$ dcos marathon pod show <pod-id>
```
You can use the `show` command to read data about the pod programmatically.

# Update Pod
To update a pod, first modify the JSON definition for the pod, then run the following command: 

```
$ dcos marathon pod update <pod-id> < <new-pod-definition>
```

If the pod is currently deploying, you will not be able to update the pod. To update the pod anyway, run the command with the `--force` flag.