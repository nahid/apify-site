---
title: "Initialization"
weight: 3
---

Initializes a new API testing project in the current directory. To start using Apify in your project, navigate to your project's root directory and run:

```bash
apify init [--force]
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

#### Command Options

- `--force`: Overwrite existing `apify-config.json` and `.apify` directory if they exist.
- `--name`: Specify a custom project name.
- `--force`: Overwrite existing files without prompting.
- `--debug`: Enable debug mode for more verbose output.
- Prompts for project name, default environment name, and other environments to create.
- Creates `apify-config.json`, `.apify/` directory with sample test and mock files.

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

#### Key Fields

_(\*) Required fields_

- **`Name`**: Name of the project.
- **`Description`**: Project metadata.
- **`DefaultEnvironment`**: The environment used if `--env` is not specified.
- **`Variables` (Project-Level)**: Key-value pairs available across all tests and environments unless overridden.
- **`Options`** - (optional): Additional global options for the API testing.
  - **`Verbose`**: Enable verbose logging for the CLI.
  - **`Tests`**: Enable test runner globally for all requests.
  - **`ShowRequest`**: Show request details in the console.
  - **`ShowResponse`**: Show response details in the console.
  - **`ShowOnlyResponse`**: Show only response details in the console.
- **`Authorization`** - (optional): Global authorization settings.
  - **`Type`**: Type of authorization (e.g., "Bearer", "Basic").
  - **`Token`**: Authorization token or credentials.
- **`Environments`**: An array of environment objects.
  - **`Name`**: Unique name for the environment (e.g., "Development", "Staging").
  - **`Variables`**: Key-value pairs specific to this environment. These override project-level variables.
- **`MockServer`**: Configuration for the mock server.
  - **`Port`**: Port for the mock server.
  - **`Verbose`**: Enable verbose logging for the mock server.
  - **`EnableCors`**: Enable CORS headers (defaults to allow all).
  - **`DefaultHeaders`**: Headers to be added to all mock responses.
