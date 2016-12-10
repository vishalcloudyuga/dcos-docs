---
post_title: Debugging
menu_order: 3.3
---

When things go wrong, you need to see the logs to understand how to fix it. DC/OS services and tasks write `stdout` and `stderr` files in their sandboxes by default. Traditionally, log aggregation has been the solution here. Write the logs locally and then ship all that data elsewhere for someone to actually access. Moving data around is expensive, especially when it ends up being written multiple times.

For most debugging tasks, log aggregation ends up being a particularly heavy solution for a simple task. All you want is to see the logs, where they come from doesn’t actually matter. By scoping the debugging task down to this level, we’ve been able to provide a couple simple solutions that work for most use cases.

DC/OS knows where every task has run in your cluster. It is also able to stream every file your application outputs. By combining these two features, you’re able to use the CLI or GUI to access historical and current logs such as stdout/stderr from your local machine.

Let’s say that you’ve got a service misbehaving. For some reason, it is continually crashing and you need to figure out why. You don’t need to SSH to a specific machine to find the logs and start to understand this problem. Instead, you can use the CLI or GUI to immediately get access to the files that your service is creating.


## Example


A good test is to launch a long running ﻿⁠⁠⁠⁠Job﻿⁠⁠⁠⁠ and then exec into it.


2) Save the following file as ﻿⁠⁠⁠⁠test-job.json﻿⁠⁠⁠⁠:
```{
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

3) Add the job and verify that it’s there:
```$ ./dcos job add test-job.json
$ ./dcos job list
ID              DESCRIPTION  STATUS   LAST SUCCESFUL RUN        
task-exec-test               Unscheduled         None
```

4) Run the job:
```$ ./dcos job run task-exec-test
```

5) Get the task id of the job:
```$ ./dcos task
NAME                                HOST       USER  STATE  ID                                                                       
20161209183121nz2F5.klueska-test    10.0.2.53  root    R    task-exec-test_<unique-id>
```

6) Start a bash session inside the task’s container:
```$ ./dcos task exec --interactive --tty task-exec-test_<unique-id> bash
root@ip-10-0-2-53 / #
```

At this point you should be inside the container running an interactive bash session. There is a known bug that the connection will drop if unused for more than 1 min, but other than that everything should be functional.