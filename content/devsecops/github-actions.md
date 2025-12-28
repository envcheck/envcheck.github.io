+++
title = "GitHub Actions"
weight = 4
+++

# GitHub Actions Integration

The `actions` command scans GitHub Actions workflow files for `env:` blocks and verifies those variables exist in your `.env` files.

## Usage

```bash
envcheck actions .github/workflows --env .env
envcheck actions . --env .env.example
```

## What it checks

### Workflow-level env
```yaml
name: CI
on: push

env:
  NODE_ENV: production  # ← Checked
  API_URL: https://api.example.com

jobs:
  build:
    runs-on: ubuntu-latest
```

### Job-level env
```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    env:
      DEPLOY_TOKEN: ${{ secrets.DEPLOY_TOKEN }}  # ← Checked
```

### Step-level env
```yaml
steps:
  - name: Run tests
    env:
      TEST_DATABASE_URL: postgres://localhost/test  # ← Checked
    run: npm test
```

## Example Output

```
$ envcheck actions .github/workflows --env .env

Scanning .github/workflows/ci.yml
  W012: env 'CODECOV_TOKEN' missing in .env

Scanning .github/workflows/deploy.yml
  W012: env 'AWS_ACCESS_KEY_ID' missing in .env
  W012: env 'AWS_SECRET_ACCESS_KEY' missing in .env

Found 3 issues
```

## CI Integration

```yaml
- uses: envcheck/action-envcheck@v1
  with:
    command: actions
    args: ".github/workflows"
    env_file: ".env.example"
```
