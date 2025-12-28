+++
title = "✅ Rules"
weight = 3
sort_by = "weight"
template = "section.html"
+++

# Lint Rules Reference

Complete documentation of all envcheck lint rules.

## Rule Severity

| Severity | Description | Exit Code |
|----------|-------------|-----------|
| Error | Must be fixed before deployment | 1 |
| Warning | Should be reviewed | 2 |
| Info | Informational only | 0 |

## Rule Summary

| ID | Rule | Severity | Category |
|----|------|----------|----------|
| E001 | Duplicate Key | Error | Linting |
| E002 | Invalid Syntax | Error | Linting |
| W001 | Empty Value | Warning | Linting |
| W002 | Trailing Whitespace | Warning | Linting |
| W003 | Unsorted Keys | Info | Linting |
| W004 | Missing Key | Warning | Comparison |
| W005 | K8s Missing Env | Warning | Kubernetes |
| W006 | Unused Env | Info | Kubernetes |
| W007 | Terraform Variable | Warning | Terraform (Phase 2) |
| W008 | TF_VAR Missing | Warning | Terraform (Phase 2) |
| W009 | Ansible Lookup | Warning | Ansible (Phase 2) |
| W010 | ArgoCD Plugin Env | Warning | ArgoCD (Planned) |
| W011 | Helm Convention | Info | Helm (Planned) |
| W012 | GitHub Actions Env | Warning | GitHub Actions (Planned) |

## Ignoring Rules

### Command line
```bash
envcheck lint .env --ignore=W001,W002
```

### Config file
```yaml
# .envcheckrc.yaml
rules:
  ignore:
    - W001  # Allow empty values
    - W003  # Don't care about sorting
```

### Inline comments (planned)
```bash
# envcheck:ignore=W001
API_KEY=
```
