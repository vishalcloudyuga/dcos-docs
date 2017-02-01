---
post_title: dcos node diagnostics
menu_order: 5
---
    
# Description
View the details of diagnostics bundles.

# Usage

```bash
dcos node diagnostics [OPTION]
```

# Options

| Name, shorthand | Default | Description |
|---------|-------------|-------------|
| `--cancel`   |             | Cancel a running diagnostics job. |
| `--help, h`   |             |  Print usage. |
| `--info`   |             |  Print a short description of this subcommand. |
| `--list`   |             |  List the available diagnostics bundles. |
| `--json`   |             |  Print JSON-formatted data. |
| `--status`   |             |  Print diagnostics job status. |
| `--version, v`   |             | Print version information. |



    dcos node diagnostics (--list | --status | --cancel) [--json]
    dcos node diagnostics create (<nodes>)...
    dcos node diagnostics delete <bundle>
    dcos node diagnostics download <bundle> [--location=<location>]
    dcos node list-components [--leader --mesos-id=<mesos-id> --json]
    dcos node log [--follow --lines=N --leader --master --mesos-id=<mesos-id> --slave=<slave-id>]
                  [--component=<component-name> --filter=<filter>...]
    dcos node ssh [--option SSHOPT=VAL ...]
                  [--config-file=<path>]
                  [--user=<user>]
                  [--master-proxy]
                  (--leader | --master | --mesos-id=<mesos-id> | --slave=<slave-id>)
                  [<command>]

Commands:
    diagnostics
        View the details of diagnostics bundles.
    diagnostics create
        Create a diagnostics bundle.
    diagnostics download
        Download a diagnostics bundle.
    diagnostics delete
        Delete a diagnostics bundle.
    log
        Print the Mesos logs for the leading master node, agent nodes, or both.
    ssh
        Establish an SSH connection to the master or agent nodes of your DC/OS
        cluster.

Options:
    --cancel
        Cancel a running diagnostics job.
    --component=<component-name>
        Show DC/OS component logs.
    --config-file=<path>
        Path to SSH configuration file.
    --filter=<filter>
        Filter logs by field and value. Filter must be a string separated by colon.
        For example: --filter _PID:0 --filter _UID:1.
    --follow
        Dynamically update the log.
    -h, --help
        Show this screen.
    --info
        Show a short description of this subcommand.
    --json
        Print JSON-formatted list of nodes.
    --leader
        The leading master.
    --lines=N
        Print the last N lines, where 10 is the default.
    --list
        List available diagnostics bundles.
    --list-components
        Print a list of available DC/OS components on specified node.
    --location=<location>
        Download a diagnostics bundle to a particular location.
        If not set, default to present working directory.
    --master
        Deprecated. Please use --leader.
    --master-proxy
        Proxy the SSH connection through a master node. This can be useful when
        accessing DC/OS from a separate network. For example, in the default AWS
        configuration, the private slaves are unreachable from the public
        internet. You can access them using this option, which will first hop
        from the publicly available master.
    --option SSHOPT=VAL
        The SSH options. For information, enter `man ssh_config` in your
        terminal.
    --slave=<agent-id>
        Agent node with the provided ID.
    --status
        Print diagnostics job status.
    --user=<user>
        The SSH user, where the default user [default: core].
    --version
        Print version information.

Positional Arguments:
    <bundle>
        The bundle filename. For example, `bundle-2017-02-01T00:33:48-110930856.zip`.
    <command>
        Command to execute on the DCOS cluster node.
    <nodes>
        Node to run command upon. A node can be any of the following: IP address, hostname, Mesos ID, or the keywords "all", "masters", "agents". You must use quotation marks around keywords.

By default, `dcos node ssh` connects to the private IP of the node, which is only accessible from hosts within the same network, so you must use the `--master-proxy` option to access your cluster from an outside network. For example, in the default AWS configuration, the private agents are unreachable from the public internet, but you can SSH to them using this option, which will proxy the SSH connection through the publicly reachable master.

# Parent command

| Command | Description |
|---------|-------------|
| [dcos node](/docs/1.9/usage/cli/command-reference/dcos-node/) | View DC/OS node information. | 

# Examples

