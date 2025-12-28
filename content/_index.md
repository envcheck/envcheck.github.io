+++
title = "envcheck Documentation"
sort_by = "weight"
template = "section.html"
weight = 0

[extra]
hero_title = "envcheck"
hero_subtitle = "Fast .env linter with DevSecOps superpowers. Written in Rust 🦀"
hero_buttons = [
    { text = "Get Started", link = "/installation", class = "btn-primary" },
    { text = "GitHub", link = "https://github.com/envcheck/envcheck", class = "btn-secondary" }
]
+++

## What is envcheck?

**envcheck** is a fast, modern Rust CLI for linting `.env` files and ensuring environment synchronization across your entire DevSecOps stack.

## Why envcheck?

| Feature | envcheck 🦀 | dotenv-linter |
|---------|-------------|---------------|
| **Linting** | ✅ | ✅ |
| **Compare** | ✅ | ✅ |
| **K8s Sync** | ✅ | ❌ |
| **Terraform** | ✅ | ❌ |
| **Ansible** | ✅ | ❌ |
| **Helm** | ✅ | ❌ |
| **ArgoCD** | ✅ | ❌ |
| **GitHub Actions Check** | ✅ | ❌ |
| **TUI Mode** | ✅ | ❌ |
| **SARIF Output** | ✅ | ❌ |
| **Config Files** | ✅ | ❌ |
| **Shell Completions** | ✅ | ❌ |
| **Auto-Fix + Commit/PR** | ✅ | ✅ (fix only) |

## DevSecOps Integrations

| Integration | Command | What it checks |
|-------------|---------|----------------|
| **Kubernetes** | `envcheck k8s-sync` | SecretKeyRef/ConfigMapKeyRef vs `.env` |
| **Terraform** | `envcheck terraform` | `TF_VAR_*` variable usage |
| **Ansible** | `envcheck ansible` | `lookup('env', 'VAR')` calls |
| **GitHub Actions** | `envcheck actions` | `env:` blocks in workflows |
| **Helm** | `envcheck helm` | `SCREAMING_SNAKE_CASE` in `values.yaml` |
| **ArgoCD** | `envcheck argo` | `plugin.env` and `kustomize.commonEnv` |

## License

MIT License - See [GitHub](https://github.com/envcheck/envcheck) for details.
