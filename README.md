# SQL Server MCP Server

Connect your AI application to SQL Server using the Model Context Protocol (MCP). This server provides two tools to interact with your database: execute queries and modify data.

## Installation

### Option 1: Direct download (recommended)

1. Download the binary for your platform from [Releases](https://github.com/user/SqlServerMcp/releases)
2. Extract the zip file
3. Ensure the executable has execute permissions (Linux): `chmod +x SQLServerMcp`

### Option 2: NuGet

```json
{
  "servers": {
    "sqlserver": {
      "type": "stdio",
      "command": "dnx",
      "args": ["SqlServerMcp", "--yes"],
      "env": {
        "SQLSERVER_CONNECTION_STRING": "Server=...;Database=...;User Id=...;Password=...;TrustServerCertificate=True;"
      }
    }
  }
}
```

## Configuration

### Database connection

Set the `SQLSERVER_CONNECTION_STRING` environment variable with your connection string:

```
SQLSERVER_CONNECTION_STRING="Server=myserver;Database=mydb;User Id=myuser;Password=mypassword;TrustServerCertificate=True;"
```

### MCP client configuration

#### With downloaded binary

```json
{
  "servers": {
    "sqlserver": {
      "type": "stdio",
      "command": "./SQLServerMcp",
      "args": [],
      "env": {
        "SQLSERVER_CONNECTION_STRING": "Server=...;Database=...;User Id=...;Password=...;TrustServerCertificate=True;"
      }
    }
  }
}
```

#### On Windows

```json
{
  "servers": {
    "sqlserver": {
      "type": "stdio",
      "command": ".\\SQLServerMcp.exe",
      "args": [],
      "env": {
        "SQLSERVER_CONNECTION_STRING": "Server=...;Database=...;User Id=...;Password=...;TrustServerCertificate=True;"
      }
    }
  }
}
```

## Available tools

### `query` — Read queries

Execute SELECT statements and return results as JSON.

**Parameter:**
- `query` (string): The SQL SELECT statement to execute

**Example:**
```sql
SELECT TOP 10 Name, CreatedDate FROM sys.tables
```

**Response:**
```json
[
  {"Name": "Users", "CreatedDate": "2024-01-15T10:30:00.000"},
  {"Name": "Orders", "CreatedDate": "2024-01-15T10:30:00.000"}
]
```

### `execute` — Data modification

Execute commands that modify data or structure (INSERT, UPDATE, DELETE, CREATE, ALTER, DROP).

**Parameter:**
- `query` (string): The SQL statement to execute

**Example:**
```sql
INSERT INTO Users (Name, Email) VALUES ('Juan', 'juan@ejemplo.com')
```

**Response:**
```
Rows affected: 1
```

## Supported data types

The server automatically converts SQL Server types to JSON:

| SQL Server | JSON |
|---|---|
| `INT`, `BIGINT`, `SMALLINT` | Number |
| `FLOAT`, `DECIMAL`, `MONEY` | Number |
| `VARCHAR`, `NVARCHAR`, `TEXT` | String |
| `BIT` | Boolean |
| `DATETIME`, `DATE` | ISO 8601 string |
| `VARBINARY` | Base64 |
| `NULL` | `null` |

## Troubleshooting

### Connection error
- Verify that `SQLSERVER_CONNECTION_STRING` is configured correctly
- Ensure the SQL Server is accessible from where you run the MCP server
- For remote connections, add `TrustServerCertificate=True;` if using self-signed certificates

### Long query timeout
- Queries have a default timeout of 30 seconds
- For longer queries, optimize your query or add indexes

### Authentication error
- Verify username and password in the connection string
- For Windows authentication, use `Integrated Security=True;` instead of User Id/Password

## Supported platforms

- Windows x64
- Linux x64
- Linux x64 (musl, statically compiled)

## More information

- [Official MCP Documentation](https://modelcontextprotocol.io/)
