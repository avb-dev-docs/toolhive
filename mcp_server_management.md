# MCP Server Management with ToolHive

This guide explains how to manage MCP (Managed Control Plane) servers using ToolHive. You'll learn how to start, stop, and monitor servers, as well as perform common management tasks and troubleshoot issues.

## Starting an MCP Server

To start an MCP server, use the `thv run` command. This command supports three ways to run an MCP server:

1. From the registry:
   ```
   thv run server-name [-- args...]
   ```
   This looks up the server in the registry and uses its predefined settings.

2. From a container image:
   ```
   thv run ghcr.io/example/mcp-server:latest [-- args...]
   ```
   This runs the specified container image directly with the provided arguments.

3. Using a protocol scheme:
   ```
   thv run uvx://package-name [-- args...]
   thv run npx://package-name [-- args...]
   thv run go://package-name [-- args...]
   ```
   This automatically generates a container that runs the specified package using either uvx (Python with uv package manager), npx (Node.js), or go (Golang).

### Common Options

- `--transport`: Specify the transport mode (sse or stdio, default: stdio)
- `--name`: Set a custom name for the MCP server
- `--port`: Specify the host port for the HTTP proxy to listen on
- `--target-port`: Set the container port to expose (only for SSE transport)
- `--permission-profile`: Choose a permission profile (none, network, or path to JSON file)
- `--env` or `-e`: Set environment variables (format: KEY=VALUE)
- `--volume` or `-v`: Mount volumes (format: host-path:container-path[:ro])
- `--secret`: Specify secrets to be fetched from the secrets manager

Example:
```
thv run my-mcp-server --transport sse --port 8080 --env API_KEY=secret123 -- --verbose
```

## Stopping an MCP Server

To stop a running MCP server, use the `thv stop` command followed by the container name:

```
thv stop container-name
```

You can specify a timeout for graceful shutdown using the `--timeout` flag:

```
thv stop container-name --timeout 60
```

## Listing MCP Servers

To view all running MCP servers managed by ToolHive, use the `thv list` command:

```
thv list
```

Options:
- `--all` or `-a`: Show all containers (including stopped ones)
- `--format`: Choose output format (json, text, or mcpservers)

Example of JSON output:
```
thv list --format json
```

## Troubleshooting Tips

1. If a container fails to start, check the logs for error messages:
   ```
   docker logs container-name
   ```

2. Ensure that required environment variables and secrets are properly set when starting an MCP server.

3. If you encounter permission issues, review and adjust the permission profile used for the MCP server.

4. For network-related problems, verify that the specified ports are not already in use and that your firewall settings allow the required connections.

5. When using custom images or protocol schemes, make sure the necessary dependencies and configurations are correctly set up in the container.

## Best Practices

1. Use descriptive names for your MCP servers to easily identify them later.

2. Regularly update your container images to ensure you have the latest security patches and features.

3. Use volume mounts for persistent data to avoid data loss when containers are stopped or removed.

4. Implement proper secret management by using the `--secret` flag instead of passing sensitive information directly as environment variables.

5. Monitor resource usage of your MCP servers and adjust container limits as needed.

By following these guidelines and using the provided ToolHive commands, you can effectively manage your MCP servers throughout their lifecycle.