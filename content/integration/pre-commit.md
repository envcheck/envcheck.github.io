+++
title = "Pre-commit Hooks"
weight = 2
+++

# Pre-commit Integration

Use envcheck with pre-commit hooks to catch issues before you commit.

## Basic Setup

Install pre-commit:
```bash
pip install pre-commit
```

Create `.pre-commit-config.yaml`:

```yaml
repos:
  - repo: https://github.com/envcheck/envcheck
    rev: v0.1.0
    hooks:
      - id: envcheck
        files: '^\.env.*$'

      - id: envcheck-fix
        files: '^\.env.*$'

      - id: envcheck-k8s
        files: '^k8s/.*\.ya?ml$'
```

## Hook IDs

| Hook ID | Description |
|---------|-------------|
| `envcheck` | Lint `.env` files |
| `envcheck-fix` | Auto-fix `.env` files |
| `envcheck-k8s` | Check Kubernetes sync |
| `envcheck-doctor` | Run full doctor check |

## Configuration Examples

### Lint all .env files
```yaml
repos:
  - repo: https://github.com/envcheck/envcheck
    rev: v0.1.0
    hooks:
      - id: envcheck
        args: ['lint', '.env.example', '.env']
```

### Auto-fix on commit
```yaml
repos:
  - repo: https://github.com/envcheck/envcheck
    rev: v0.1.0
    hooks:
      - id: envcheck-fix
        args: ['--commit']
```

### Kubernetes validation
```yaml
repos:
  - repo: https://github.com/envcheck/envcheck
    rev: v0.1.0
    hooks:
      - id: envcheck-k8s
        args: ['k8s/**/*.yaml', '--env', '.env.example']
        files: '^k8s/.*\.ya?ml$'
```

### Multiple environments
```yaml
repos:
  - repo: https://github.com/envcheck/envcheck
    rev: v0.1.0
    hooks:
      - id: envcheck
        name: envcheck (staging)
        args: ['lint', '.env.staging', '--format=github']
        files: '^\.env\.staging$'

      - id: envcheck
        name: envcheck (production)
        args: ['lint', '.env.prod', '--format=github']
        files: '^\.env\.prod$'
```

### Complete .pre-commit-config.yaml
```yaml
repos:
  # envcheck hooks
  - repo: https://github.com/envcheck/envcheck
    rev: v0.1.0
    hooks:
      - id: envcheck
        name: envcheck lint
        args: ['lint', '.env.example']
        files: '^\.env.*$'

      - id: envcheck-fix
        name: envcheck fix
        args: ['.env.example']

      - id: envcheck-k8s
        name: envcheck k8s sync
        args: ['k8s/**/*.yaml', '--env', '.env.example']
        files: '^k8s/.*\.ya?ml$'

  # Other hooks...
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v4.5.0
    hooks:
      - id: trailing-whitespace
      - id: end-of-file-fixer
      - id: check-yaml
```

## Install Hooks

```bash
# Install hooks
pre-commit install

# Install hooks for all git hooks (including post-commit)
pre-commit install --hook-type pre-commit \
                  --hook-type pre-push \
                  --hook-type commit-msg

# Run on all files
pre-commit run --all-files
```

## Stage-Specific Hooks

### Pre-commit (before committing)
```yaml
- id: envcheck
  args: ['lint', '.env.example']
  stages: [pre-commit]
```

### Pre-push (before pushing)
```yaml
- id: envcheck-doctor
  args: ['--all']
  stages: [pre-push]
```

## Skip Hooks

Skip envcheck for a specific commit:
```bash
git commit -m "WIP" --no-verify
```

Or skip only envcheck:
```bash
SKIP=envcheck git commit -m "WIP"
```

## See Also

- [GitHub Actions](github-actions.md)
- [CI/CD Pipelines](ci-cd.md)
