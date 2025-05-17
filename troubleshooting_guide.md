---
title: Troubleshooting Guide
description: A comprehensive guide for troubleshooting common issues in ToolHive
---

# Troubleshooting Guide

## Introduction

This guide provides solutions for common issues you may encounter while using ToolHive. It covers error messages, container problems, client configuration issues, and general debugging techniques.

## Checking Logs

Logs are crucial for diagnosing issues in ToolHive. Here's how to access and interpret logs:

### Accessing Container Logs

To view logs for a specific container:

```bash
toolhive container logs <container-id>
```

This command retrieves the logs for the specified container, which can help identify runtime errors or issues within the container.

### Interpreting Log Levels

ToolHive uses different log levels to categorize the severity of messages:

- DEBUG: Detailed information, typically useful for debugging.
- INFO: General information about the system's operation.
- WARN: Warning messages that don't prevent the system from functioning but require attention.
- ERROR: Error messages that indicate a failure in a specific operation.

To adjust the log level for more detailed output, you can use the `--debug` flag when running ToolHive commands.

## Common Issues and Solutions

### 1. Container Creation Fails

If you're having trouble creating a container, try the following:

1. Check if the image exists:

   ```bash
   toolhive image exists <image-name>
   ```

   If the image doesn't exist, pull it manually:

   ```bash
   toolhive image pull <image-name>
   ```

2. Verify the permission profile:
   Ensure that the permission profile you're using is correctly configured and exists.

3. Check for port conflicts:
   Make sure the ports you're trying to expose are not already in use by other containers or processes.

### 2. Container is Created but Not Running

If a container is created but not running:

1. Check the container status:

   ```bash
   toolhive container list
   ```

2. If the status is not "Running", try to start the container:

   ```bash
   toolhive container start <container-id>
   ```

3. If it fails to start, check the logs for error messages:

   ```bash
   toolhive container logs <container-id>
   ```

### 3. Client Configuration Issues

If you're experiencing problems with client configurations:

1. Verify the client configuration files:
   ToolHive looks for configuration files in specific locations. Check if the files exist and are accessible:

   - VS Code: `~/.config/Code/User/settings.json` (Linux) or `~/Library/Application Support/Code/User/settings.json` (macOS)
   - VS Code Insiders: `~/.config/Code - Insiders/User/settings.json` (Linux) or `~/Library/Application Support/Code - Insiders/User/settings.json` (macOS)
   - Cursor: `~/.cursor/mcp.json`
   - Claude Code CLI: `~/.claude.json`

2. Check file permissions:
   Ensure that the configuration files have the correct read and write permissions.

3. Validate JSON format:
   Make sure the configuration files are valid JSON. You can use online JSON validators or the `jq` command-line tool:

   ```bash
   jq . ~/.cursor/mcp.json
   ```

4. Manually update configurations:
   If automatic configuration fails, you can manually add or update MCP server entries in the configuration files.

### 4. Network Connectivity Issues

If containers can't communicate or if you're experiencing network-related problems:

1. Check if the container is using the correct network mode:
   For Kubernetes, ensure the pod is in the correct namespace and has the right network policies.

2. Verify port mappings:
   Use the `toolhive container info <container-id>` command to check if ports are correctly mapped.

3. Test connectivity:
   Use tools like `curl` or `wget` to test if you can reach the container's exposed ports.

## Debugging Kubernetes Deployments

When using ToolHive with Kubernetes, additional troubleshooting steps may be necessary:

1. Check pod status:

   ```bash
   kubectl get pods -n <namespace>
   ```

2. Describe the pod for more details:

   ```bash
   kubectl describe pod <pod-name> -n <namespace>
   ```

3. Check events in the namespace:

   ```bash
   kubectl get events -n <namespace>
   ```

4. Verify service configuration:

   ```bash
   kubectl get svc -n <namespace>
   kubectl describe svc <service-name> -n <namespace>
   ```

## Reporting Issues

If you encounter a bug or issue that you can't resolve, please report it to the ToolHive team:

1. Gather relevant information:
   - ToolHive version
   - Operating system
   - Error messages and logs
   - Steps to reproduce the issue

2. Create a new issue on the ToolHive GitHub repository, providing as much detail as possible.

## Conclusion

This troubleshooting guide covers common issues you may encounter while using ToolHive. If you're still experiencing problems after following these steps, don't hesitate to reach out to the ToolHive community or support channels for further assistance.