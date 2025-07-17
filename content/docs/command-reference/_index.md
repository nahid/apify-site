---
title: "Command Reference"
weight: 3
---

Apify commands are run as `apify <command> [subcommand] [options]` if installed globally, or `dotnet run -- <command> [subcommand] [options]` if run from the project directory.

### `apify init`
Initializes a new API testing project in the current directory.

```bash
apify init [--force]
```
- `--force`: Overwrite existing `apify-config.json` and `.apify` directory if they exist.
- Prompts for project name, default environment name, and other environments to create.
- Creates `apify-config.json`, `.apify/` directory with sample test and mock files.

### `apify create:request`
Interactively creates a new API test definition file.

```bash
apify create:request <file> [--force]
```
- `<file>`: (Required) The file path for the new API request definition (e.g., `users.all` becomes `.apify/users/all.json`). The `.json` extension is added automatically.
- `--force`: Overwrite if the file already exists.
- Prompts for request name, HTTP method, URI, headers, payload, and basic assertions.

### `apify create:mock`
Interactively creates a new mock API definition file.

```bash
apify create:mock <file> [--force]
```
- `<file>`: (Required) The file path for the new mock API definition (e.g., `users.get` becomes `.apify/users/get.mock.json`). The `.mock.json` extension is added automatically.
- `--force`: Overwrite if the file already exists.
- Prompts for mock name, HTTP method, endpoint path, status code, content type, response body, headers, and conditional responses.

### `apify call`
Executes an API test from a specified definition file.

```bash
apify call <file> [--env <environment_name>] [--verbose]
```
- `<file>`: (Required) An API definition file path (e.g., `users/all.json`). Dot notation like `users.all` is also supported.
- `--env <environment_name>`: Specifies the environment to use (e.g., "Production"). Uses default from `apify-config.json` if not set.
- `--verbose` or `-v`: Displays detailed output, including request and response bodies.

### `apify tests`
Runs all API tests found in the `.apify` directory and its subdirectories.

```bash
apify tests [--env <environment_name>] [--tag <tag_name>] [--verbose]
```
- `--env <environment_name>`: Specifies the environment.
- `--tag <tag_name>`: Filters and runs only tests that have the specified tag in their definition.
- `--verbose` or `-v`: Displays detailed output.
- Shows visual progress indicators and a summary at the end.

### `apify server:mock`
Starts a local API mock server using mock definition files.

```bash
apify server:mock [--port <port_number>] [--directory <path_to_mocks>] [--verbose]
```
- `--port <port_number>`: Port for the mock server (default: from `apify-config.json` or 1988).
- `--directory <path_to_mocks>`: Directory containing mock definition files (default: `.apify`).
- `--verbose` or `-v`: Enable verbose logging for the mock server.
- Reads configuration from the `MockServer` block in `apify-config.json` but command-line options take precedence.

### `apify list-env`
Lists all available environments and their variables from `apify-config.json`.

```bash
apify list-env
```

### Global Options
These options can be used with most commands.

- `--debug`: Show detailed debug output, including stack traces and internal logging. Useful for troubleshooting Apify itself.