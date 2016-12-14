---
post_title: Quick Start
menu_order: 3.4
---

Use this guide to get started with DC/OS logging. 

# Make Mesos logs available in journald

1.  Create the following Marathon app definition and save as `test-log.json`:

    ```json
    {
      "id": "test-log",
      "labels": {},
      "run": {
        "artifacts": [],
        "cmd": "while true;do echo stdout;echo stderr >&2;sleep 1;done",
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
    $ dcos task
    ```
    
    The output should resemble:
    
    ```bash
    
    ```



Deploy marathon app that writes to stdout and stderr. For example:
"cmd": "while true;do echo stdout;echo stderr >&2;sleep 1;done"

Get task ID with CLI by running dcos task
Get stdout logs with command dcos task log <task_id>
Get stderr logs with command dcos task log <task_id> stderr
Follow logs with command dcos task log --follow <task_id>
Get last N log entries dcos task log <task_id> --lines=5