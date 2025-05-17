---
title: Advanced Usage Guide
description: A comprehensive guide on advanced usage of ToolHive, including customizing permissions, running in Kubernetes, implementing custom MCP servers, and using different transport protocols.
---

# Advanced Usage Guide

This guide covers advanced topics for using ToolHive, including customizing permissions, running ToolHive in Kubernetes, implementing custom MCP servers, and using different transport protocols.

## Table of Contents

1. [Customizing Permissions](#customizing-permissions)
2. [Running ToolHive in Kubernetes](#running-toolhive-in-kubernetes)
3. [Implementing Custom MCP Servers](#implementing-custom-mcp-servers)
4. [Using Different Transport Protocols](#using-different-transport-protocols)

## Customizing Permissions

ToolHive allows you to customize the permissions for containers launched by the application. This is done using a JSON-based permission profile.

### Permission Profile Structure

A permission profile is a JSON file that defines the following:

- `read`: List of mount declarations that the container can read from
- `write`: List of mount declarations that the container can write to
- `network`: Network permissions for the container

Here's an example of a permission profile:

```json
{
  "read": ["/var/run/mcp.sock"],
  "write": ["/var/run/mcp.sock"],
  "network": {
    "outbound": {
      "insecure_allow_all": false,
      "allow_transport": ["tcp", "udp"],
      "allow_host": ["localhost", "google.com"],
      "allow_port": [80, 443]
    }
  }
}
```

### Using a Custom Permission Profile

To use a custom permission profile, use the `--permission-profile` flag when running a container:

```bash
thv run --permission-profile /path/to/your/profile.json your-mcp-server
```

### Built-in Profiles

ToolHive includes two built-in permission profiles for convenience:

1. `none`: Grants minimal permissions with no network access.
2. `network`: Permits outbound network connections to any host on any port (not recommended for production use).

To use a built-in profile:

```bash
thv run --permission-profile none your-mcp-server
```

## Running ToolHive in Kubernetes

ToolHive can be deployed in a Kubernetes cluster using the ToolHive Operator. This functionality is still under active development for production use cases, but you can try it out locally using a Kind cluster.

### Setting Up a Kind Cluster

1. Install Kind (Kubernetes in Docker) if you haven't already:

   ```bash
   go install sigs.k8s.io/kind@v0.14.0
   ```

2. Create a Kind cluster:

   ```bash
   kind create cluster --name toolhive-cluster
   ```

3. Set your kubectl context to the new cluster:

   ```bash
   kubectl cluster-info --context kind-toolhive-cluster
   ```

### Deploying ToolHive Operator

1. Apply the ToolHive Operator manifests:

   ```bash
   kubectl apply -f https://raw.githubusercontent.com/stacklok/toolhive/main/deploy/operator.yaml
   ```

2. Verify the operator is running:

   ```bash
   kubectl get pods -n toolhive-system
   ```

### Deploying MCP Servers

To deploy an MCP server using the ToolHive Operator, create a custom resource definition (CRD) for your MCP server:

```yaml
apiVersion: toolhive.stacklok.dev/v1alpha1
kind: MCPServer
metadata:
  name: example-mcp-server
spec:
  image: your-mcp-server-image:tag
  command: ["your-mcp-server-command"]
  env:
    - name: ENV_VAR_NAME
      value: "env_var_value"
  ports:
    - containerPort: 8080
```

Apply the CRD to your cluster:

```bash
kubectl apply -f your-mcp-server-crd.yaml
```

For more detailed information on running ToolHive in Kubernetes, refer to the [Kubernetes deployment guide](./kubernetes-deployment-guide.md).

## Implementing Custom MCP Servers

ToolHive supports running custom MCP servers that are not in the built-in registry. To implement a custom MCP server:

1. Create a Docker image for your MCP server.
2. Ensure your MCP server implements the required MCP protocol.
3. Run your custom MCP server using the `thv run` command with the `--name` flag:

   ```bash
   thv run --transport sse --name my-custom-mcp --port 8080 my-custom-mcp-image:latest -- custom-args
   ```

### Best Practices for Custom MCP Servers

1. Use a minimal base image to reduce attack surface.
2. Implement proper error handling and logging.
3. Follow the MCP protocol specification closely.
4. Provide clear documentation for your custom MCP server.

## Using Different Transport Protocols

ToolHive supports two transport protocols for communication between the client and MCP server:

1. Standard I/O (stdio)
2. Server-Sent Events (SSE)

### Standard I/O (stdio)

This is the default transport method. ToolHive redirects SSE traffic from the client to the container's standard input and output.

To explicitly use stdio transport:

```bash
thv run --transport stdio your-mcp-server
```

### Server-Sent Events (SSE)

SSE transport creates a reverse proxy on a random port that forwards requests to the container.

To use SSE transport:

```bash
thv run --transport sse --port 8080 your-mcp-server
```

When using SSE transport, make sure your MCP server is configured to listen on the specified port and handle SSE connections.

## Conclusion

This advanced usage guide covers key topics for getting the most out of ToolHive. By customizing permissions, leveraging Kubernetes deployment, implementing custom MCP servers, and understanding transport protocols, you can tailor ToolHive to your specific needs and environment.

For further assistance or to report issues, please visit the [ToolHive GitHub repository](https://github.com/stacklok/toolhive) or join our [community Discord server](https://discord.gg/stacklok).