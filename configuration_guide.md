# ToolHive Configuration Guide

This guide provides detailed information on configuring ToolHive, including the config file structure, environment variables, and command-line options. It also includes examples for common configuration scenarios.

## Table of Contents

1. [Config File Structure](#config-file-structure)
2. [Config File Location](#config-file-location)
3. [Secrets Management](#secrets-management)
4. [Client Configuration](#client-configuration)
5. [Environment Variables](#environment-variables)
6. [Command-Line Options](#command-line-options)
7. [Common Configuration Scenarios](#common-configuration-scenarios)

## Config File Structure

ToolHive uses a YAML configuration file to store its settings. The main structure of the config file is as follows:

```yaml
secrets:
  provider_type: <provider_type>
clients:
  auto_discovery: <true|false>
  registered_clients:
    - <client1>
    - <client2>
    # ...
```

## Config File Location

ToolHive uses the XDG Base Directory Specification to determine the location of the config file. By default, the config file is located at:

```
$XDG_CONFIG_HOME/toolhive/config.yaml
```

If `$XDG_CONFIG_HOME` is not set, it defaults to `~/.config/toolhive/config.yaml`.

## Secrets Management

The `secrets` section in the config file determines how ToolHive manages secrets:

```yaml
secrets:
  provider_type: <provider_type>
```

Currently, ToolHive supports two provider types:

1. `encrypted`: The default provider, which stores secrets in an encrypted format.
2. `1password`: Integrates with 1Password for secrets management.

To change the secrets provider, update the `provider_type` field in the config file.

## Client Configuration

The `clients` section in the config file manages the behavior of MCP (Model Context Protocol) clients:

```yaml
clients:
  auto_discovery: <true|false>
  registered_clients:
    - <client1>
    - <client2>
    # ...
```

- `auto_discovery`: When set to `true`, ToolHive will automatically discover and configure MCP clients. When `false`, you need to manually register clients.
- `registered_clients`: A list of manually registered MCP clients.

## Environment Variables

ToolHive respects the following environment variables:

- `XDG_CONFIG_HOME`: Overrides the default config directory.
- `THV_DEBUG`: Set to `true` to enable debug mode.

## Command-Line Options

ToolHive supports the following command-line options:

- `--debug`: Enable debug mode. This can be used instead of the `THV_DEBUG` environment variable.

Example:

```sh
thv --debug run <command>
```

## Common Configuration Scenarios

### Scenario 1: Enabling Auto-Discovery of MCP Clients

To enable auto-discovery of MCP clients, update your config file as follows:

```yaml
clients:
  auto_discovery: true
```

### Scenario 2: Manually Registering MCP Clients

If you prefer to manually register MCP clients, update your config file like this:

```yaml
clients:
  auto_discovery: false
  registered_clients:
    - client1
    - client2
    - client3
```

### Scenario 3: Changing Secrets Provider to 1Password

To use 1Password for secrets management, update your config file:

```yaml
secrets:
  provider_type: 1password
```

Note: Make sure you have 1Password CLI installed and configured on your system before making this change.

### Scenario 4: Updating Configuration Programmatically

ToolHive provides a way to update the configuration programmatically. Here's an example of how to use the `UpdateConfig` function:

```go
err := config.UpdateConfig(func(c *config.Config) {
    c.Clients.AutoDiscovery = false
    c.Clients.RegisteredClients = append(c.Clients.RegisteredClients, "new_client")
})
if err != nil {
    // Handle error
}
```

This function ensures thread-safe updates to the config file by acquiring a file lock before making changes.

Remember to always validate your configuration after making changes to ensure ToolHive operates as expected.