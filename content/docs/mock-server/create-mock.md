---
title: "Create Mock Definition"
---

Interactively creates a new mock API definition file. This command guides you through a step-by-step process where you specify details such as the API endpoint, HTTP methods, request parameters, and example responses. Once completed, a mock definition file is generated, which can be used to simulate API behavior for testing and development purposes.

```bash
apify create:mock <file> [--force]
```

- `<file>`: (Required) The file path for the new mock API definition (e.g., `users.get` becomes `.apify/users/get.mock.json`). The `.mock.json` extension is added automatically.
- `--force`: Overwrite if the file already exists.
- Prompts for mock name, HTTP method, endpoint path, status code, content type, response body, headers, and conditional responses.

### Example

```bash
apify create:mock users.get --prompt --force
```

### Command Arguments

This command requires a single argument:

- `<file>`: The path where the mock definition file will be created. It should follow the format of `<directory>.<schema>`, which translates to `.apify/<directory>/<schema>.mock.json`.

### Command Options

Here are the options you can specify when running the command:

- **`--name`**: The name of the mock API. This is a required field and will be used to identify the mock in the system.
- **`--method`**: The HTTP method for the mock API (e.g., GET, POST, PUT, DELETE). This is required and must be a valid HTTP method.
- **`--endpoint`**: The endpoint path for the mock API (e.g., `/api/users/{id}`). This is required and can include path parameters.
- **`--content-type`**: The content type of the response (e.g., `application/json`). This is required and should match the expected response format.
- **`--status-code`**: The HTTP status code for the response (e.g., 200, 404). This is required and must be a valid HTTP status code.
- **`--response-body`**: The body of the response, which can be a JSON object or a string. This is required and should match the content type specified.
- **`--force`**: Overwrite if the file already exists.
- **`--prompts`**: If set, the command will prompt for additional details interactively, such as headers and conditional responses.
- **`--debug`**: Enable debug mode to log additional information during the command execution.

### Mock API Definitions (`.mock.json`)

Mock APIs are defined in `.mock.json` files, typically located within the `.apify` directory of your project. Each file represents a single mock endpoint and follows a naming convention based on the API resource and operation (for example, `.apify/users/get-user-by-id.mock.json`).

```json
{
  "name": "Get post by ID",
  "method": "GET",
  "endpoint": "/api/posts/{postId}",
  "responses": [
    {
      "condition": "$.path.postId == '1'",
      "statusCode": 200,
      "headers": {
        "X-Source": "Mock-Conditional-User1"
      },
      "responseTemplate": {
        "id": 1,
        "name": "{{ $.faker.person.fullName() }}",
        "email": "{{ $.faker.internet.email() }}",
        "requested_id": "{{ $.path.postId }}",
        "random_code": "{{ $.faker.number.int({ min: 1000, max: 9999 }) }}"
      }
    },
    {
      "condition": "$.query.type == 'admin' && $.headers['x-requested-with'] == 'XMLHttpRequest'",
      "statusCode": 200,
      "responseTemplate": {
        "id": "{{ $.path.postId }}",
        "name": "Admin User (Mocked)",
        "email": "admin.mock@example.com",
        "role": "admin",
        "requested_id": "{{ $.headers['x-requested-with'] }}",
        "uuid": "{{ $.faker.string.uuid() }}"
      }
    },
    {
      "condition": "default",
      "statusCode": 404,
      "responseTemplate": {
        "error": "User not found",
        "id_searched": "{{ $.path.postId }}"
      }
    }
  ]
}
```

### Fields Explained

- **`Name`**: The name of the mock API (e.g., "Get User by ID").
- **`Description`**: Optional description of the mock API.
- **`Method`**: The HTTP method for the mock API (e.g., GET, POST).
- **`Endpoint`**: The endpoint path for the mock API, which can include path parameters (e.g., `/api/users/{id}`).
- **`Responses`**: An array of response definitions, each containing:
  - **`Condition`**: A condition that determines when this response should be used. It can reference path parameters, query parameters, headers, or body content.
  - **`StatusCode`**: The HTTP status code for the response (e.g., 200, 404).
  - **`Headers`**: Optional headers to include in the response.
  - **`ResponseTemplate`**: The body of the response, which can include dynamic values using expressions (e.g., `{{expr|> Faker.Name.FullName()}}`).

#### Path Parameters

Path parameters in the `Endpoint` field can be defined using either `{param}` syntax. For example:

- `GET /api/users/{userId}`
- `GET /api/posts/{postId}/comments/{commentId}`
  You can reference path parameters in both conditions and response templates:

- In **conditions** or expression blocks (e.g., `Condition` or `{{ expr|> ... }}`), access path parameters using `path["paramName"]` (for example, `path["userId"]`).
- In **replacement templates**, use the double curly braces syntax: `{{path.paramName}}` (for example, `{{path.userId}}`).

### Available Variables

You can use the following variables in your mock definitions:

- **`path`**: Contains path parameters from the request URL.
- **`query`**: Contains query parameters from the request URL.
- **`headers`**: Contains HTTP headers from the request.
- **`body`**: Contains the request body (for POST, PUT, PATCH).

Access data in templates with `{{ ... }}` (dot notation)

### Available Reserved Objects

- **`$.path`**: Represents the request path parameters.
- **`$.query`**: Represents the query string parameters.
- **`$.headers`**: Represents the request headers.
- **`$.body`**: Represents the request body.
- **`$.faker`**: Provides access to the [Faker.js](https://fakerjs.dev/) library for generating random data.
