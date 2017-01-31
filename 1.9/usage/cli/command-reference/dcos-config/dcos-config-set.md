---
post_title: dcos config set
menu_order: 1
---

# Description
This command adds or sets a DC/OS configuration property.

# Usage

```bash
dcos config set [OPTION]
```

# Options

| Name, shorthand | Default | Description |
|---------|-------------|-------------|
| `--help, h`   |             |  Print usage. |
| `--info`   |             |  Print a short description of this subcommand. |
| `--version`   |             |  Print version information. |
| `<name>`   |             |  The name of the property. |
| `<value>`   |             |  The value of the property. |

# Parent command

| Command | Description |
|---------|-------------|
| [dcos config](/docs/1.9/usage/cli/command-reference/dcos-config/) |  Manage DC/OS configuration. |


# Examples

## Configure CLI to DC/OS Cluster

```bash
$ dcos config set core.dcos_url cluster_url
```

The output should resemble:

```bash

```

## Set SSL setting

``bash
$ dcos config set core.ssl_verify True
```


