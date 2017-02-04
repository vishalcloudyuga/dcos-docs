---
post_title: dcos task ls
menu_order: 1
---

# Description
Print the list of files in the Mesos task sandbox.

# Usage

```bash
dcos task ls [OPTION]
```

# Options

| Name, shorthand | Default | Description |
|---------|-------------|-------------|
| `--completed`   |             | Print completed and in-progress tasks. |
| `--help, h`   |             |  Print usage. |
| `--info`   |             |  Print a short description of this subcommand. |
| `--long`   |             |  Print full Mesos sandbox file attributes. |
| `--version, v`   |             | Print version information. | 

# Positional arguments

| Name, shorthand | Default | Description |
|---------|-------------|-------------|
| `<task>`   |             |  A full task ID, a partial task ID, or a regular expression. |
| `<path>`   |     `.`      |  The Mesos sandbox directory path. |

# Parent command

| Command | Description |
|---------|-------------|
| [dcos task](/docs/1.9/usage/cli/command-reference/dcos-task/)   | Manage DC/OS tasks. |  