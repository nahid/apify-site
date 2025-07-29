---
title: "Mock Server"
weight: 8
description: "Learn how to use Apify's built-in mock server for API testing and development."
---

Apify includes an integrated mock server to simulate API endpoints for development and testing. This allows you to work on your application without needing a live backend, or to simulate specific API behaviors for testing edge cases.

### Features

- **Dynamic & Conditional Responses**: Define mock responses based on request parameters, headers, or body content.
- **Template Variables**: Use built-in (random data, timestamps) and custom (request-derived) variables in mock responses.
- **File-based Configuration**: Manage mock definitions in simple `.mock.json` files.

### Create Mock Definition

Creates a new mock API definition file interactively.

```bash
apify create:mock <file> [--force]
```

- `<file>`: (Required) The file path for the new mock API definition (e.g., `users.get` becomes `.apify/users/get.mock.json`). The `.mock.json` extension is added automatically.
- `--force`: Overwrite if the file already exists.
- Prompts for mock name, HTTP method, endpoint path, status code, content type, response body, headers, and conditional responses.

### Mock API Definitions (`.mock.json`)

Mock APIs are defined in `.mock.json` files (e.g., `.apify/mocks/users/get-user-by-id.mock.json`).

Structure:

```json
{
  "Name": "Mock User by ID",
  "Method": "GET",
  "Endpoint": "/api/users/:id", // Path parameters with :param or {param}
  "Responses": [
    {
      "Condition": "path.id == \"1\"", // C#-like condition
      "StatusCode": 200,
      "Headers": {
        "X-Source": "Mock-Conditional-User1"
      },
      "ResponseTemplate": {
        "id": 1,
        "name": "John Doe (Mocked)",
        "email": "john.mock@example.com",
        "requested_id": "{{path.id}}",
        "random_code": "{# $.faker.number.int({min: 1000, max: 9999}) #}" // Random number
      }
    },
    {
      "Condition": "$.query.type == \"admin\" && $.headers[\"X-Admin-Token\"] == \"SUPER_SECRET\"",
      "StatusCode": 200,
      "ResponseTemplate": {
        "id": "{{path.id}}",
        "name": "Admin User (Mocked)",
        "email": "admin.mock@example.com",
        "role": "admin",
        "token_used": "{{header.X-Admin-Token}}",
        "uuid": "{# $.faker.string.uuid() #}"
      }
    },
    {
      "Condition": "$.body.status == \"pending\"", // Example for POST/PUT
      "StatusCode": 202,
      "ResponseTemplate": {
        "message": "Request for user {{path.id}} with status 'pending' accepted.",
        "received_payload": "" // Full request body
      }
    },
    {
      "Condition": "default", // Default response if no other conditions match
      "StatusCode": 404,
      "ResponseTemplate": {
        "error": "User not found",
        "id_searched": "{{path.id}}"
      }
    }
  ]
}
```

- **`Endpoint`**: The URL path for the mock. Supports path parameters like `/users/:id` or `/users/{id}`.
- **`Responses`**: An array of conditional response objects. They are evaluated in order.
  - **`Condition`**: A C#-like expression to determine if this response should be used.
    - Access request data:
      - `path.paramName` (e.g., `path.id`)
      - `query.paramName` (e.g., `query.page`)
      - `headers.HeaderName` (e.g., `headers.Authorization`, case-insensitive)
      - `body.fieldName` (e.g., `body.username`, for JSON bodies)
    - `default` can be used for a default fallback response.
  - **`StatusCode`**: The HTTP status code to return.
  - **`Headers`**: An object of response headers.
  - **`ResponseTemplate`**: The body of the response. Can be a JSON object or a string.

### Mock Server Command

Starts a local API mock server using mock definition files.

```bash
apify server:mock [--port <port_number>] [--directory <path_to_mocks>] [--verbose]
```

- `--port <port_number>`: Port for the mock server (default: from `apify-config.json` or 1988).
- `--directory <path_to_mocks>`: Directory containing mock definition files (default: `.apify`).
- `--verbose` or `-v`: Enable verbose logging for the mock server.

> Reads configuration from the `MockServer` block in `apify-config.json` but command-line options take precedence.
