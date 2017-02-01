---
post_title: dcos marathon app add
menu_order: 1
---

# Description
Add an application.

# Usage

```bash
dcos marathon app add [OPTION]
```

# Options

| Name, shorthand | Default | Description |
|---------|-------------|-------------|
| `<app-resource>`   |             |  Path to a file or HTTP(S) URL that contains the app's JSON definition. If omitted, the definition is read from stdin. For a detailed description see the [documentation](https://docs.mesosphere.com/usage/marathon/rest-api/). |
| `--help, h`   |             |  Print usage. |
| `--version, v`   |             | Print version information. |


# Parent command

| Command | Description |
|---------|-------------|
| [dcos marathon](/docs/1.9/usage/cli/command-reference/dcos-marathon/) | Deploy and manage applications to DC/OS. |

# Examples