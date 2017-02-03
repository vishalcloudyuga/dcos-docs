---
post_title: dcos node diagnostics download
menu_order: 3
---
    
# Description
View the details of diagnostics bundles.

# Usage

```bash
dcos node diagnostics download [OPTION]
```

# Options

| Name, shorthand | Default | Description |
|---------|-------------|-------------|
| `--help, h`   |             |  Print usage. |
| `--info`   |             |  Print a short description of this subcommand. |
| `--location=<location>`   |  Current directory |  Download the diagnostics bundle to a specific location. If not set, the default location is your current working directory. |
| `--version, v`   |             | Print version information. |

# Positional arguments

| Name, shorthand | Default | Description |
|---------|-------------|-------------|
| `<bundle>`   |             |  The bundle filename. For example, `bundle-2017-02-01T00:33:48-110930856.zip`. |

# Parent command

| Command | Description |
|---------|-------------|
| [dcos node](/docs/1.9/usage/cli/command-reference/dcos-node/) | View DC/OS node information. | 

# Examples

