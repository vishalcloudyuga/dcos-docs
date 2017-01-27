---
post_title: dcos config
menu_order: 2
--- 

# Description
This command the DC/OS configuration file. A default configuration file is created during initial DCOS CLI setup and is located in `~/.dcos/dcos.toml`.

# Usage

```bash
dcos config set <name> <value>
dcos config show [<name>]
dcos config unset <name>
dcos config validate
```

# Child commands

| Command | Description |
|---------|-------------|
| `dcos config set`   |             |  
| `dcos config show`   |             |  
| `dcos config unset`   |             |  
| `dcos config validate`   |             |  

# Options

| Name, shorthand | Default | Description |
|---------|-------------|-------------|
| `--`   |             |  Enable debug mode. |

# Parent command

# Examples


Commands:
    set
        Add or set a DC/OS configuration property.
    show
        Print the DC/OS configuration file contents.
    unset
        Remove a property from the configuration file.
    validate
        Validate changes to the configuration file.

Options:
    -h, --help
        Print usage.
    --info
        Print a short description of this subcommand.
    --version
        Print version information.

Positional Arguments:
    <name>
        The name of the property.
    <value>
        The value of the property.

Environment Variables:
    Configuration properties all have corresponding environment variables. If a
    property is in the "core" section (ex. "core.foo"), it corresponds to
    environment variable DCOS_FOO. All other properties (ex "foo.bar")
    correspond to enviroment variable DCOS_FOO_BAR.

    Environment variables take precendence over corresponding configuration
    property.
```