---
title: "Import"
description: "Import API definitions and mock responses into Apify."
---

Apify provides commands to import API definitions and mock responses from various formats, including Postman collections. This feature allows you to quickly set up your API testing environment with existing definitions.

# Import Postman Collection (`import:postman`)

The `import:postman` command allows you to import a Postman collection and convert it into Apify's file-based format. This is useful for migrating your existing API collections to Apify.

```bash
apify import:postman <file> [options]
```

## Arguments

- `<file>` (required): The path to the Postman collection JSON file.

## Options

- `--output-dir <directory>`: The directory where the Apify collection will be saved. Defaults to `requests`.
- `--force`: Force overwrite if the output directory already exists.
- `--env-file <file>`: The path to the Postman environment JSON file. If provided, the variables from this file will be imported into your `apify-config.json`.

The `import:postman` command performs the following actions:

1.  **Reads a Postman Collection:** It takes a Postman collection JSON file as input.
2.  **Creates Apify Requests:** It iterates through the items in the Postman collection and creates a corresponding `.json` file for each request in the specified output directory.
3.  **Imports Environment Variables:** If you provide a Postman environment file using the `--env-file` option, it will extract the variables and add them to your `apify-config.json` file under the default environment.
4.  **Imports Authorization:** If the Postman collection has authorization configured at the collection level (Bearer Token or Basic Auth), it will be imported into the `apify-config.json` file. Request-level authorization is also imported into the individual request files.
5.  **Variable Transformation:** It automatically converts Postman's `{{variable}}` syntax to Apify's `{{env.variable}}` syntax.

## Examples

### Basic Import

```bash
apify import:postman /path/to/your/postman_collection.json
```

This command will create a new `requests` directory (or overwrite it if you use `--force`) and populate it with `.json` files corresponding to the requests in your Postman collection.

### Import with an Environment File

```bash
apify import:postman /path/to/your/postman_collection.json --env-file /path/to/your/postman_environment.json
```

This will do the same as the basic import, but it will also add the variables from your Postman environment to your `apify-config.json`.

### Specifying an Output Directory

```bash
apify import:postman /path/to/your/postman_collection.json --output-dir my-api-collection
```

This will create the `my-api-collection` directory and save the request files there.
