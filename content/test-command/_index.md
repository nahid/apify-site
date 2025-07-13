---
title: "Test Command"
weight: 7
---

The tests command scans the project directory and runs all API tests found.

| Option | Description | Required | Default |
|--------|-------------|----------|---------|
| `--verbose` or `-v` | Display detailed output including response body | No | false |
| `--dir` | The directory containing API test files | No | .apify |
| `--env` or `-e` | Environment to use from the profile | No | Profile's default |
| `--vars` | Runtime variables for the configuration | No | - |
| `--tag` | Filter tests by tag | No | - |

Example:
```bash
# Run all tests in the project
dotnet run tests

# Run all tests in a specific directory with verbose output
dotnet run tests --dir "apis" --verbose

# Run only tests with a specific tag
dotnet run tests --tag payments
```