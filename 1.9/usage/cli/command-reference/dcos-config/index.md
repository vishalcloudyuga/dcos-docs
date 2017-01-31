---
post_title: dcos config
menu_order: 2
---

# Description
This command manages the DC/OS configuration file. A default configuration file is created during initial DCOS CLI setup and is located in `~/.dcos/dcos.toml`.

## Environment variables
Configuration properties all have corresponding environment variables. If a property is in the "core" section (ex. "core.foo", it corresponds to environment variable DCOS_FOO. All other properties (ex "foo.bar") correspond to environment variable DCOS_FOO_BAR.

Environment variables take precedence over corresponding configuration property.

# Usage

```bash
dcos config 
```

# Options

| Name, shorthand | Default | Description |
|---------|-------------|-------------|
| `--config-schema`   |             |  Show the configuration schema for the Marathon subcommand. |
| `--help, h`   |             |  Print usage. |
| `--info`   |             |  Print a short description of this subcommand. |
| `--version, v`   |             | Print auth version information. |

# Child commands

| Command | Description |
|---------|-------------|
| [dcos config set](/docs/1.9/usage/cli/command-reference/dcos-config/dcos-config-set/)   |             |  
| [dcos config show](/docs/1.9/usage/cli/command-reference/dcos-config/dcos-config-show/)    |             |  
| [dcos config unset](/docs/1.9/usage/cli/command-reference/dcos-config/dcos-config-unset/)    |             |  
| [dcos config validate](/docs/1.9/usage/cli/command-reference/dcos-config/dcos-config-validate/)    |             |  