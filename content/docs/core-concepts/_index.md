---
title: "Core Concepts"
weight: 2
description: "Understanding Apify's core concepts for API testing and mocking."
---

## Overview

Apify provides a robust framework for defining, running, and mocking API tests in your projects. This guide covers the foundational concepts, including project setup, configuration, test definitions, and mock API responses.

---

### 1. Project Initialization

Start by initializing your project with Apify:

```bash
apify init
```

This command will guide you through:

- Naming your project
- Setting a default environment (e.g., "Development")
- Adding additional environments (e.g., "Staging", "Production")

After initialization, your project will include:

- `apify-config.json`: Central configuration for environments, variables, and mock server.
- `.apify/`: Directory for API test definitions (`.json`) and mock definitions (`.mock.json`).
- Sample test and mock files to help you get started.

---

### 2. Configuration (`apify-config.json`)

This JSON file defines global project settings, environments, and mock server options.

**Example:**

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
      "Variables": {
        "baseUrl": "https://dev-api.myproject.com",
        "apiKey": "dev-secret-key"
      }
    },
    {
      "Name": "Production",
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

**Key Sections:**

- **Name/Description**: Project metadata.
- **DefaultEnvironment**: Used if `--env` is not specified.
- **Variables**: Project-wide variables, overridable by environments or requests.
- **Environments**: Define environment-specific variables.
- **MockServer**: Configure the built-in mock server.

---

### 3. API Test Definitions

API tests are stored as JSON files in `.apify/` (e.g., `.apify/users/get-users.json`).

**Structure:**

```json
{
  "Name": "Get All Users",
  "Description": "Fetches the list of all users",
  "Url": "{{env.baseUrl}}/users?page={{env.defaultPage}}",
  "Method": "GET",
  "Headers": {
    "Accept": "application/json",
    "X-Api-Key": "{{env.apiKey}}"
  },
  "Variables": {
    "defaultPage": "1",
    "apiKey": "abcxyz123",
    "baseUrl": "https://api.example.com"
  },
  "Tests": [
    {
      "Title": "Status code is 200 OK",
      "Case": "$.response.getStatusCode() == 200"
    }
  ]
}
```

**Concepts:**

- **Variables**: Request-level variables override environment/project variables.
- **Tags**: (Optional) For filtering tests.
- **Tests**: Each assertion includes a title and a ES6(JavaScript) expression using the `Assert` object.

---

### 4. Mock API Definitions

Mocks are defined in `.mock.json` files under `.apify/mocks/` (e.g., `.apify/mocks/users/get-user-by-id.mock.json`).

**Structure:**

```json
{
  "Name": "Mock User by ID",
  "Method": "GET",
  "Endpoint": "/api/users/{id}",
  "Responses": [
    {
      "Condition": "$.path.id == 1",
      "StatusCode": 200,
      "Headers": {
        "X-Source": "Mock-Conditional-User1"
      },
      "ResponseTemplate": {
        "id": 1,
        "name": "John Doe (Mocked)",
        "email": "john.mock@example.com",
        "requested_id": "{{path.id}}",
        "random_code": "{# $.faker.datatype.number({min: 1000, max: 9999}) #}"
      }
    },
    {
      "Condition": "default",
      "StatusCode": 404,
      "ResponseTemplate": {
        "error": "User not found",
        "id_searched": "{{path.id}}"
      }
    }
  ]
}
```

**Key Points:**

- **Endpoint**: Supports path parameters (`{id}`).
- **Responses**: Evaluated in order; first matching condition is used.
  - **Condition**: ES6(JavaScript) expression using request data (`path`, `query`, `headers`, `body`).
  - **ResponseTemplate**: Supports template variables for dynamic responses, including random data via Faker.

---

### 5. Variable Resolution

Variables are resolved in the following order (highest to lowest precedence):

1. Request-level (`Variables` in test definition)
2. Environment-level (`Variables` in environment)
3. Project-level (`Variables` in config)

---

### 6. Mock Server

The built-in mock server can be started using your configuration. It serves mock responses based on your `.mock.json` files, supporting dynamic templating and conditional logic.

---

For more advanced usage, see the full documentation on test assertions, environment management, and mock server customization.
