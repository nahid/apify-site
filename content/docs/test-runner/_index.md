---
title: "Tests Runner"
weight: 7
description: "Run and manage API tests with Apify CLI."
---

The Tests Runner is a command-line utility for executing API tests defined in Apify projects. It supports running tests against both live APIs and mock servers, and generates detailed result reports. By default, all tests are executed in the specified environment, or in the default environment if none is provided.

```bash
apify tests
```

### Command Options

- **`--env`**: Runs tests in the specified environment (e.g., `Development`, `Staging`, `Production`). Defaults to the environment set in `apify-config.json` if omitted.
- **`--vars`**: Lets you define or override custom variables for your requests or tests, for example: `--vars "key1=value1;key2=value2"`. These variables are merged with the current environment and can be accessed using `{{ vars.key1 }}` or `{{ vars.key2 }}` in your placeholders.
- **`--dir`**: Sets the directory containing the tests. Uses the default directory if not specified.
- **`--debug`**: Activates debug mode for more detailed logging during test execution.
- **`--verbose`**: Produces verbose output with comprehensive test execution logs.
