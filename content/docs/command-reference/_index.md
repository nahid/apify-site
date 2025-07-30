---
title: "Command Reference"
weight: 3
description: "Apify CLI command reference for API testing and management."
---

Apify CLI provides commands to help you create, manage, and test API projects. You can run commands globally as `apify <command> [options]`

## Commands

### `apify init`

Set up a new API testing project in your current directory.

```bash
apify init [--force]
```

- `--force`: Overwrite existing `apify-config.json` and `.apify` directory if present.
- Prompts for project and environment details.
- Generates `apify-config.json` and a `.apify/` directory with sample files.

### `apify create:request`

Create a new API test definition interactively.

```bash
apify create:request <file> [--force]
```

- `<file>`: (Required) Path for the new API request definition (e.g., `users.all` → `.apify/users/all.json`).
- `--force`: Overwrite if the file exists.
- Guided prompts for request details and assertions.

### `apify create:mock`

Create a new mock API definition interactively.

```bash
apify create:mock <file> [--force]
```

- `<file>`: (Required) Path for the new mock definition (e.g., `users.get` → `.apify/users/get.mock.json`).
- `--force`: Overwrite if the file exists.
- Guided prompts for mock details, responses, and conditions.

### `apify call`

Run an API test from a definition file.

```bash
apify call <file> [--env <environment>] [--verbose]
```

- `<file>`: (Required) API definition file (e.g., `users/all.json` or `users.all`).
- `--env <environment>`: Specify environment (uses default if omitted).
- `--verbose`, `-v`: Show detailed request/response output.

### `apify tests`

Run all API tests in the `.apify` directory.

```bash
apify tests [--env <environment>] [--tag <tag>] [--verbose]
```

- `--env <environment>`: Specify environment.
- `--tag <tag>`: Run only tests with the given tag.
- `--verbose`, `-v`: Show detailed output.
- Displays progress and summary.

### `apify server:mock`

Start a local mock server using your mock definitions.

```bash
apify server:mock [--port <port>] [--directory <mocks_dir>] [--verbose]
```

- `--port <port>`: Port for the server (default: from config or 1988).
- `--project <mocks_dir>`: Project directory with mock files (default: `.apify`).
- `--verbose`, `-v`: Enable verbose logging.
- Reads settings from `apify-config.json` (`MockServer` block); CLI options override config.

### `apify list:env`

Show all environments and their variables from `apify-config.json`.

```bash
apify list-env
```

## Global Options

- `--debug`: Show debug output, including stack traces and internal logs. Useful for troubleshooting.
