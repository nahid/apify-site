---
title: "Create Request"
---

Interactively guides you through the process of creating a new API test definition file. You will be prompted to provide details such as the request name, HTTP method, endpoint URL, headers, payload, and basic assertions. Once completed, the tool generates a structured JSON file in the `.apify` directory, ready to be used for automated API testing.

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
  "Url": "{{env.baseUrl}}/users?page={{env.defaultPage}}",
  "Method": "GET",
  "Headers": {
    "Accept": "application/json",
    "X-Api-Key": "{{env.apiKey}}"
  },
  "Body": null,
  "PayloadType": "none", // "json", "text", "formData"
  "Timeout": 30000, // Optional, in milliseconds
  "Tags": ["users", "smoke"], // Optional, for filtering tests
  "Variables": {
    // Request-specific variables (highest precedence)
    "defaultPage": "1"
  },
  "Tests": [
    {
      "Title": "Status code is 200 OK",
      "Case": "$.response.statusCode == 200"
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

### Payload Types (`PayloadType`)

Apify supports multiple payload types for flexibility in testing different types of API endpoints.

| Payload Type | Description                         | Content-Type Header (auto-set)      |
| ------------ | ----------------------------------- | ----------------------------------- |
| `none`       | No request body                     | None                                |
| `json`       | JSON structured data                | `application/json`                  |
| `text`       | Plain text content                  | `text/plain`                        |
| `formData`   | URL-encoded form data               | `application/x-www-form-urlencoded` |
| `multipart`  | Multipart form data (file uploads)  | `multipart/form-data`               |
| `binary`     | Raw binary data (e.g., file upload) | `application/octet-stream`          |

### Body Type (`Body`)

The `Body` field in the API test definition specifies the content of the request body based on the `PayloadType`, it's an optional field. Depending on the type, you can provide different formats:

- **`Json`**: For `json` payload type, specify a JSON object.
- **`Text`**: For `text` payload type, specify a string.
- **`FormData`**: For `formData` payload type, specify key-value pairs, e.g., `{"key": "value"}`.
- **`Multipart`**: For `multipart` payload type, specify files in the `Files` array.
  - **`name`**: The name of the file in the form.
  - **`content`**: The content of the file, which can be a string or binary data.
- **`Binary`**: For `binary` payload type, specify the file path.

### Test Assertions

Apify provides comprehensive assertion capabilities using ES6(JavaScript) expressions to validate API responses. Assertions are defined in the `Tests` array of your API test definition. To know more about assertions, see the [Test Assertions](/docs/api-testing/assertions) section.
