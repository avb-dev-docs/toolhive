---
title: MCP Server Registry
description: A guide to using the MCP server registry in ToolHive
---

# MCP Server Registry

## Introduction

The MCP (Model Context Protocol) Server Registry is a key component of ToolHive (thv) that allows users to manage and interact with various MCP servers. This guide will walk you through how to use the registry to list available servers, search for specific servers, and view detailed information about each server.

## Listing Available Servers

To list all available MCP servers in the registry, you can use the `ListServers` function. Here's an example of how to use it:

```go
servers, err := registry.ListServers()
if err != nil {
    // Handle error
    return
}

for _, server := range servers {
    fmt.Printf("Server Name: %s\n", server.Name)
    fmt.Printf("Description: %s\n", server.Description)
    fmt.Printf("Tags: %v\n\n", server.Tags)
}
```

This will provide you with a list of all servers in the registry, including their names, descriptions, and associated tags.

## Searching for Specific Servers

If you're looking for a specific server or a group of servers with certain characteristics, you can use the `SearchServers` function. This function searches through server names, descriptions, and tags. Here's how to use it:

```go
query := "language model"
results, err := registry.SearchServers(query)
if err != nil {
    // Handle error
    return
}

for _, server := range results {
    fmt.Printf("Server Name: %s\n", server.Name)
    fmt.Printf("Description: %s\n", server.Description)
    fmt.Printf("Tags: %v\n\n", server.Tags)
}
```

This will return all servers that match the query "language model" in their name, description, or tags.

## Viewing Detailed Server Information

To get detailed information about a specific server, you can use the `GetServer` function. Here's an example:

```go
serverName := "gpt-3.5-turbo"
server, err := registry.GetServer(serverName)
if err != nil {
    // Handle error
    return
}

fmt.Printf("Server Name: %s\n", server.Name)
fmt.Printf("Description: %s\n", server.Description)
fmt.Printf("Tags: %v\n", server.Tags)
fmt.Printf("Version: %s\n", server.Version)
fmt.Printf("Image: %s\n", server.Image)
```

This will provide you with all the available information about the specified server.

## Running MCP Servers

Once you've identified the server you want to use, you can run it using the ToolHive CLI. Here are some examples:

1. Running a basic language model server:

```bash
thv run gpt-3.5-turbo
```

2. Running a specialized server with specific options:

```bash
thv run code-llama --gpu
```

3. Running a server with custom environment variables:

```bash
thv run stable-diffusion -e CUDA_VISIBLE_DEVICES=0 -e MODEL_PATH=/path/to/model
```

Remember to check the specific requirements and options for each server in the registry, as they may vary.

## Conclusion

The MCP Server Registry in ToolHive provides a powerful and flexible way to manage and interact with various MCP servers. By using the functions described in this guide, you can easily list, search, and retrieve information about available servers, and then run them using the ToolHive CLI. This enables you to efficiently work with a wide range of AI models and tools in a containerized environment.