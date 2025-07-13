---
title: "Project Initialization"
weight: 4
---

### Init Command

Initializes a new API testing project. Can be run interactively or with options for non-interactive setup.

| Option | Description | Required | Default |
|--------|-------------|----------|---------|
| `--name` | The name of the API testing project | Yes | - |
| `--mock` | Create sample mock API definitions | No | false |
| `--force` | Force overwrite of existing files | No | false |

Example:
```bash
# Interactive initialization
dotnet run init

# Non-interactive initialization
dotnet run init --name "Payment API Tests" --mock
```

### Configuration Overview

Apify projects can be configured using various methods, allowing you to tailor their behavior to specific needs and environments. Understanding the configuration hierarchy and available options is key to building robust and flexible Apify solutions.

### Configuration Sources

Apify CLI and actors typically read configuration from several sources, processed in a specific order of precedence (later sources override earlier ones):

1.  **`apify.json` (Project Configuration)**: This file, located in the root of your Apify project, defines project-level settings, such as actor name, version, and default input.

2.  **`.env` files (Environment Variables)**: For local development, you can use `.env` files to set environment variables. These are useful for storing sensitive information (like API keys) or environment-specific settings without hardcoding them into your application.

3.  **Command-line Arguments**: When running actors or CLI commands, you can pass configuration options directly as command-line arguments. These often take precedence over file-based configurations.

4.  **Platform Environment Variables**: When your actor runs on the Apify platform, various environment variables are automatically set by the system (e.g., `APIFY_LOCAL_STORAGE_DIR`, `APIFY_ACTOR_ID`). These can also be used to influence your actor's behavior.

### Key Configuration Areas

*   **Input Configuration**: Defining the expected input for your actors, including data types, default values, and validation rules.
*   **Proxy Configuration**: Setting up proxies for your web scraping tasks, including proxy groups and authentication.
*   **Compute Unit Allocation**: Managing how your actor consumes compute units on the Apify platform.
*   **Logging and Error Handling**: Configuring logging levels and how errors are reported.

By effectively utilizing these configuration mechanisms, you can create highly adaptable Apify projects that perform consistently across different environments and use cases.

### Configuration Properties

Apify uses a configuration file (`apify-config.json`) to store environment variables and project settings.

```json
{
  "Name": "My API Project",
  "DefaultEnvironment": "Development",
  "Variables": {
    "projectVar": "project-value"
  },
  "Environments": [
    {
      "Name": "Development",
      "Variables": {
        "baseUrl": "https://dev-api.example.com"
      }
    }
  ]
}
```

This page lists common configuration properties used in Apify projects. These properties can be set in `apify.json`, via environment variables, or as command-line arguments, depending on the specific context.

### General Properties

*   **`APIFY_LOCAL_STORAGE_DIR`**: (Environment Variable) Specifies the directory where local key-value stores, request queues, and datasets are stored when running an actor locally. Defaults to `./apify_storage`.

*   **`APIFY_TOKEN`**: (Environment Variable) Your Apify API token. Required for authenticating with the Apify platform when deploying or running actors remotely.

*   **`APIFY_ACTOR_ID`**: (Environment Variable) The ID of the currently running actor. Automatically set by the Apify platform.

### Actor Input Properties

These properties are typically defined in the actor's input schema or provided as input to the actor run.

*   **`startUrls`**: (Input) An array of URLs from which the crawler should start scraping.

*   **`proxyConfiguration`**: (Input) An object defining proxy settings for the actor, including `useApifyProxy` (boolean) and `proxyUrls` (array of strings).

*   **`maxRequestsPerCrawl`**: (Input) The maximum number of requests the crawler will process during a single run.

*   **`maxPagesPerCrawl`**: (Input) The maximum number of pages the crawler will visit during a single run.

### Crawler-Specific Properties

*   **`handlePageFunction`**: (Code) A JavaScript function that defines how each page should be processed by the crawler.

*   **`handleRequestFunction`**: (Code) A JavaScript function that defines how each request should be handled before being sent.

*   **`preNavigationHooks`**: (Code) An array of functions to be executed before navigation to a new URL.

*   **`postNavigationHooks`**: (Code) An array of functions to be executed after navigation to a new URL.

This is not an exhaustive list, but it covers the most frequently used configuration properties. Refer to the specific actor or library documentation for a complete list of available options.