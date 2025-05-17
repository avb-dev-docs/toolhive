---
title: ToolHive Configuration Guide
description: A comprehensive guide to configuring ToolHive, including client auto-discovery, secrets management, and MCP server permissions.
---

# ToolHive Configuration Guide

## Introduction

This guide provides detailed information on configuring ToolHive, a powerful tool for managing and interacting with MCP (Model Control Protocol) servers. We'll cover all available configuration options, including setting up client auto-discovery, configuring secrets management, and customizing permissions for MCP servers.

## Table of Contents

1. [Configuration File](#configuration-file)
2. [Client Auto-Discovery](#client-auto-discovery)
3. [Secrets Management](#secrets-management)
4. [MCP Server Permissions](#mcp-server-permissions)
5. [Supported MCP Clients](#supported-mcp-clients)

## Configuration File

ToolHive uses a YAML configuration file to store its settings. By default, this file is located at:

- Linux: `~/.config/toolhive/config.yaml`
- macOS: `~/Library/Application Support/toolhive/config.yaml`
- Windows: `%APPDATA%\toolhive\config.yaml`

If the configuration file doesn't exist, ToolHive will create one with default values when you first run the application.

Here's an example of the configuration file structure:

```yaml
secrets:
  provider_type: encrypted
clients:
  auto_discovery: true
  registered_clients: []
```

## Client Auto-Discovery

ToolHive can automatically discover and configure MCP clients on your system. This feature is controlled by the `auto_discovery` setting in the configuration file.

To enable or disable auto-discovery:

1. Open the configuration file in a text editor.
2. Locate the `clients` section.
3. Set `auto_discovery` to `true` to enable, or `false` to disable.

Example:

```yaml
clients:
  auto_discovery: true
```

When auto-discovery is disabled, you can manually specify which clients to configure using the `registered_clients` list:

```yaml
clients:
  auto_discovery: false
  registered_clients:
    - vscode
    - cursor
```

Valid client names are: `roo-code`, `cursor`, `vscode-insider`, `vscode`, and `claude-code`.

## Secrets Management

ToolHive supports different methods for managing secrets. The secrets provider is configured in the `secrets` section of the configuration file.

Currently, there are two supported provider types:

1. `encrypted`: Stores secrets in an encrypted file on disk.
2. `1password`: Uses 1Password for secrets management.

To set the secrets provider:

1. Open the configuration file in a text editor.
2. Locate the `secrets` section.
3. Set `provider_type` to your desired provider.

Example:

```yaml
secrets:
  provider_type: encrypted
```

## MCP Server Permissions

ToolHive uses permission profiles to control what resources MCP servers can access. These profiles are defined using JSON files and can be customized for each server.

### Permission Profile Structure

A permission profile consists of three main sections:

1. `read`: List of mount declarations that the container can read from.
2. `write`: List of mount declarations that the container can write to.
3. `network`: Network permissions for the container.

Here's an example of a permission profile:

```json
{
  "read": [
    "/path/to/read/only/directory",
    "host-path:/container-path",
    "volume://name:container-path"
  ],
  "write": [
    "/path/to/writable/directory"
  ],
  "network": {
    "outbound": {
      "insecure_allow_all": false,
      "allow_transport": ["tcp", "udp"],
      "allow_host": ["example.com"],
      "allow_port": [80, 443]
    }
  }
}
```

### Mount Declarations

Mount declarations can be in one of three formats:

1. Single path: The same path will be mounted from host to container.
2. `host-path:container-path`: Different paths for host and container.
3. `resource-uri:container-path`: Mount a resource identified by URI to a container path.

### Network Permissions

Network permissions are defined in the `network.outbound` section:

- `insecure_allow_all`: If set to `true`, allows all outbound network connections.
- `allow_transport`: List of allowed transport protocols (e.g., "tcp", "udp").
- `allow_host`: List of allowed hosts.
- `allow_port`: List of allowed ports.

### Built-in Profiles

ToolHive provides two built-in permission profiles:

1. `none`: No permissions granted.
2. `network`: Allows all outbound network connections.

To use a built-in profile, specify its name when configuring an MCP server.

## Supported MCP Clients

ToolHive supports several MCP clients out of the box. Here's a list of the supported clients and their configuration file locations:

1. Roo Code (VS Code extension):
   - Linux: `~/.config/Code/User/globalStorage/rooveterinaryinc.roo-cline/settings/mcp_settings.json`
   - macOS: `~/Library/Application Support/Code/User/globalStorage/rooveterinaryinc.roo-cline/settings/mcp_settings.json`

2. Visual Studio Code:
   - Linux: `~/.config/Code/User/settings.json`
   - macOS: `~/Library/Application Support/Code/User/settings.json`

3. Visual Studio Code Insiders:
   - Linux: `~/.config/Code - Insiders/User/settings.json`
   - macOS: `~/Library/Application Support/Code - Insiders/User/settings.json`

4. Cursor editor:
   - All platforms: `~/.cursor/mcp.json`

5. Claude Code CLI:
   - All platforms: `~/.claude.json`

When auto-discovery is enabled, ToolHive will automatically detect and configure these clients. If you're using a custom location for any of these clients, you may need to manually configure them or adjust the auto-discovery logic.

## Conclusion

This guide covered the main aspects of configuring ToolHive, including client auto-discovery, secrets management, and MCP server permissions. By properly configuring these settings, you can ensure that ToolHive works seamlessly with your MCP servers and clients while maintaining the desired level of security and access control.

For more advanced topics or specific use cases, please refer to the ToolHive documentation or reach out to the support team.