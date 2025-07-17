---
title: "CI/CD with GitHub Actions"
weight: 14
---

This project includes GitHub Actions workflows for continuous integration and releases.

### Continuous Integration

The CI workflow (if included in your project, typically in `.github/workflows/ci.yml`) runs on every push to the main branch and pull requests. It usually:
1. Builds the project.
2. Runs API tests (e.g., `apify tests --env ci`) to verify functionality against a deployed or mocked environment.

### Release Process

To create a release (if configured, typically in `.github/workflows/release.yml`):
1. **Tag-based release**: Create and push a new tag: `git tag -a v1.0.0 -m "Version 1.0.0" && git push origin v1.0.0`
2. **Manual release**: Via GitHub Actions UI.

This process typically builds single file executables for various platforms and attaches them to a GitHub release.
