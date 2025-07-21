---
title: "Environment Variables"
weight: 3
---

Apify's variable system enables dynamic configuration of API definitions and mock responses through template substitution. Variables can be set at different levels, each with a specific precedence:

### Variable Precedence

1. **Request-level variables** (highest priority):  
    Defined within an individual API test definition file, these override all other variables.
2. **Environment variables** (medium priority):  
    Specified in the `Environments` section of `apify-config.json`, these override project-level variables.
3. **Project-level variables** (lowest priority):  
    Set in the root `Variables` object of `apify-config.json`, these provide default values.

Reference variables using the `{{variableName}}` syntax in URLs, headers, and response bodies.

### Managing Environments

Configure multiple environments (such as development, staging, or production) in the `Environments` array of `apify-config.json`. Each environment can define its own variables, which take precedence over project-level variables.

### Example Environment Configuration

You can define multiple environments in the `Environments` array of `apify-config.json`. Each environment can have its own set of variables.

```json
{
    "Environments": [
        {
            "Name": "Development",
            "Variables": {
                "API_BASE_URL": "https://dev.api.example.com",
                "AUTH_TOKEN": "dev-token"
            }
        },
        {
            "Name": "Production",
            "Variables": {
                "API_BASE_URL": "https://api.example.com",
                "AUTH_TOKEN": "prod-token"
            }
        }
    ]
}
```

### Using Variables in API Definitions

When defining API requests, you can reference environment variables or custom variables directly in your API definition files. For example:

```json
{
    "Method": "GET",
    "Url": "{{ env.API_BASE_URL }}/users",
    "Headers": {
        "Authorization": "Bearer {{ env.AUTH_TOKEN }}"
    }
}
```



### Listing Environments and Variables

```bash
apify list-env
```

### Defining Global Variables

Global variables can be added directly to API definition files under the `Variables` object. These are scoped to all requests and override both environment and request-level variables with the same name.

```json
{
    "Variables": {
        "CUSTOM_VAR": "custom-value"
    }
}
```

To display all environments and their variables defined in `apify-config.json`, use:

```bash
apify list-env
```

