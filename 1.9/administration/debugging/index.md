---
post_title: Debugging
menu_order: 3.3
---

When things go wrong, you need to see the logs to understand how to fix it. DC/OS services and tasks write `stdout` and `stderr` files in their sandboxes by default. Traditionally, log aggregation has been the solution here. Write the logs locally and then ship all that data elsewhere for someone to actually access. Moving data around is expensive, especially when it ends up being written multiple times.

For most debugging tasks, log aggregation ends up being a particularly heavy solution for a simple task. All you want is to see the logs, where they come from doesn’t actually matter. By scoping the debugging task down to this level, we’ve been able to provide a couple simple solutions that work for most use cases.

DC/OS knows where every task has run in your cluster. It is also able to stream every file your application outputs. By combining these two features, you’re able to use the CLI or GUI to access historical and current logs such as stdout/stderr from your local machine.

Let’s say that you’ve got a service misbehaving. For some reason, it is continually crashing and you need to figure out why. You don’t need to SSH to a specific machine to find the logs and start to understand this problem. Instead, you can use the CLI or GUI to immediately get access to the files that your service is creating.

<!-- Fork a Process Inside a Mesos Container, stream its output (OSS) -->
<!-- Support Optional Stream of STDIN to Forked Process (OSS) -->
<!-- Support Optional Pseudo-Teletype for Forked Process (OSS) -->
<!-- Secure the the Debugging API with Fine Grained Auth (Enterprise) -->


## Example

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

 For more information about the `dcos task exec` command, see the CLI command [reference](/docs/1.9/usage/cli/command-reference/).