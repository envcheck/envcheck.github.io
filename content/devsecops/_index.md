+++
title = "☁️ DevSecOps"
weight = 3
sort_by = "weight"
template = "section.html"
+++

# DevSecOps Integrations

envcheck goes beyond basic `.env` linting to verify environment variable consistency across your entire infrastructure stack.

## Supported Integrations

| Integration | Command | What it checks |
|-------------|---------|----------------|
| **Kubernetes** | `envcheck k8s-sync` | SecretKeyRef/ConfigMapKeyRef vs `.env` |
| **Terraform** | `envcheck terraform` | `TF_VAR_*` variable usage |
| **Ansible** | `envcheck ansible` | `lookup('env', 'VAR')` calls |
| **GitHub Actions** | `envcheck actions` | `env:` blocks in workflows |
| **Helm** | `envcheck helm` | `SCREAMING_SNAKE_CASE` in `values.yaml` |
| **ArgoCD** | `envcheck argo` | `plugin.env` and `kustomize.commonEnv` |

## Why DevSecOps Integration?

Environment variables often drift across different parts of your infrastructure:

- **Development** → `.env`, `.env.local`
- **CI/CD** → GitHub Actions `env:` blocks
- **Kubernetes** → Secrets, ConfigMaps
- **IaC** → Terraform `TF_VAR_*`, Ansible lookups
- **GitOps** → ArgoCD, Helm values

envcheck ensures all these stay in sync, catching mismatches before they cause production issues.
