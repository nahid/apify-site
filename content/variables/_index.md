---
title: "Variables"
weight: 5
---

### List Environment Command

Lists all available environments and their variables.

```bash
dotnet run list-env
```

### Variable System

Apify provides a powerful variable system that supports template substitution in API definitions. Variables can be defined at three levels, in order of precedence:

1.  **Request-level variables** (highest priority)
2.  **Environment variables** (medium priority)
3.  **Project-level variables** (lowest priority)

Variables are referenced using `{{variableName}}`.

### Custom Variables

You can define custom variables directly in API definition files within the `Variables` object.