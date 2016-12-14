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


# Launch a long running interactive Bash session

dcos task exec --interactive --tty task-exec-test_<unique-id> bash

In this example, a long running job app is launched and then a TTY process is launched inside of its task container for debugging.

1.  Deploy and run a job app with the DC/OS CLI:

    1.  Create the following app definition and save as `⁠⁠⁠⁠test-job.json`. This specifies a sleep job that runs for `10000000` seconds.
    
        ```bash
        {
          "id": "task-exec-test",
          "labels": {},
          "run": {
            "artifacts": [],
            "cmd": "sleep 100000000",
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

1.  Launch a TTY process inside of the container with the task ID (<task_id>) specified. This will launch an interactive Bash session.

    ```bash
    $ dcos task exec --interactive --tty task-exec-test_<task_id> bash
    ```
    
    You should now be inside the container running an interactive Bash session.
    
    ```bash
    root@ip-10-0-2-53 / #
    ```
    
1.  Run a command from the interactive Bash session from inside the cluster. For example, the `ls` command:

    ```bash
    root@ip-10-0-1-104 / # ls
    bin   dev  home  lib64	     media  opt   root	sbin  sys  usr
    boot  etc  lib	 lost+found  mnt    proc  run	srv   tmp  var
    ```

# Pipe output from a command running inside a container 

You can run commands inside a container by using the `dcos exec` command. In this example, the `dcos task exec` command is used to get the hostname of the node running your app. 

1.  Create a Marathon app definition and name it `my-app.json` with the following contents:

    ```bash
    {
       "id": "/my-app",
       "cmd": "sleep 100000000",
       "instances": 1,
       "cpus": 1,
       "mem": 128,
       "disk": 0,
       "gpus": 0,
       "backoffSeconds": 1,
       "backoffFactor": 1.15,
       "maxLaunchDelaySeconds": 3600,
       "upgradeStrategy": {
         "minimumHealthCapacity": 1,
         "maximumOverCapacity": 1
       },
       "portDefinitions": [
         {
           "protocol": "tcp",
           "port": 10000
         }
       ],
       "requirePorts": false
     }
     ```

1.  Deploy the service on DC/OS:

    ```bash
    $ dcos marathon app add my-app.json
    ```

1.  Get the task ID of the job with this CLI command:

    ```bash
    $ dcos task
    ```
    
    The output should look similar to this: 
    
    ```bash
    NAME        HOST        USER  STATE  ID                                               
    my-app  10.0.1.106  root    R    my-app.<task-ID>
    ```

1.  Run this command to show the hostname of the container running your app, where `<task-ID>` is your task ID.

    ```bash
    $ dcos task exec my-app.<task-ID> hostname
    ```
    
    The output should look similar to this:
    
    ```bash
    ip-10-0-1-105.us-west-2.compute.internal
    ```

For more information about the `dcos task exec` command, see the CLI command [reference](/docs/1.9/usage/cli/command-reference/).

# Run an interactive command on a remote machine
You can run interactive commands on machines in your cluster by using the `dcos exec command`. In this example, a file is copied from your local machine to a cluster node.

1.  Create a Marathon app definition and name it `my-app.json` with the following contents:

    ```bash
    {
       "id": "/my-app",
       "cmd": "sleep 100000000",
       "instances": 1,
       "cpus": 1,
       "mem": 128,
       "disk": 0,
       "gpus": 0,
       "backoffSeconds": 1,
       "backoffFactor": 1.15,
       "maxLaunchDelaySeconds": 3600,
       "upgradeStrategy": {
         "minimumHealthCapacity": 1,
         "maximumOverCapacity": 1
       },
       "portDefinitions": [
         {
           "protocol": "tcp",
           "port": 10000
         }
       ],
       "requirePorts": false
     }
     ```

1.  Deploy the service on DC/OS:

    ```bash
    $ dcos marathon app add my-app.json
    ```

1.  Get the task ID of the job with this CLI command:

    ```bash
    $ dcos task
    ```
    
    The output should look similar to this: 
    
    ```bash
    NAME        HOST        USER  STATE  ID                                               
    my-app  10.0.1.106  root    R    my-app.<task-ID>
    ```

1.  Run this command to copy a file to your cluster node. In this example a Marathon app definition is added. 
 
    ```bash
    $ cat my-app-cli.json | dcos task exec --interactive my-app-cli.a15eb2ae-c232-11e6-a451-aa711cbcaa78 cat my-app-cli.json
    ```

an interactive, non-tty-based command to feed data into the command running on the remote machine (e.g. you can use `cat some-file | dcos task exec —interactive <task_id> cat - some-file` to copy a file into the remote container).