---
title: "Mock Server"
weight: 8
---

### Create Mock Definition

Creates a new mock API response definition interactively.

| Option | Description | Required | Default |
|--------|-------------|----------|---------|
| `--file` | The file path for the new mock API response definition | Yes | - |
| `--force` or `-f` | Overwrite existing file if it exists | No | false |

The command will interactively prompt for all necessary information, including endpoint, HTTP method, status code, headers, and response body.

Example:
```bash
# Create a new mock API response interactively
dotnet run create mock --file users.get
```

### Mock Server Command

Starts a local API mock server using mock definition files.

| Option | Description | Required | Default |
|--------|-------------|----------|---------|
| `--port` or `-p` | Port number to run the mock server on | No | 8080 |
| `--verbose` or `-v` | Enable verbose logging | No | false |
| `--directory` or `-d` | Directory containing mock definitions | No | ".apify" |

Example:
```bash
# Start the mock server on a custom port with verbose logging
dotnet run mock-server --port 3000 --verbose
```

### Basic Mock Server

Create a file with the `.mock.json` extension in your `.apify` directory.

```json
{
  "Name": "Get User by ID",
  "Method": "GET",
  "Endpoint": "/api/users/:id",
  "StatusCode": 200,
  "ContentType": "application/json",
  "Response": {
    "id": 1,
    "name": "John Doe"
  }
}
```

### Advanced Mock Server

Use conditional responses for more complex scenarios.

```json
{
  "Name": "User API",
  "Method": "GET",
  "Endpoint": "/api/users/:id",
  "Responses": [
    {
      "Condition": "q.id == \"1\"",
      "StatusCode": 200,
      "Response": { "id": 1, "name": "John Doe" }
    },
    {
      "Condition": "true",
      "StatusCode": 404,
      "Response": { "error": "User not found" }
    }
  ]
}
```

### Dynamic Responses

The mock server supports dynamic responses using variable substitution.

Available template variables: `{{body}}`, `{{headers}}`, `{{query}}`, `{{path}}`, `{{timestamp}}`, `{{datetime}}`, `{{randomString}}`, `{{randomInt}}`.

### Conditional Responses

Conditions are JavaScript expressions with access to request data: `q` (query), `h` (headers), `b` (body), `p` (path).
