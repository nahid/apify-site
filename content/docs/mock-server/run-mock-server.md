---
title: "Run Mock Server"
---

Launches a local API mock server based on mock definition files, allowing you to simulate API endpoints for testing and development.

```bash
apify server:mock [--port <port_number>]
```

- `--port <port_number>`: Port for the mock server (default: from `apify-config.json` or 1988).

This command reads configuration from the `MockServer` block in `apify-config.json` but command-line options take precedence.

### Example

```bash
apify server:mock --port 3000
```

### Command Options

- **`--port <port_number>`**: Specify the port on which the mock server will run. If not provided, it defaults to the port specified in `apify-config.json` or 1988 if not set.
- **`--directory <path>`**: Specify the directory where mock definition files are located. Defaults to `.apify/mocks`.
