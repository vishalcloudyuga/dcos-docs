---
post_title: dcos job show runs
menu_order: 11
---
    
# Description
Show the successful and failure status of job runs.

# Usage

```bash
dcos job show runs <job-id> [OPTION]
```

# Options

| Name, shorthand | Default | Description |
|---------|-------------|-------------|
| `--help, h`   |             |  Print usage. |
| `--info`   |             |  Print a short description of this subcommand. |
| `--json`   |             |  Print JSON-formatted list. |
| `--q`   |             |  Print an array of run IDs only. |
| `--run-id <run-id>`   |             |  The ID of a job run. |
| `--version, v`   |             | Print version information. |

# Positional arguments

| Name, shorthand | Default | Description |
|---------|-------------|-------------|
| `<job-id>`   |             |  Specify the job ID. |

# Parent command

| Command | Description |
|---------|-------------|
| [dcos job](/docs/1.9/usage/cli/command-reference/dcos-job/) |  Deploy and manage jobs in DC/OS. |

# Examples