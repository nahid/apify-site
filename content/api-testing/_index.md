---
title: "API Testing"
weight: 6
---

### Create Request

Creates a new API request definition interactively.

| Option | Description | Required | Default |
|--------|-------------|----------|---------|
| `--file` | The file path for the new API request definition | Yes | - |
| `--force` | Force overwrite if the file already exists | No | false |

The command will interactively prompt for all necessary information, including HTTP method, URI, headers, payload, and assertions.

Example:
```bash
# Create a new API request interactively
dotnet run create request --file users.create
```

### API Test Definition

API tests are defined in JSON files with the following structure:

```json
{
  "Name": "User API Test",
  "Description": "Tests the user endpoints",
  "Uri": "{{baseUrl}}/users/1",
  "Method": "GET",
  "Headers": {
    "Accept": "application/json",
    "Authorization": "Bearer {{token}}"
  },
  "Payload": null,
  "PayloadType": "none",
  "Files": null,
  "Tests": [
    {
      "Name": "Status code is valid",
      "Description": "Checks if status code is 200",
      "AssertType": "StatusCode",
      "ExpectedValue": "200"
    }
  ],
  "Timeout": 30000,
  "Variables": {
    "customVar": "my-custom-value"
  },
  "Tags": ["users", "smoke"]
}
```

### Fields Explained

| Field | Type | Description | Required |
|-------|------|-------------|----------|
| `Name` | String | Name of the API test | Yes |
| `Description` | String | Description of the test | No |
| `Uri` | String | The endpoint URI (supports variable substitution) | Yes |
| `Method` | String | HTTP method (GET, POST, PUT, DELETE, etc.) | Yes |
| `Headers` | Object | HTTP headers as key-value pairs | No |
| `Payload` | String | The request body (can be JSON, text, etc.) | No |
| `PayloadType` | String | Type of payload (none, json, text, formData) | No |
| `Files` | Array | Files to upload (for multipart/form-data) | No |
| `Tests` | Array | Test assertions to validate the response | No |
| `Timeout` | Number | Request timeout in milliseconds | No |
| `Variables` | Object | Custom variables defined in the test file | No |
| `Tags` | Array | Tags for filtering tests | No |

### Call Command

Executes a single API test from a definition file.

| Option | Description | Required | Default |
|--------|-------------|----------|---------|
| `file` | API definition file to test (supports dot notation) | Yes | - |
| `--verbose` or `-v` | Display detailed output | No | false |
| `--env` or `-e` | Environment to use from the profile | No | Profile's default |
| `--vars` | Runtime variables for the configuration | No | - |

Examples:
```bash
# Run a single test using dot notation
dotnet run call users.get

# Run a test with a specific environment and runtime variables
dotnet run call users.create --env Production --vars "userId=123,role=admin"
```

The `call` command supports simplified paths using dot notation:
- `users.get` will run `.apify/users/get.json`
- `auth.login` will run `.apify/auth/login.json`

The `.json` extension is optional.

### Payload Types

Apify supports multiple payload types for flexibility in testing different types of API endpoints.

| Payload Type | Description | Content-Type Header |
|--------------|-------------|---------------------|
| `none` | No request body | None |
| `json` | JSON structured data | application/json |
| `text` | Plain text content | text/plain |
| `formData` | URL-encoded form data | application/x-wWw-form-urlencoded |

### File Uploads

Apify supports file uploads using multipart/form-data.

```json
"Files": [
  {
    "Name": "Profile Picture",
    "FieldName": "avatar",
    "FilePath": "./images/profile.jpg",
    "ContentType": "image/jpeg"
  }
]
```

### Test Assertions

Apify provides comprehensive assertion capabilities to validate API responses.

### Supported Assertion Types

| Assertion Type | Description |
|----------------|-------------|
| `StatusCode` | Validates the HTTP status code |
| `ContainsProperty` | Checks if response JSON contains a specific property |
| `HeaderContains` | Validates a response header's value |
| `ResponseTimeBelow` | Verifies response time is below a threshold |
| `Equal` | Checks if a JSON property equals a specific value |
| `IsArray` | Checks if a JSON property is an array |
| `ArrayNotEmpty` | Verifies a JSON array is not empty |