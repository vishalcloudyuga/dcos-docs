---
post_title: dcos service
menu_order: 8
--- 

# Description

# Usage

# Options

# Parent command

# Examples

# dcos service

    Description:
        Manage DC/OS services.
    
    Usage:
        dcos service --info
        dcos service [--completed --inactive --json]
        dcos service log [--follow --lines=N --ssh-config-file=<path>]
                         <service> [<file>]
        dcos service shutdown <service-id>
    
    Commands:
        log
            Print the service logs.
    
        shutdown
            Shutdown a service.
    
    
    Options:
        --completed
            Show the completed and active services. Completed services have either
            been disconnected from master and reached their failover timeout, or
            have been explicitly shutdown via the /shutdown endpoint.
        -h, --help
            Print usage.
        --follow
            Print data as the log file grows.
        --inactive
            Show the inactive and active services. Inactive services have been
            disconnected from master, but haven't yet reached their failover timeout.
        --info
            Print a short description of this subcommand.
        --json
            Print JSON-formatted list of DC/OS services.
        --lines=N
            Print the last N lines, where 10 is the default.
        --ssh-config-file=<path>
            The path to the SSH config file. This is used to access the Marathon
            logs.
        --version
            Print version information.
    
    Positional Arguments:
        <file>
            The service log filename for the Mesos sandbox. The default is stdout.
        <service>
            The DC/OS Service name.
        <service-id>
            The DC/OS Service ID.
    

**Important:** To view the native DC/OS Marathon logs by using the `dcos service log marathon` command, you must be on the same network or connected by VPN to your cluster. For more information, see [Accessing native DC/OS Marathon logs](/docs/1.9/administration/logging/quickstart/).

