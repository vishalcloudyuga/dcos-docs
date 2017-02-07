---
post_title: dcos experimental package build
menu_order: 1
---
    
# Description
Build a package locally to be added to DC/OS or to be shared with Universe.

# Usage

```bash
dcos experimental package build [OPTION]
```

# Options

| Name, shorthand | Default | Description |
|---------|-------------|-------------|
| `--help, h`   |             |  Print usage. |
| `--info`   |             |  Print a short description of this subcommand. |
| `--json`   |             |  JSON-formatted data. |
| `--output-directory=<output-directory>`   | current working directory | Path to the directory where the data should be stored.|
| `--version, v`   |             | Print version information. |  
    
# Positional arguments

| Name, shorthand | Default | Description |
|---------|-------------|-------------|
| `<build-definition>`   |             |  Path to a DC/OS package build definition. |

# Parent command

| Command | Description |
|---------|-------------|
| [dcos experimental](/docs/1.9/usage/cli/command-reference/dcos-experimental/)   |  Manage commands that under development and subject to change. |     