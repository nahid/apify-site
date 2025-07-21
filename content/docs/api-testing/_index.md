---
title: "API Testing"
weight: 6
---

Apify provides comprehensive capabilities for defining and running detailed API tests.

### Features

- **Multiple Request Methods**: Support for GET, POST, PUT, DELETE, PATCH, HEAD, OPTIONS.
- **Rich Payload Types**: JSON, Text, Form Data.
- **File Upload Support**: Test multipart/form-data and raw binary requests with file uploads.
- **Detailed Assertions**: Validate response status, headers, body content (JSON properties, arrays, values), and response time.

### Create Request

Creates a new API test definition file interactively.

```bash
apify create:request <file> [--prompt] [--force]
```
- `<file>`: (Required) The file path for the new API request definition (e.g., `users.all` becomes `.apify/users/all.json`). The `.json` extension is added automatically.
- `--force`: Overwrite if the file already exists.
- Prompts for request name, HTTP method, URI, headers, payload, and basic assertions.

### Example

```bash
apify create:request users.all --prompt
```

This command will create a new API test definition file at `.apify/users/all.json` with an interactive prompt to fill in the details. 

#### Command Arguments
- **`<file>`**: The path where the API test definition will be created.

#### Command Options
- **`--name`**: Specify a custom name for the request (e.g., `--name "Get All Users"`).
- **`--method`**: Specify the HTTP method (e.g., `--method GET`).
- **`--url`**: Specify the request URL (e.g., `--url "{{env.baseUrl}}/users"`).
- **`--prompt`**: Use interactive prompts to fill in the request details.
- **`--force`**: Overwrite existing files without confirmation.
- **`--debug`**: Enable debug mode for more detailed output during creation.



### API Test Definition

API tests are defined in JSON files (e.g., `.apify/users/all.json`).

Structure:
```json
{
  "Name": "Get All Users",
  "Description": "Fetches the list of all users",
  "Url": "{{env.baseUrl}}/users?page={{defaultPage}}",
  "Method": "GET",
  "Headers": {
    "Accept": "application/json",
    "X-Api-Key": "{{env.apiKey}}"
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
    }
  ]
}
```

### Fields Explained

- **`Name`**: Name of the API test (e.g., "Get All Users").
- **`Description`**: Optional description of the test.
- **`Url`**: The endpoint URL (supports variable substitution).
- **`Method`**: HTTP method (GET, POST, PUT, DELETE, PATCH, HEAD, OPTIONS).
- **`Headers`**: HTTP headers as key-value pairs.
- **`PayloadType`**: Type of payload (`none`, `json`, `text`, `formData`, `multipart`, `binary`).
- **`Body`**: The request body (for POST, PUT, PATCH). Can be JSON, text, etc.
  - **`Json`**: JSON object for `json` payload type.
  - **`Text`**: Plain text for `text` payload type.
  - **`FormData`**: URL-encoded form data for `formData`
  - **`Multipart`**: For `multipart/form-data`, specify files in the `Files` array.
  - **`Binary`**: For raw binary data, specify the file path.
  - **`Binary`**: For raw binary data, specify the file path.
- **`Timeout`**: Request timeout in milliseconds (optional).
- **`Tags`**: Array of strings for categorizing and filtering tests (optional).
- **`Variables`**: Key-value pairs specific to this request (highest precedence).
- **`Tests`**: A list of assertion objects to validate the response.

### Call Command

Executes an API test from a specified definition file.

```bash
apify call <file> [--env <environment_name>] [--verbose]
```
- `<file>`: (Required) An API definition file path (e.g., `users/all.json`). Dot notation like `users.all` is also supported.
- `--env <environment_name>`: Specifies the environment to use (e.g., "Production"). Uses default from `apify-config.json` if not set.
- `--verbose` or `-v`: Displays detailed output, including request and response bodies.

### Running All Tests

Runs all API tests found in the `.apify` directory and its subdirectories.

```bash
apify tests [--env <environment_name>] [--tag <tag_name>] [--verbose]
```
- `--env <environment_name>`: Specifies the environment.
- `--tag <tag_name>`: Filters and runs only tests that have the specified tag in their definition.
- `--verbose` or `-v`: Displays detailed output.
- Shows visual progress indicators and a summary at the end.

### Payload Types

Apify supports multiple payload types for flexibility in testing different types of API endpoints.

| Payload Type | Description | Content-Type Header (auto-set) |
|--------------|-------------|--------------------------------|
| `none`       | No request body | None                           |
| `json`       | JSON structured data | `application/json`             |
| `text`       | Plain text content | `text/plain`                   |
| `formData`   | URL-encoded form data | `application/x-www-form-urlencoded` |

### File Uploads

For `multipart/form-data` requests, you can specify files to upload within your test definition:

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

Apify provides comprehensive assertion capabilities using C# expressions to validate API responses. Assertions are defined in the `Tests` array of your API test definition.

- **`Title`**: A descriptive name for the assertion.
- **`Case`**: A C# expression to be evaluated. The expression should return a boolean value. You can use the `Assert` object and its methods to perform assertions.

**Available Assertions (via `Assert.Response` object):**

| Method | Description | Example `Case` |
|---|---|---|
| `StatusCodeIs(int code)` | Checks if the HTTP status code matches. | `Assert.Response.StatusCodeIs(200)` |
| `HeaderExists(string name)` | Checks if a specific header exists. | `Assert.Response.Headers.HeaderExists("Content-Type")` |
| `HeaderContains(string name, string value)` | Checks if a header contains a specific value. | `Assert.Response.Headers.HeaderContains("Content-Type", "application/json")` |
| `Json.PropertyExists(string path)` | Checks if a JSON property exists at the given path. | `Assert.Response.Json.PropertyExists("data.id")` |
| `Json.PropertyEquals(string path, object value)` | Checks if a JSON property equals a specific value. | `Assert.Response.Json.PropertyEquals("data.name", "John Doe")` |
| `Json.IsArray(string path)` | Checks if a JSON property is an array. | `Assert.Response.Json.IsArray("data")` |
| `Json.ArrayNotEmpty(string path)` | Checks if a JSON array is not empty. | `Assert.Response.Json.ArrayNotEmpty("data")` |
| `Json.ArrayLength(string path, int length)` | Checks if a JSON array has a specific length. | `Assert.Response.Json.ArrayLength("data", 5)` |
| `ResponseTimeBelow(int milliseconds)` | Checks if the response time is below a threshold. | `Assert.Response.ResponseTimeBelow(500)` |

You can also use direct C# comparisons and LINQ queries on the `Response.Json` object (which is a `JObject` or `JArray` from Newtonsoft.Json) for more complex validations. For example:

- `Response.Json.SelectToken("$.data[0].status").ToString() == "active"`
- `Response.Json["data"].Count() > 0`
- `Response.Json["errors"].Any(e => e["code"].ToString() == "INVALID_INPUT")`
