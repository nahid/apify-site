---
title: "Variables"
weight: 5
---

Apify provides a powerful variable system that supports template substitution in API definitions and mock responses. Variables can be defined at multiple levels with clear precedence:

### Variable Precedence

1.  **Request-level variables** (highest priority): Defined directly within an API test definition file.
2.  **Environment variables** (medium priority): Defined within a specific environment in `apify-config.json`.
3.  **Project-level variables** (lowest priority): Defined at the root `Variables` object in `apify-config.json`.

Variables are referenced using `{{variableName}}` syntax within URLs, headers, and response bodies.

### Environment Management

Apify allows you to manage different configurations for various environments (e.g., development, staging, production) using the `Environments` array in `apify-config.json`.

Each environment can have its own set of variables that override project-level variables.

### List Environment Command

Lists all available environments and their variables from `apify-config.json`.

```bash
apify list-env
```

### Custom Variables

You can define custom variables directly in API definition files within the `Variables` object. These variables will only be available for that specific request and will override any environment or project-level variables with the same name.
