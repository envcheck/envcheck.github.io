+++
title = "envcheck doctor"
weight = 5
+++

# envcheck doctor

Comprehensive health check for your project's environment configuration.

## Usage

```bash
envcheck doctor [OPTIONS]
```

## Options

| Option | Description |
|--------|-------------|
| `-d, --dir <DIR>` | Project directory (default: current) |
| `-f, --format <FORMAT>` | Output format: `text`, `json`, `html` |
| `--all` | Include all checks (including slow ones) |
| `--no-k8s` | Skip Kubernetes checks |
| `--no-terraform` | Skip Terraform checks (Phase 2) |
| `--no-ansible` | Skip Ansible checks (Phase 2) |

## What It Checks

The `doctor` command runs a comprehensive suite of checks:

### 1. .env Files
```bash
✓ Found .env.example
✓ Found .env.local
✓ Found .env.prod
✓ Linted 3 files (0 errors, 2 warnings)
```

### 2. Key Consistency
```bash
Comparing .env.example ↔ .env.local
  Missing in .env.local: API_KEY (W004)

Comparing .env.example ↔ .env.prod
  Missing in .env.prod: DEBUG_MODE (W004)
  Extra in .env.prod: PRODUCTION_SECRET (I001)
```

### 3. Kubernetes Sync
```bash
Checking k8s/**/*.yaml
  W005: Key in K8s but missing in .env: DATABASE_URL
  W006: Key in .env but unused in K8s: LOCAL_ONLY
```

### 4. Terraform (Phase 2)
```bash
Checking terraform/**/*.tf
  W007: var.database_url not in .env
  W008: TF_VAR_api_key not in .env
```

### 5. Ansible (Phase 2)
```bash
Checking ansible/**/*.yml
  W009: lookup('env', 'APP_SECRET') not in .env
```

### 6. GitOps (Phase 2.5)
```bash
Checking argocd/**/*.yaml
  W010: ArgoCD plugin env not in .env
```

## Examples

### Run in current directory
```bash
envcheck doctor
```

### Run in specific directory
```bash
envcheck doctor --dir /path/to/project
```

### HTML report
```bash
envcheck doctor --format=html > report.html
```

### Skip Kubernetes checks
```bash
envcheck doctor --no-k8s
```

### All checks (including slow ones)
```bash
envcheck doctor --all
```

## Exit Codes

| Code | Meaning |
|------|---------|
| 0 | All checks passed |
| 1 | Errors found |
| 2 | Warnings found |
| 3 | Some checks skipped |

## Output Formats

### Text (default)
```
=== envcheck doctor ===

[1/5] .env files
  ✓ .env.example exists
  ✓ .env.local exists
  ✓ .env.prod exists

[2/5] Linting
  ✓ 3 files linted (1 warning)

[3/5] Key consistency
  ⚠ .env.local missing: API_KEY

[4/5] Kubernetes sync
  ⚠ K8s uses: DATABASE_URL (not in .env)

[5/5] Terraform
  ⊘ Skipped (--no-terraform)

Summary: 1 warning found
```

### JSON
```json
{
  "summary": {"errors": 0, "warnings": 2, "skipped": 1},
  "checks": [...]
}
```

### HTML
Generates a standalone HTML report with:
- Summary dashboard
- Expandable sections
- Copy-pasteable fix suggestions

## CI/CD Usage

### GitHub Actions
```yaml
- name: Run envcheck doctor
  run: envcheck doctor --format=github

- name: Upload HTML report
  uses: actions/upload-artifact@v4
  with:
    name: envcheck-report
    path: report.html
  if: always()
```

### Pre-commit
```yaml
repos:
  - repo: https://github.com/envcheck/envcheck
    rev: v0.1.0
    hooks:
      - id: envcheck-doctor
        args: ["--all"]
```

## Project Discovery

The `doctor` command automatically finds files:

| Pattern | Description |
|---------|-------------|
| `.env*` | All .env files in project root |
| `k8s/**/*.yaml` | Kubernetes manifests |
| `k8s/**/*.yml` | Kubernetes manifests (alt extension) |
| `terraform/**/*.tf` | Terraform files |
| `ansible/**/*.yml` | Ansible playbooks |
| `.github/workflows/*.yml` | GitHub Actions |
| `argocd/**/*.yaml` | ArgoCD manifests |

## See Also

- [envcheck lint](lint.md) - Individual file linting
- [envcheck compare](compare.md) - File comparison
- [envcheck k8s-sync](k8s-sync.md) - Kubernetes sync
