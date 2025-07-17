---
title: "Core Concepts"
weight: 2
---

### Project Initialization (`apify init`)

To start using Apify in your project, navigate to your project's root directory and run:

```bash
apify init
```

This command interactively prompts for:
- Project Name
- Default Environment Name (e.g., "Development")
- Additional environments (e.g., "Staging,Production")

It creates:
- `apify-config.json`: The main configuration file.
- `.apify/`: A directory to store your API test definitions (`.json`) and mock definitions (`.mock.json`).
- Sample API test and mock definition files within `.apify/` and `.apify/mocks/` respectively.
- A `MockServer` configuration block in `apify-config.json`.

### Configuration (`apify-config.json`)

This file stores project-level settings, environments, and mock server configuration.

```json
{
  "Name": "My Project API Tests",
  "Description": "API Tests for My Project",
  "DefaultEnvironment": "Development",
  "Variables": {
    "globalProjectVar": "This variable is available in all environments and tests"
  },
  "Environments": [
    {
      "Name": "Development",
      "Description": "Development environment specific variables",
      "Variables": {
        "baseUrl": "https://dev-api.myproject.com",
        "apiKey": "dev-secret-key"
      }
    },
    {
      "Name": "Production",
      "Description": "Production environment specific variables",
      "Variables": {
        "baseUrl": "https://api.myproject.com",
        "apiKey": "prod-secret-key"
      }
    }
  ],
  "MockServer": {
    "Port": 8080,
    "Directory": ".apify/mocks",
    "Verbose": false,
    "EnableCors": true,
    "DefaultHeaders": {
      "X-Mock-Server": "Apify"
    }
  }
}
```

- **`Name`, `Description`**: Project metadata.
- **`DefaultEnvironment`**: The environment used if `--env` is not specified.
- **`Variables` (Project-Level)**: Key-value pairs available across all tests and environments unless overridden.
- **`Environments`**: An array of environment objects.
    - **`Name`**: Unique name for the environment (e.g., "Development", "Staging").
    - **`Variables`**: Key-value pairs specific to this environment. These override project-level variables.
- **`MockServer`**: Configuration for the mock server.
    - **`Port`**: Port for the mock server.
    - **`Verbose`**: Enable verbose logging for the mock server.
    - **`EnableCors`**: Enable CORS headers (defaults to allow all).
    - **`DefaultHeaders`**: Headers to be added to all mock responses.

### API Test Definitions (`.json`)

API tests are defined in JSON files (e.g., `.apify/users/get-users.json`).

Structure:
```json
{
  "Name": "Get All Users",
  "Description": "Fetches the list of all users",
  "Url": "{{baseUrl}}/users?page={{defaultPage}}",
  "Method": "GET",
  "Headers": {
    "Accept": "application/json",
    "X-Api-Key": "{{apiKey}}"
  },
  "Body": null,
  "PayloadType": "none", // "json", "text", "formData"
  "Timeout": 30000, // Optional, in milliseconds
  "Tags": ["users", "smoke"], // Optional, for filtering tests
  "Variables": { // Request-specific variables (highest precedence)
    "defaultPage": "1"
  },
  "Tests": [
    {
      "Title": "Status code is 200 OK",
      "Case": "Assert.Response.StatusCodeIs(200)"
    },
    {
      "Title": "Response body is an array",
      "Case": "Assert.Response.Json.data.Type == JTokenType.Array"
    }
  ]
}
```

- **`Variables` (Request-Level)**: Override environment and project variables.
- **`Tags`**: Used for filtering tests with `apify tests --tag <tagname>`.
- **`Tests` (Assertions)**: A list of assertion objects.
    - **`Title`**: A descriptive name for the assertion.
    - **`Case`**: A C# expression to be evaluated. The expression should return a boolean value. You can use the `Assert` object and its methods to perform assertions.

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
        "random_code": "{{expr|> Faker.Random.Int(1000,9999)}}"
      }
    },
    {
      "Condition": "query.type == \"admin\" && header.X-Admin-Token == \"SUPER_SECRET\"",
      "StatusCode": 200,
      "ResponseTemplate": {
        "id": "{{path.id}}",
        "name": "Admin User (Mocked)",
        "email": "admin.mock@example.com",
        "role": "admin",
        "token_used": "{{header.X-Admin-Token}}",
        "uuid": "{{expr|> Faker.Random.Uuid()}}"
      }
    },
    {
      "Condition": "body.status == \"pending\"", // Example for POST/PUT
      "StatusCode": 202,
      "ResponseTemplate": {
        "message": "Request for user {{path.id}} with status 'pending' accepted.",
        "received_payload": "{{body}}" // Full request body
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
        - **Template Variables**:
            - `{{path.paramName}}`: Value of a path parameter.
            - `{{query.paramName}}`: Value of a query parameter.
            - `{{headers.HeaderName}}`: Value of a request header (case-insensitive).
            - `{{body.fieldName}}`: Value of a field from the JSON request body.
            - `{{body}}`: The full raw request body (string).
            - `{{expr|> Faker.Random.Int(min,max)}}`: A random integer between min and max (inclusive). E.g., `{{expr|> Faker.Random.Int(1,100)}}`.
            - `{{expr|> Faker.Random.Uuid()}}`: A random UUID.
            - `{{expr|> Faker.Date.Recent()}}`: A recent date.
            - Any environment or project variable (e.g., `{{baseUrl}}`).
