---
title: "Run Mock Server"
---

Launches a local API mock server based on mock definition files, allowing you to simulate API endpoints for testing and development.

```bash
apify server:mock [--watch]
```

- `--watch`: Automatically reloads the server when mock definition files change.

This command reads configuration from the `MockServer` block in `apify-config.json` but command-line options take precedence.

### Example

```bash
apify server:mock --port 3000 -w
```

### Command Options

- **`--port <port_number>`**: Specify the port on which the mock server will run. If not provided, it defaults to the port specified in `apify-config.json` or 1988 if not set.
- **`--project <path>`**: Specify the directory where mock definition files are located. Defaults to `.apify`.
- **`--watch`**: Enable automatic reloading of the server when mock definition files change.
