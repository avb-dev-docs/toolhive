# API Reference

ToolHive (thv) is a lightweight, secure, and fast manager for MCP (Model Context Protocol) servers. This API reference provides comprehensive information about the available commands, their options, and usage examples.

## Table of Contents

1. [Root Command](#root-command)
2. [Subcommands](#subcommands)
   - [run](#run)
   - [list](#list)
   - [stop](#stop)
   - [rm](#rm)
   - [proxy](#proxy)
   - [restart](#restart)
   - [serve](#serve)
   - [version](#version)
   - [logs](#logs)
   - [secret](#secret)
3. [Global Flags](#global-flags)
4. [REST API](#rest-api)
   - [Health Check](#health-check)
   - [Version](#version-1)

## Root Command

The root command for ToolHive is `thv`. When used without any subcommands, it displays the help information.

```
thv
```

## Subcommands

### run

(Description and usage of the `run` subcommand)

### list

(Description and usage of the `list` subcommand)

### stop

(Description and usage of the `stop` subcommand)

### rm

(Description and usage of the `rm` subcommand)

### proxy

(Description and usage of the `proxy` subcommand)

### restart

(Description and usage of the `restart` subcommand)

### serve

(Description and usage of the `serve` subcommand)

### version

Displays the current version of ToolHive.

```
thv version
```

### logs

(Description and usage of the `logs` subcommand)

### secret

(Description and usage of the `secret` subcommand)

## Global Flags

The following global flags are available for all ToolHive commands:

- `--debug`: Enable debug mode

Example:
```
thv --debug <subcommand>
```

## REST API

ToolHive also provides a REST API for programmatic access to certain functionalities.

### Health Check

Endpoint: `/health`

Method: GET

Description: Checks the health status of the ToolHive server.

### Version

Endpoint: `/api/v1/version`

Method: GET

Description: Retrieves the current version of ToolHive.

Response:
```json
{
  "version": "<version_string>"
}
```

Example usage with curl:
```
curl http://localhost:<port>/api/v1/version
```

Note: Replace `<port>` with the actual port number where the ToolHive server is running.

For more detailed information on using ToolHive programmatically, please refer to our SDK documentation (if available).