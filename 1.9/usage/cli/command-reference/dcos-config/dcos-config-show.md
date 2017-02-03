---
post_title: dcos config show
menu_order: 2
---

# Description
Print the DC/OS configuration file contents.

# Usage

```bash
dcos config show [OPTION]
``` 

# Options

| Name, shorthand | Default | Description |
|---------|-------------|-------------|
| `--help, h`   |             |  Print usage. |
| `--info`   |             |  Print a short description of this subcommand. |
| `--version`   |             |  Print version information. |

# Positional arguments

| Name, shorthand | Default | Description |
|---------|-------------|-------------|
| `<name>`   |             |  The name of the property. |

# Parent command

| Command | Description |
|---------|-------------|
| [dcos config](/docs/1.9/usage/cli/command-reference/dcos-config/) |  Manage DC/OS configuration. |

# Examples

## View a specific config value
In this example, the DC/OS URL is shown:

```bash
$ dcos config show core.dcos_url
```

The output should resemble:

```bash
https://your-cluster-9vqnkrq5pt2n-2781474.cloue-1.elb.amazonaws.com
```

## View all config values

```bash
$ [core.dcos_url]: set to 'https://your-cluster-9vqnkrq5pt2n-2781474.cloue-1.elb.amazonaws.com'
```

The output should resemble:

```bash
core.dcos_url https://your-cluster-9vqnkrq5pt2n-2781474.cloue-1.elb.amazonaws.com
core.ssl_verify false
```


