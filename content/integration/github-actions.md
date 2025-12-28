+++
title = "GitHub Actions"
weight = 1
+++

# GitHub Actions Integration

Use envcheck in your GitHub Actions workflows for automated environment validation.

## Basic Setup

```yaml
name: Environment Validation

on:
  push:
    branches: [main]
  pull_request:

jobs:
  envcheck:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Install envcheck
        run: cargo install envcheck

      - name: Lint .env files
        run: envcheck lint .env.example .env.prod

      - name: Compare environments
        run: envcheck compare .env.example .env.prod
```

## GitHub Actions Annotations

Use `--format=github` to get inline annotations in your workflow runs:

```yaml
- name: Check with annotations
  run: envcheck lint .env --format=github
```

This produces:
```
::error file=.env,line=5,col=1::E001: Duplicate key: DATABASE_URL
::warning file=.env,line=12,col=1::W001: Empty value: API_KEY
```

## Complete Workflow

```yaml
name: envcheck

on:
  push:
    paths:
      - '.env*'
      - 'k8s/**'
      - '.github/workflows/envcheck.yml'
  pull_request:

jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Install envcheck
        run: cargo install envcheck

      - name: Lint .env files
        run: |
          envcheck lint .env.example --format=github

      - name: Compare environments
        run: |
          envcheck compare .env.example .env.prod --format=github

      - name: Check Kubernetes sync
        run: |
          envcheck k8s-sync k8s/**/*.yaml --env .env.example --format=github

      - name: Run doctor
        run: |
          envcheck doctor --format=json > envcheck-report.json

      - name: Upload report
        uses: actions/upload-artifact@v4
        with:
          name: envcheck-report
          path: envcheck-report.json
```

## PR Comment Reporter

Post envcheck results as a PR comment:

```yaml
- name: Run envcheck
  run: |
    envcheck doctor --format=json > result.json

- name: Comment on PR
  uses: actions/github-script@v7
  if: github.event_name == 'pull_request'
  with:
    script: |
      const fs = require('fs');
      const result = JSON.parse(fs.readFileSync('result.json', 'utf8'));

      const comment = `
      ## envcheck Report

      | Status | Count |
      |--------|-------|
      | ✅ Passed | ${result.summary.passed} |
      | ⚠️ Warnings | ${result.summary.warnings} |
      | ❌ Errors | ${result.summary.errors} |

      Details:
      \`\`\`
      ${result.message}
      \`\`\`
      `;

      github.rest.issues.createComment({
        issue_number: context.issue.number,
        owner: context.repo.owner,
        repo: context.repo.repo,
        body: comment
      });
```

## SARIF Reporter

For GitHub Security tab integration (Phase 3):

```yaml
- name: Run envcheck with SARIF
  run: envcheck lint .env --format=sarif > results.sarif

- name: Upload SARIF
  uses: github/codeql-action/upload-sarif@v3
  with:
    sarif_file: results.sarif
```

## Composite Action

Create `.github/actions/envcheck/action.yml`:

```yaml
name: 'envcheck'
description: 'Run envcheck environment validation'
inputs:
  format:
    description: 'Output format'
    required: false
    default: 'github'
  env-file:
    description: 'Environment file to check'
    required: false
    default: '.env.example'
  k8s-dir:
    description: 'Kubernetes manifest directory'
    required: false
    default: 'k8s'

runs:
  using: 'composite'
  steps:
    - name: Install envcheck
      shell: bash
      run: cargo install envcheck

    - name: Lint env files
      shell: bash
      run: |
        envcheck lint ${{ inputs.env-file }} --format=${{ inputs.format }}

    - name: Check K8s sync
      shell: bash
      run: |
        envcheck k8s-sync ${{ inputs.k8s-dir }}/**/*.yaml \
          --env ${{ inputs.env-file }} \
          --format=${{ inputs.format }}
```

Usage in workflows:

```yaml
- uses: ./.github/actions/envcheck
  with:
    format: github
    env-file: .env.prod
```

## See Also

- [Pre-commit Hooks](pre-commit.md)
- [CI/CD Pipelines](ci-cd.md)
