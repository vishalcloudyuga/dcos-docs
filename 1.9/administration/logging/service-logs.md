---
post_title: Service and Task Logging
menu_order: 0
---

As soon as you move from one machine to many, accessing and aggregating logs becomes difficult. Once you hit a certain scale, keeping these logs and making them available to others can add massive overhead to your cluster. After watching how users interact with their logs, we’ve scoped the problem to two primary use cases. This allows you to pick the solution with the lowest overhead that solves your specific problem.

## CLI

If you’ve created a DC/OS service named `service` and would like to see stdout for every instance of that in real time, you can run the following:

```
$ dcos task log --follow service
```

For more advanced usage, you can check out the CLI documentation:

- [DC/OS CLI Usage][1]

## GUI

From the **Services** tab in the [DC/OS UI](/docs/1.9/usage/webinterface/) you can download all the log files for your service. You can also monitor stdout/stderr.

# Compliance

Unfortunately, streaming logs from machines in your cluster isn’t always viable. Sometimes, you need the logs stored somewhere else as a history of what’s happened. This is where log aggregation really is required. Check out how to get it setup with some of the most common solutions:

- [ELK][2]
- [Splunk][3]

[1]: /docs/1.9/usage/cli/
[2]: ../elk/
[3]: ../splunk/
