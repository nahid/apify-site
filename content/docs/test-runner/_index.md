---
title: "Tests Runner"
weight: 7
---

The Tests Runner is a command-line utility for executing API tests defined in Apify projects. It supports running tests against both live APIs and mock servers, and generates detailed result reports. By default, all tests are executed in the specified environment, or in the default environment if none is provided.

```bash
apify tests
```

### Command Options

- **`--env`**: Runs tests in the specified environment (e.g., `Development`, `Staging`, `Production`). Defaults to the environment set in `apify-config.json` if omitted.
- **`--vars`**: Overrides or adds environment variables, using the format `--vars "var1=value1;var2=value2"`.
- **`--dir`**: Sets the directory containing the tests. Uses the default directory if not specified.
- **`--debug`**: Activates debug mode for more detailed logging during test execution.
- **`--verbose`**: Produces verbose output with comprehensive test execution logs.
