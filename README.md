# SQL Server MCP Server

A Model Context Protocol (MCP) server for SQL Server, built with C# and fully compatible with **Native AOT**.

Provides two tools: `query` for read-only SELECT statements and `execute` for data modification commands.

## Tools

| Tool | Description |
|------|-------------|
| `query` | Execute a read-only SQL SELECT query and return results as JSON |
| `execute` | Execute a SQL command (INSERT, UPDATE, DELETE, CREATE, ALTER, DROP) and return rows affected |

## Configuration

Set the `SQLSERVER_CONNECTION_STRING` environment variable before running the server:

```
SQLSERVER_CONNECTION_STRING="Server=myserver;Database=mydb;User Id=myuser;Password=mypassword;TrustServerCertificate=True;"
```

## Developing locally

```json
{
  "servers": {
    "sqlserver": {
      "type": "stdio",
      "command": "dotnet",
      "args": [
        "run",
        "--project",
        "<PATH TO SQLServerMcp DIRECTORY>"
      ],
      "env": {
        "SQLSERVER_CONNECTION_STRING": "Server=...;Database=...;User Id=...;Password=...;TrustServerCertificate=True;"
      }
    }
  }
}
```

## Using from NuGet.org

```json
{
  "servers": {
    "sqlserver": {
      "type": "stdio",
      "command": "dnx",
      "args": [
        "SqlServerMcp",
        "--version",
        "1.0.0",
        "--yes"
      ],
      "env": {
        "SQLSERVER_CONNECTION_STRING": "Server=...;Database=...;User Id=...;Password=...;TrustServerCertificate=True;"
      }
    }
  }
}
```

## Building

The server is compiled as a self-contained Native AOT single-file executable for Windows and Linux:

- `win-x64`
- `linux-x64`
- `linux-musl-x64` (statically linked, no glibc dependency)

## More information

- [MCP Official Documentation](https://modelcontextprotocol.io/)
- [MCP C# SDK](https://csharp.sdk.modelcontextprotocol.io/)
