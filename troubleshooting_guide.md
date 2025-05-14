# Troubleshooting Guide

This guide provides solutions for common issues you might encounter while using ToolHive. It includes debugging techniques, error message explanations, and steps to resolve typical problems.

## Table of Contents

1. [Container Runtime Issues](#container-runtime-issues)
2. [Networking Problems](#networking-problems)
3. [Permission and Security Concerns](#permission-and-security-concerns)
4. [Logging and Debugging](#logging-and-debugging)
5. [Kubernetes-Specific Issues](#kubernetes-specific-issues)

## Container Runtime Issues

### Container Fails to Start

If a container fails to start, follow these steps:

1. Check the container logs:
   ```
   toolhive logs <container-id>
   ```
2. Verify the image exists:
   ```
   toolhive image exists <image-name>
   ```
3. Ensure the container runtime (Docker/Podman) is running:
   ```
   systemctl status docker
   ```
   or
   ```
   systemctl status podman
   ```

### Container Exits Unexpectedly

If a container exits unexpectedly:

1. Check the container's exit code:
   ```
   toolhive inspect <container-id> | grep State
   ```
2. Review the container logs for error messages.
3. Verify resource constraints (CPU, memory) are not being exceeded.

## Networking Problems

### Unable to Connect to Container

If you can't connect to a container:

1. Verify the container is running:
   ```
   toolhive ls
   ```
2. Check the container's network settings:
   ```
   toolhive inspect <container-id> | grep -A 10 NetworkSettings
   ```
3. Ensure the required ports are exposed and mapped correctly.

### Port Conflicts

If you encounter port conflicts:

1. List all running containers and their port mappings:
   ```
   toolhive ls
   ```
2. Change the host port mapping in your container configuration.
3. Stop any conflicting services on the host machine.

## Permission and Security Concerns

### Access Denied Errors

If you encounter "Access Denied" errors:

1. Check the user and group IDs the container is running as:
   ```
   toolhive exec <container-id> id
   ```
2. Verify file permissions on mounted volumes:
   ```
   ls -l /path/to/mounted/volume
   ```
3. Review the container's security context in Kubernetes deployments.

### Unable to Access Host Resources

If the container can't access host resources:

1. Verify the mount configurations in your container setup.
2. Check SELinux or AppArmor settings on the host.
3. Review the container's capabilities and security options.

## Logging and Debugging

### Enabling Debug Logs

To enable debug logs for ToolHive:

1. Set the debug flag when running ToolHive commands:
   ```
   toolhive --debug <command>
   ```
2. Check the ToolHive log file location (typically `/var/log/toolhive.log`).

### Analyzing Container Logs

To analyze container logs effectively:

1. Use the `toolhive logs` command with options:
   ```
   toolhive logs --tail 100 <container-id>
   ```
2. For real-time log monitoring, use the `--follow` flag:
   ```
   toolhive logs --follow <container-id>
   ```
3. Use `grep` to filter logs for specific errors or messages.

## Kubernetes-Specific Issues

### Pod Stuck in Pending State

If a pod is stuck in a pending state:

1. Check the pod description for events:
   ```
   kubectl describe pod <pod-name>
   ```
2. Verify node resources and pod resource requests/limits.
3. Check for any PersistentVolumeClaim issues.

### StatefulSet Not Scaling

If a StatefulSet is not scaling as expected:

1. Check the StatefulSet description:
   ```
   kubectl describe statefulset <statefulset-name>
   ```
2. Verify PersistentVolume availability if using persistent storage.
3. Check for any pod disruption budgets that might be preventing scaling.

### Service Not Accessible

If a Kubernetes service is not accessible:

1. Verify the service is running:
   ```
   kubectl get svc <service-name>
   ```
2. Check endpoint creation:
   ```
   kubectl get endpoints <service-name>
   ```
3. Verify the service's selector matches pod labels.
4. Check NetworkPolicies that might be blocking traffic.

Remember to always check the ToolHive logs and Kubernetes events for any error messages or warnings that can provide additional context to the issues you're experiencing.

If you continue to experience problems after following these troubleshooting steps, please consult the ToolHive documentation or reach out to the support team for further assistance.