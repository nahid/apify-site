---
title: "Execute API Test"

---


Executes an API test based on a specified definition file. The command reads the API request and expected response details from the provided file, sends the request to the target API endpoint, and evaluates the response using the assertions defined in the test file. This allows you to automate the validation of API endpoints, ensuring they behave as expected under different scenarios. You can specify the environment, enable verbose output for detailed logs, and use either JSON or dot notation for the file path. The results, including assertion outcomes and any errors, are displayed in the console.

```bash
apify call <file> [--env <environment_variables>] [--verbose]
```
- `<file>`: (Required) An API definition file path (e.g., `users/all.json`). Dot notation like `users.all` is also supported.
- `--env <environment_name>`: Specifies the environment to use (e.g., "Production"). Uses default from `apify-config.json` if not set.
- `--verbose` or `-v`: Displays detailed output, including request and response bodies.

### Running All Tests

Runs all API tests found in the `.apify` directory and its subdirectories.

```bash
apify tests [--env <environment_name>] [--tag <tag_name>] [--verbose]
```
- **`--env`**: Specifies the environment name, e.g., "Production".
- **`--vars`**: Allows passing additional variables to the request/tests.
- **`--tests`**: Runs all tests in the `.apify` directory.
- **`--show-request`**: Displays the request details before execution.
- **`--show-response`**: Displays the response details after execution.
- **`--show-only-response`**: Only shows the response details, skipping the request.
- **`--verbose`**: Displays detailed output.
- **`--debug`**: Enables debug mode for more detailed output.
- **`--tag`**: Filters tests by tag (e.g., "smoke", "regression").


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
