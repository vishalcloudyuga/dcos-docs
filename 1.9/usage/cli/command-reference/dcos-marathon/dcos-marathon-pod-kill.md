---
post_title: dcos marathon pod kill
menu_order: 24
---

# Description
Kill one or more running pod instances.

# Usage

```bash
dcos marathon pod kill <instance-ids> <pod-id> [OPTION]
```

# Options

| Name, shorthand | Default | Description |
|---------|-------------|-------------|
| `--help, h`   |             |  Print usage. |
| `--info`   |             |  Print a short description of this subcommand. |
| `--version, v`   |             | Print version information. |

# Positional arguments

| Name, shorthand | Default | Description |
|---------|-------------|-------------|
| `<instance-ids>`   |             | List of one or more pod instance IDs, separated by a space. |
| `<pod-id>`   |             | The pod ID. |

# Parent command

| Command | Description |
|---------|-------------|
| [dcos marathon](/docs/1.9/usage/cli/command-reference/dcos-marathon/) | Deploy and manage applications to DC/OS. |

# Examples