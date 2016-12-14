---
post_title: Quick Start
menu_order: 0
---

Use this guide to get started with DC/OS logging. 

# Make Mesos logs available in journald

You can access the Mesos logs natively through the DC/OS CLI `dcos task log` command. In this example, a task is launched and the stderr and stdout logs from Mesos are accessed. 

1.  Create the following Marathon app definition and save as `test-log.json`.
    
    ```json
    {
      "id": "/test-log",
      "cmd": "while true;do echo stdout;echo stderr >&2;sleep 1;done",
      "cpus": 0.001,
      "instances": 1,
      "mem": 128
    }
    ```

1.  Deploy the app with this CLI command:
    
    ```bash
    $ dcos marathon app add test-log.json
    ```

1.  Verify that the app has been successfully deployed and note task ID:

    ```bash
    $ dcos task
    ```
    
    The output should resemble:
    
    ```bash
    NAME                                HOST        USER  STATE  ID
    test-log                            10.0.1.105  root    R    test-log.e69c4b2f-c255-11e6-a451-aa711cbcaa78
    ```

1.  Run this command to view the stdout logs, where `<task_id>` is the task ID:

    ```bash
    $ dcos task log <task_id>
    ```

1.  Run this command to view the stderr logs, where `<task_id>` is the task ID:

    ```bash
    $ dcos task log <task_id>
    ```
    
1.  Run this command to follow the logs, where `<task_id>` is the task ID:

    ```bash
    dcos task log --follow <task_id>
    ```
    
1.  Run this command to get last *N* log entries:
 
    ```bash
    dcos task log <task_id> --lines=5
    ```

# 

1.  Create the following Marathon app definition and save as `test-log.json`.
    
    ```json
    {
      "id": "/test-log",
      "cmd": "while true;do echo stdout;echo stderr >&2;sleep 1;done",
      "cpus": 0.001,
      "instances": 1,
      "mem": 128
    }
    ```

1.  Deploy the app with this CLI command:
    
    ```bash
    $ dcos marathon app add test-log.json
    ```

1.  Verify that the app has been successfully deployed and note task ID:

    ```bash
    $ dcos task
    ```
    
    The output should resemble:
    
    ```bash
    NAME                                HOST        USER  STATE  ID
    test-log                            10.0.1.105  root    R    test-log.e69c4b2f-c255-11e6-a451-aa711cbcaa78
    ```

1.  

Get agent nodes IDs with command dcos node
Get logs from leader with command dcos node log --leader
Get logs from agent with command dcos node log --mesos-id <node-id>
Get a list of components running on leader or agent node with a commands:
dcos node list-components --leader
dcos node list-componenets --mesos-id <node-id>
Get marathon logs from leader with command
dcos node log --leader --component dcos-marathon.service