---
post_title: dcos task log
menu_order: 0
---

# Description
Print the task log.

# Usage

```bash
dcos task log [OPTION]
```

# Options

| Name, shorthand | Default | Description |
|---------|-------------|-------------|
| `--completed`   |             | Print completed and in-progress tasks. |
| `--follow`   |             |  Dynamically update the log. |
| `--help, h`   |             |  Print usage. |
| `--info`   |             |  Print a short description of this subcommand. |
| `--lines=N`   |     10      |  Print the last N lines. |
| `--version, v`   |             | Print version information. | 

# Positional arguments

| Name, shorthand | Default | Description |
|---------|-------------|-------------|
| `<file>`   |  stdout  |  Specify the sandbox file to print. |
| `<task>`   |             |  A full task ID, a partial task ID, or a regular expression. |

# Parent command

| Command | Description |
|---------|-------------|
| [dcos task](/docs/1.9/usage/cli/command-reference/dcos-task/)   | Manage DC/OS tasks. | 