---
title: Kubernetes Integration
description: A comprehensive guide on using ToolHive with Kubernetes
---

# Kubernetes Integration

This guide provides comprehensive information on using ToolHive with Kubernetes. It covers deploying MCP servers in a Kubernetes cluster, configuring the Kubernetes runtime, and best practices for Kubernetes integration.

## Table of Contents

1. [Overview](#overview)
2. [Deploying MCP Servers in Kubernetes](#deploying-mcp-servers-in-kubernetes)
3. [Configuring the Kubernetes Runtime](#configuring-the-kubernetes-runtime)
4. [Best Practices](#best-practices)
5. [Advanced Configuration](#advanced-configuration)

## Overview

ToolHive supports running MCP (Managed Command Protocol) servers in Kubernetes clusters. This integration allows you to leverage the scalability, reliability, and orchestration capabilities of Kubernetes while managing your tools and services through ToolHive.

## Deploying MCP Servers in Kubernetes

To deploy an MCP server in Kubernetes using ToolHive, use the `thv run` command with the appropriate flags. ToolHive will automatically create the necessary Kubernetes resources, including StatefulSets and Services.

Example:

```bash
thv run --transport sse --port 8080 --target-port 3000 my-mcp-server
```

This command will:

1. Create a StatefulSet for the MCP server
2. Set up a headless Service for SSE transport (if applicable)
3. Configure the necessary ports and labels

## Configuring the Kubernetes Runtime

ToolHive uses a Kubernetes client to interact with your cluster. The client is automatically configured using in-cluster configuration when running inside a Kubernetes pod.

Key aspects of the Kubernetes runtime:

- **Namespace**: ToolHive uses the current namespace of the pod it's running in. You can override this by setting the `POD_NAMESPACE` environment variable.
- **Container Runtime**: Kubernetes is used as the container runtime, replacing Docker or Podman in traditional deployments.
- **Resource Management**: StatefulSets are used to manage MCP server instances, ensuring stable network identities and persistent storage if needed.

## Best Practices

1. **Use StatefulSets**: ToolHive uses StatefulSets for MCP servers in Kubernetes. This ensures stable network identities and ordered deployment and scaling.

2. **Configure Resource Limits**: Always set appropriate CPU and memory limits for your MCP servers to prevent resource contention.

3. **Use Secrets**: Utilize Kubernetes Secrets for sensitive information. ToolHive supports injecting secrets as environment variables.

4. **Leverage Labels**: ToolHive automatically adds labels to resources. Use these for organization and potential network policies.

5. **Consider Network Policies**: Implement Kubernetes Network Policies to control traffic to and from your MCP servers.

## Advanced Configuration

### Custom Pod Template

You can provide a custom pod template patch to modify the Kubernetes pod configuration. Use the `--k8s-pod-patch` flag with a JSON string:

```bash
thv run --k8s-pod-patch '{"spec":{"containers":[{"name":"mcp","resources":{"limits":{"cpu":"500m","memory":"512Mi"}}}]}}' my-mcp-server
```

### Security Context

ToolHive sets a default security context for pods and containers. You can override these settings using the pod template patch. The defaults are:

```json
{
  "securityContext": {
    "runAsNonRoot": true,
    "runAsUser": 1000,
    "runAsGroup": 1000,
    "fsGroup": 1000
  }
}
```

### Headless Services

For SSE transport, ToolHive creates a headless service. This allows direct communication with individual pods, which is necessary for the SSE transport mode.

### Port Configuration

When using SSE transport, you can specify both the host port (`--port`) and the container port (`--target-port`). ToolHive will configure the Kubernetes Service and pod ports accordingly.

Remember to consider your cluster's network policies and ingress configuration when exposing ports.

By following these guidelines and leveraging ToolHive's Kubernetes integration, you can efficiently manage and deploy your MCP servers in a Kubernetes environment, taking full advantage of both ToolHive's capabilities and Kubernetes' robust orchestration features.