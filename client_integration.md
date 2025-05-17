---
title: Client Integration Guide
description: A comprehensive guide on integrating different clients with ToolHive, including configuration steps for supported and unsupported clients.
---

# Client Integration Guide

This guide explains how to integrate and configure various clients with ToolHive, ensuring seamless interaction with MCP (Model Context Protocol) servers.

## Supported Clients

ToolHive has been tested and supports automatic configuration for the following clients:

1. VS Code with GitHub Copilot (v1.99.0 or newer)
2. Cursor
3. Roo Code (VS Code extension)
4. PydanticAI
5. Claude Code

## Automatic Client Configuration

ToolHive can automatically discover and configure supported clients. To enable this feature:

```bash
thv config auto-discovery true
```

This command enables automatic discovery and configuration of supported clients.

## Manual Client Registration

If you prefer to manually register clients or need more control over the configuration process, you can use the following commands:

```bash
# Register a client
thv config register-client <client-name>

# Show registered clients
thv config list-registered-clients

# Remove a client
thv config remove-client <client-name>
```

Replace `<client-name>` with the appropriate client identifier (e.g., "vscode", "cursor", "roo-code").

## Client-Specific Configuration

### VS Code with GitHub Copilot

1. Ensure you have VS Code (v1.99.0 or newer) installed with an active GitHub Copilot subscription.
2. Enable auto-discovery or manually register the client:
   ```bash
   thv config register-client vscode
   ```
3. Run an MCP server using ToolHive:
   ```bash
   thv run <mcp-server-name>
   ```
4. ToolHive will automatically update the VS Code settings to use the MCP server.

### Cursor

1. Install the Cursor editor.
2. Enable auto-discovery or manually register the client:
   ```bash
   thv config register-client cursor
   ```
3. Run an MCP server using ToolHive.
4. ToolHive will automatically update the Cursor configuration to use the MCP server.

### Roo Code (VS Code extension)

1. Install the Roo Code extension in VS Code.
2. Enable auto-discovery or manually register the client:
   ```bash
   thv config register-client roo-code
   ```
3. Run an MCP server using ToolHive.
4. ToolHive will automatically update the Roo Code settings to use the MCP server.

### PydanticAI and Claude Code

These clients are supported but may require manual configuration. Refer to their respective documentation for detailed setup instructions.

## Manual Configuration for Unsupported Clients

For clients that are not automatically configured by ToolHive, you can manually set up the MCP server URL. The general steps are:

1. Run an MCP server using ToolHive:
   ```bash
   thv run <mcp-server-name>
   ```

2. Get the MCP server URL:
   ```bash
   thv list
   ```
   This will display the running MCP servers and their URLs.

3. In your client's configuration, set the MCP server URL to the one provided by ToolHive. The exact steps will vary depending on the client.

## Verifying Client Configuration

To verify that your client is correctly configured:

1. Run an MCP server using ToolHive.
2. Open your client (e.g., VS Code, Cursor).
3. Attempt to use a feature that requires the MCP server (e.g., code completion, function calling).
4. Check the ToolHive logs for any connection or usage information:
   ```bash
   thv logs <mcp-server-name>
   ```

## Troubleshooting

If you encounter issues with client integration:

1. Ensure ToolHive is running the latest version:
   ```bash
   thv version
   ```

2. Check that the MCP server is running:
   ```bash
   thv list
   ```

3. Verify client registration:
   ```bash
   thv config list-registered-clients
   ```

4. Review the ToolHive logs for any error messages:
   ```bash
   thv logs <mcp-server-name>
   ```

5. For manually configured clients, double-check that the MCP server URL is correctly set in the client's configuration.

If problems persist, consider reaching out to the ToolHive community on Discord or opening an issue on the GitHub repository.

## Conclusion

By following this guide, you should be able to successfully integrate and configure various clients with ToolHive, enabling seamless interaction with MCP servers. Whether using supported clients with automatic configuration or manually setting up unsupported clients, ToolHive provides a flexible and secure way to manage your MCP server connections.