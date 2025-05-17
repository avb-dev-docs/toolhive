---
title: Getting Started with ToolHive
description: A comprehensive guide to installing and using ToolHive, including basic usage examples and an overview of main features.
---

# Getting Started with ToolHive

## Introduction

ToolHive (thv) is a lightweight utility designed to simplify the deployment and management of MCP (Model Context Protocol) servers. This guide will walk you through the installation process, basic usage, and main features of ToolHive.

## Prerequisites

Before getting started with ToolHive, ensure you have the following:

- A macOS or Linux operating system
- Docker or Podman installed
- For client auto-discovery/configuration, one of the following supported clients:
  - VS Code (v1.99.0 or newer) with an active GitHub Copilot subscription
  - Cursor
  - Roo Code (VS Code extension)

## Installation

You can install ToolHive using one of the following methods:

### Download the Binary

1. Visit the [ToolHive releases page](https://github.com/stacklok/toolhive/releases) on GitHub.
2. Download the latest cross-platform release binary for your operating system.

### Homebrew (macOS)

If you're using macOS and have Homebrew installed, you can install ToolHive with the following commands:

```bash
brew tap stacklok/tap
brew install thv
```

### Build from Source

To build ToolHive from source:

1. Clone the ToolHive repository:

```bash
git clone https://github.com/stacklok/toolhive.git
cd toolhive
```

2. Build and install the CLI using Go:

```bash
go build ./cmd/thv
go install ./cmd/thv
```

Alternatively, if you have [Task](https://taskfile.dev/installation) installed:

```bash
task build
task install
```

## Basic Usage

### Enabling Client Auto-discovery

To automatically configure supported clients, enable auto-discovery:

```bash
thv config auto-discovery true
```

### Running Your First MCP Server

To run your first MCP server, use the `thv run` command. For example, to run the Fetch MCP server:

```bash
thv run fetch
```

This command will start the Fetch MCP server, which allows LLMs to fetch the contents of a website.

### Listing Running MCP Servers

To see which MCP servers are currently running:

```bash
thv list
```

## Main Features

### Finding and Running MCP Servers

ToolHive provides a curated registry of MCP servers. To explore available servers:

```bash
# List all available servers
thv registry list

# Search for a specific server
thv search <search-term>

# Get detailed information about a server
thv registry info <name-of-mcp-server>
```

### Managing MCP Servers

To stop or remove a running MCP server:

```bash
# Stop a server
thv stop <name-of-mcp-server>

# Remove a server
thv rm <name-of-mcp-server>

# Stop and remove in one command
thv rm -f <name-of-mcp-server>
```

### Secrets Management

ToolHive provides secure secrets management for API tokens and other sensitive information. To use the encrypted secrets store:

```bash
# Enable the encrypted secrets provider
thv config secrets-provider encrypted

# Set a secret
thv secret set <secret-name>

# Use the secret when running an MCP server
thv run --secret <secret-name>,target=<environment-variable> <mcp-server-name>
```

### Custom MCP Servers

You can run custom MCP servers not included in the registry:

```bash
thv run --transport sse --name my-mcp-server --port 8080 my-mcp-server-image:latest -- my-mcp-server-args
```

### Running MCP Servers Using Protocol Schemes

ToolHive supports running MCP servers directly from package managers using protocol schemes:

```bash
# Run a Python-based MCP server
thv run uvx://awslabs.core-mcp-server@latest

# Run a Node.js-based MCP server
thv run npx://pulumi/mcp-server@latest

# Run a Go-based MCP server
thv run go://github.com/example/go-mcp-server@latest
```

## Next Steps

Now that you're familiar with the basics of ToolHive, you can explore more advanced features such as:

- Customizing permissions for MCP servers
- Running ToolHive in Kubernetes
- Contributing to the ToolHive project

For more detailed information on these topics and other advanced usage, refer to the [ToolHive documentation](https://github.com/stacklok/toolhive/tree/main/docs).