---
post_title: Quick Start
menu_order: 3.3
---


<!-- Fork a Process Inside a Mesos Container, stream its output (OSS) -->
<!-- Support Optional Stream of STDIN to Forked Process (OSS) -->
<!-- Support Optional Pseudo-Teletype for Forked Process (OSS) -->
<!-- Secure the the Debugging API with Fine Grained Auth (Enterprise) -->


**Prerequisite:**

- A container launched by using the [DC/OS Universal container runtime](/docs/1.9/usage/containerizers/).


# dcos task exec --interactive --tty task-exec-test_<unique-id> bash

In this example, a long running job app is launched and then a TTY process is launched inside of its task container for debugging.

1.  Deploy and run a job app with the DC/OS CLI:

    1.  Create the following app definition and save as `⁠⁠⁠⁠test-job.json`.
    
        ```bash
        {
          "id": "task-exec-test",
          "labels": {},
          "run": {
            "artifacts": [],
            "cmd": "cat",
            "cpus": 0.01,
            "disk": 0,
            "env": {},
            "maxLaunchDelay": 3600,
            "mem": 32,
            "placement": {
              "constraints": []
            },
            "restart": {
              "policy": "NEVER"
            },
            "volumes": []
          }
        }
        ```
    
    1.  Deploy the job app with this CLI command:
    
        ```bash
        $ dcos job add test-job.json
        ```
        
    1.  Verify that the app has been successfully deployed:
    
        ```bash
        $ dcos job list
        ID              DESCRIPTION  STATUS   LAST SUCCESFUL RUN        
        task-exec-test               Unscheduled         None
        ```

    1.  Run the job:
    
        ```bash
        $ dcos job run task-exec-test
        ```

1.  Get the task ID of the job with this CLI command:

    ```bash
    $ dcos task
    ```
    
    The output should look similar to this: 
    
    ```bash
    NAME                                HOST       USER  STATE  ID                                                                       
    20161209183121nz2F5.klueska-test    10.0.2.53  root    R    task-exec-test_<task_id>
    ```

1.  Launch a TTY process inside of the pod container with the task ID (<task_id>) specified.

    ```bash
    $ dcos task exec --interactive --tty task-exec-test_<task_id> bash
    ```
    
    You should now be inside the container running an interactive bash session.
    
    ```bash
    root@ip-10-0-2-53 / #
    ```

# dcos task exec -i …
# dcos task exec -t bash test -t 0



**Important:**

```
$ echo "kevin" | dcos task exec -i klueska-test cat
$ echo "kevin" | dcos task exec -it klueska-test cat
Must be running in a tty to pass the '--tty flag'.
```

 For more information about the `dcos task exec` command, see the CLI command [reference](/docs/1.9/usage/cli/command-reference/).