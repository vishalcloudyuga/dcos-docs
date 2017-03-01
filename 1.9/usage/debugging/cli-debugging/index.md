---
post_title: Debugging from the DC/OS CLI
feature_maturity: experimental
menu_order: 0
---

The DC/OS CLI provides a set of debugging subcommands, `dcos marathon debug`. A large portion of this functionality is also available from the DC/OS GUI.

You can also use the [`dcos task exec` command](/docs/1.9/usage/debugging/task-exec/) to execute an arbitrary command inside of a task's container and stream its output back to your local terminal. This can help you learn more about how a given task is behaving.

# Using dcos marathon debug

## Prerequisites
- A DC/OS cluster.
- The DC/OS CLI installed.
- A failing service or pod.

## dcos marathon debug list

The `dcos marathon debug list` command shows you all services that are in a waiting state, enabling you to see just those services that are not running.

```bash
dcos marathon debug list

ID            SINCE                     INSTANCES TO LAUNCH  WAITING  PROCESSED OFFERS  UNUSED OFFERS  LAST UNUSED OFFER         LAST USED OFFER           
/mem-app      2017-02-28T19:08:59.547Z  3                    True     13                13             2017-02-28T19:09:35.607Z  ---                       
/stuck-sleep  2017-02-28T19:09:25.56Z   9                    True     8                 7              2017-02-28T19:09:35.608Z  2017-02-28T19:09:25.566Z
```

The output of the command shows how many instances of the service or pod still need to launch, how many Mesos resource offers have been processed, how many Mesos resource offers have been unused, and the since the last unused and used offer. Taken together, this output can show you at a glance which services or pods are failing, how badly they are failing, and how long they have been failing.

<!-- what does `SINCE` mean? -->

## dcos marathon debug summary

Once you know which services or pods are performing poorly, use the `dcos marathon debug summary /<app-id>|/<pod-id>` command to learn more about a particular failing service or pod.

```bash
dcos marathon debug summary /mem-app

RESOURCE     REQUESTED                 MATCHED  PERCENTAGE  
ROLE         [*]                       1 / 2    50.00%      
CONSTRAINTS  [['hostname', 'UNIQUE']]  1 / 1    100.00%     
CPUS         0.1                       1 / 1    100.00%     
MEM          12000                     0 / 1    0.00%       
DISK         0                         0 / 0    ---         
PORTS        ---                       0 / 0    ---  
```

The output of the command shows the resources, what the service or pod requested, how many offers were matched, and the percentage of offers that were matched. This command can show you quickly which resource requests are not being met.

## dcos marathon debug details

The `dcos marathon debug details /<app-id>|/<pod-id>` command lets you drill down further to learn exactly how you need to change your service or pod definition.

```bash
dcos marathon debug details /mem-app

HOSTNAME    ROLE  CONSTRAINTS  CPUS  MEM  DISK  PORTS  RECEIVED                  
10.0.0.193   ok        ok       ok    -    ok     ok   2017-02-28T23:25:11.912Z  
10.0.4.126   -         ok       -     -    ok     -    2017-02-28T23:25:11.913Z
```

The output of the command shows which hosts are running the service or pod, and then the status of the role, constraints, CPUs, memory, disk, and ports the service or pod has requested. <!-- what does received mean here? is it when the offer was recevied? --> In the example above, you can see that one instance of `/mem-app` has a status of `ok` in all categories except memory. The other instance had fewer successful resource matches, with role, CPUs, memory, and ports having no match. With this information, you know more about where and how you need to modify your service or pod definition.
