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

### Command Options

- **`--env`**: Specifies the environment name, e.g., "Production".
- **`--vars`**: Lets you define or override custom variables for your requests or tests, for example: `--vars "key1=value1;key2=value2"`. These variables are merged with the current environment and can be accessed using `{{ vars.key1 }}` or `{{ vars.key2 }}` in your placeholders.
- **`--tests`**: Runs all tests in the `.apify` directory.
- **`--show-request`**: Displays the request details before execution.
- **`--show-response`**: Displays the response details after execution.
- **`--show-only-response`**: Only shows the response details, skipping the request.
- **`--verbose`**: Displays detailed output.
- **`--debug`**: Enables debug mode for more detailed output.
- **`--tag`**: Filters tests by tag (e.g., "smoke", "regression").

### Test Assertions

Apify provides comprehensive assertion capabilities using C# expressions to validate API responses. Assertions are defined in the `Tests` array of your API test definition.

- **`Title`**: A descriptive name for the assertion.
- **`Case`**: A C# expression to be evaluated. The expression should return a boolean value. You can use the `Assert` object and its methods to perform assertions.

To know more about assertions, see the [Test Assertions](/docs/api-testing/assertions) section.
