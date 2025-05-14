# Getting Started with ToolHive

ToolHive (thv) is a lightweight utility designed to simplify the deployment and management of MCP (Model Context Protocol) servers, ensuring ease of use, consistency, and security. This guide will walk you through the process of installing ToolHive, configuring it, and running your first MCP server.

## Table of Contents

1. [Installation](#installation)
2. [Basic Configuration](#basic-configuration)
3. [Running Your First MCP Server](#running-your-first-mcp-server)
4. [Common Use Cases](#common-use-cases)
5. [Key Concepts](#key-concepts)

## Installation

ToolHive can be installed using several methods:

### Download the Binary

Download the latest cross-platform release binaries from the [ToolHive Releases](https://github.com/stacklok/toolhive/releases) page.

### Homebrew (macOS)

If you're using macOS, you can install ToolHive using Homebrew:

```bash
brew tap stacklok/tap
brew install thv
```

### Build from Source

To build ToolHive from source, clone the repository and build the CLI using Go:

```bash
go build ./cmd/thv
go install ./cmd/thv
```

Alternatively, if you have [Task](https://taskfile.dev/installation) installed:

```bash
task build
task install
```

## Basic Configuration

Before running your first MCP server, you'll need to perform some basic configuration:

1. Enable client auto-discovery:

```bash
thv config auto-discovery true
```

This allows ToolHive to automatically find and configure supported clients.

2. (Optional) Set up a secrets provider:

```bash
thv config secrets-provider encrypted
```

This enables ToolHive's encrypted secrets store for managing API tokens and other sensitive information.

## Running Your First MCP Server

Now that you have ToolHive installed and configured, let's run your first MCP server:

1. Search for available MCP servers:

```bash
thv registry list
```

2. Run the "fetch" MCP server, which allows LLMs to fetch the contents of a website:

```bash
thv run fetch
```

3. List the running MCP servers:

```bash
thv list
```

Your client should now be able to use the `fetch` MCP tool to fetch website content.

## Common Use Cases

Here are some common use cases for ToolHive:

### Managing MCP Servers

- Start an MCP server:
  ```bash
  thv run <server-name>
  ```

- Stop an MCP server:
  ```bash
  thv stop <server-name>
  ```

- Remove an MCP server:
  ```bash
  thv rm <server-name>
  ```

### Working with Secrets

- Set a secret:
  ```bash
  thv secret set <secret-name>
  ```

- Use a secret when running an MCP server:
  ```bash
  thv run --secret <secret-name>,target=<env-var-name> <server-name>
  ```

### Running Custom MCP Servers

You can run custom MCP servers that are not in the registry:

```bash
thv run --transport sse --name my-mcp-server --port 8080 my-mcp-server-image:latest -- my-mcp-server-args
```

## Key Concepts

1. **MCP (Model Context Protocol)**: A protocol for communication between AI models and external tools or services.

2. **Transport Modes**: ToolHive supports two transport modes:
   - `stdio`: Standard input/output (default)
   - `sse`: Server-Sent Events

3. **Permission Profiles**: ToolHive uses permission profiles to control container permissions. Built-in profiles include:
   - `none`: Minimal permissions with no network access
   - `network`: Permits outbound network connections (not recommended for production)

4. **Secrets Management**: ToolHive provides secure secrets management to avoid exposing sensitive information in configuration files.

5. **Client Compatibility**: ToolHive works with various AI clients, including GitHub Copilot, Cursor, and Roo Code.

By understanding these concepts and following the steps in this guide, you'll be well on your way to leveraging ToolHive for managing your MCP servers efficiently and securely.