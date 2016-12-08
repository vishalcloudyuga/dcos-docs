---
post_title: Debugging
menu_order: 3.3
---

When things go wrong, you need to see the logs to understand how to fix it. DC/OS services and tasks write `stdout` and `stderr` files in their sandboxes by default. Traditionally, log aggregation has been the solution here. Write the logs locally and then ship all that data elsewhere for someone to actually access. Moving data around is expensive, especially when it ends up being written multiple times.

For most debugging tasks, log aggregation ends up being a particularly heavy solution for a simple task. All you want is to see the logs, where they come from doesn’t actually matter. By scoping the debugging task down to this level, we’ve been able to provide a couple simple solutions that work for most use cases.

DC/OS knows where every task has run in your cluster. It is also able to stream every file your application outputs. By combining these two features, you’re able to use the CLI or GUI to access historical and current logs such as stdout/stderr from your local machine.

Let’s say that you’ve got a service misbehaving. For some reason, it is continually crashing and you need to figure out why. You don’t need to SSH to a specific machine to find the logs and start to understand this problem. Instead, you can use the CLI or GUI to immediately get access to the files that your service is creating.