+++
title = "Quick Start"
weight = 2
+++

# Quick Start

Get started with envcheck in minutes.

## Basic Linting

Lint a single `.env` file:

```bash
envcheck lint .env
```

Lint multiple files:

```bash
envcheck lint .env .env.local .env.prod
```

## Auto-Fix Issues

Automatically fix formatting issues:

```bash
envcheck fix .env
```

This will:
- Sort keys alphabetically
- Remove trailing whitespace
- Normalize spacing around `=`

## Compare Environments

Check if your production environment has all required keys:

```bash
envcheck compare .env.example .env.prod
```

Output:
```
W004: Missing key in .env.prod: DATABASE_URL
W004: Missing key in .env.prod: API_KEY
```

## Kubernetes Sync

Ensure your Kubernetes manifests use the same environment variables:

```bash
envcheck k8s-sync k8s/base/*.yaml --env .env.example
```

This detects:
- Keys in K8s but missing in `.env` (W005)
- Keys in `.env` but unused in K8s (W006)

## Output Formats

### Colored Text (default)
```bash
envcheck lint .env
```

### JSON
```bash
envcheck lint .env --format=json | jq .
```

### GitHub Actions Annotations
```bash
envcheck lint .env --format=github
```

## CI/CD Integration

### GitHub Actions

```yaml
- name: Install envcheck
  run: cargo install envcheck

- name: Check .env files
  run: envcheck lint .env.example .env.prod --format=github

- name: K8s sync check
  run: envcheck k8s-sync k8s/**/*.yaml --env .env.example
```

### Pre-commit Hook

Create `.pre-commit-config.yaml`:

```yaml
repos:
  - repo: https://github.com/envcheck/envcheck
    rev: v0.1.0
    hooks:
      - id: envcheck
        args: ["lint", ".env.example", ".env"]

      - id: envcheck-k8s
        args: ["k8s-sync", "k8s/**/*.yaml", "--env", ".env.example"]
```

## Common Patterns

### Check all .env files in directory
```bash
envcheck doctor
```

### Quiet mode (exit code only)
```bash
envcheck lint .env --quiet
```

Exit codes: `0` = success, `1` = errors found, `2` = warnings only

### Ignore specific rules
```bash
envcheck lint .env --ignore=W001,W002
```
