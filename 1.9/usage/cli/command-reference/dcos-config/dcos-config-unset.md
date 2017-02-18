---
post_title: dcos config unset
menu_order: 3
---

# Description
Remove a property from the configuration file.

# Usage

```bash
dcos config unset <name> [OPTION]
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

## Remove a config value

In this example, the `core.ssl_verify` property is removed.

```bash
$ dcos config unset core.ssl_verify
```

Here is the output:

```bash
Removed [core.ssl_verify]
```