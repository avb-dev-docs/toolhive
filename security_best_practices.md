---
title: Security Best Practices for ToolHive
description: A comprehensive guide to securing your ToolHive implementation, including secrets management, authorization policies, and secure communication.
---

# Security Best Practices for ToolHive

## Introduction

This document outlines the security best practices for using ToolHive. It covers three main areas: secure secrets management, configuring authorization policies, and ensuring secure communication between clients and MCP servers. Following these guidelines will help you maintain a robust and secure ToolHive implementation.

## Secure Secrets Management

ToolHive provides a robust secrets management system to handle sensitive information securely. Here are the best practices for managing secrets:

### 1. Use the Built-in Encrypted Manager

ToolHive offers an encrypted secrets manager that securely stores your sensitive data. To use it:

```go
secretsManager, err := secrets.CreateSecretProvider(secrets.EncryptedType)
if err != nil {
    // Handle error
}
```

### 2. Protect the Encryption Password

The encryption password is crucial for securing your secrets. Follow these guidelines:

- Use the `TOOLHIVE_SECRETS_PASSWORD` environment variable to set the password.
- If not set, ToolHive will prompt for a password and store it securely in the OS keyring.
- Ensure the password is strong and unique.

### 3. Avoid Hardcoding Secrets

Never hardcode secrets in your code or configuration files. Instead, use the secrets manager to retrieve them:

```go
secret, err := secretsManager.GetSecret("my_api_key")
if err != nil {
    // Handle error
}
```

### 4. Rotate Secrets Regularly

Implement a policy to rotate secrets periodically. ToolHive's secrets manager makes it easy to update secrets:

```go
err := secretsManager.SetSecret("my_api_key", "new_secret_value")
if err != nil {
    // Handle error
}
```

## Configuring Authorization Policies

ToolHive uses Cedar policies for fine-grained authorization. Here's how to configure and use them securely:

### 1. Define Granular Policies

Create specific policies for different actions and resources. For example:

```cedar
permit (
    principal in Client::*,
    action in [Action::call_tool],
    resource in Tool::weather
) when {
    principal.tier == "premium"
};
```

### 2. Use the CedarAuthorizer

Implement the `CedarAuthorizer` in your application to enforce policies:

```go
authorizer, err := authz.NewCedarAuthorizer(authz.CedarAuthorizerConfig{
    Policies: []string{policyString},
    EntitiesJSON: entitiesJSON,
})
if err != nil {
    // Handle error
}
```

### 3. Authorize Actions

Use the `IsAuthorized` method to check permissions before performing actions:

```go
isAuthorized, err := authorizer.IsAuthorized(
    "Client::user123",
    "Action::call_tool",
    "Tool::weather",
    contextMap,
)
if err != nil || !isAuthorized {
    // Handle unauthorized access
}
```

### 4. Regularly Review and Update Policies

Periodically review your authorization policies to ensure they align with your security requirements. Update them as needed:

```go
err := authorizer.UpdatePolicies(newPolicies)
if err != nil {
    // Handle error
}
```

## Secure Communication

Ensuring secure communication between clients and MCP servers is crucial. Follow these practices:

### 1. Use SSL/TLS for All Communications

Always use HTTPS for API endpoints and WebSocket connections. Configure your server with a valid SSL certificate.

### 2. Implement Strong Authentication

Use JWT (JSON Web Tokens) for authentication. ToolHive provides middleware to extract and verify JWT claims:

```go
claims, ok := authz.ExtractClaimsFromContext(ctx)
if !ok {
    // Handle missing claims
}
```

### 3. Validate and Sanitize Input

Always validate and sanitize user input to prevent injection attacks and other security vulnerabilities.

### 4. Use Secure Transports

ToolHive offers different transport modes. Use the most secure option available:

```go
transportConfig := types.Config{
    Type: types.TransportTypeSSE,
    Host: "secure.example.com",
    Port: "443",
    // Other secure configurations
}
transport, err := transportFactory.Create(transportConfig)
if err != nil {
    // Handle error
}
```

## Conclusion

By following these security best practices, you can significantly enhance the security of your ToolHive implementation. Remember to keep your ToolHive version up-to-date and stay informed about the latest security recommendations and updates from the ToolHive team.