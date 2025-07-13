---
title: "Getting Started"
weight: 1
---

### Initializing a New Project

To create a new API testing project with default configuration:

```bash
dotnet run init --name "My API Project"
```

This will:
- Create a `.apify` directory to store your API test definitions
- Generate a configuration file `apify-config.json` with development and production environments
- Create sample API test files in the `.apify` directory

### Running Your First Test

After initialization, you can run the sample test:

```bash
dotnet run call get
```

For more detailed output, use the verbose flag:

```bash
dotnet run call get --verbose
```

For debugging information, use the debug flag:

```bash
dotnet run call get --debug
```