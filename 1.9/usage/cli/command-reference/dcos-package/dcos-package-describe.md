---
post_title: dcos package describe
menu_order: 0
---

# Description
Get specific details for packages.

# Usage

```bash
dcos package describe [OPTION]
```

# Options

| Name, shorthand | Default | Description |
|---------|-------------|-------------|
| `--app`   |             |  Application only. |
| `--cli`   |             |  Command line only. |
| `--config`   |             | Print the configurable properties of the `marathon.json` file. |
| `--help, h`   |             |  Print usage. |
| `--info`   |             |  Print a short description of this subcommand. |
| `--options=<file>`   |             | Path to a JSON file that contains customized package installation options. |
| `<package-name>`   |             |  Name of the DC/OS package. |
| `--package-versions`   |             | Print all versions for this package. |
| `--package-version=<package-version>`   |             | The package version. |
| `--render`   |             |  Collate the `marathon.json` package template with values from the `config.json` and `--options`. If not provided, print the raw templates. |
| `--version, v`   |             | Print version information. |
        
# Parent command

| Command | Description |
|---------|-------------|
| [dcos package](/docs/1.9/usage/cli/command-reference/dcos-node/dcos-package/dcos-package/)   | Install and manage DC/OS software packages. |