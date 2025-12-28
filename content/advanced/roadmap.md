+++
title = "Roadmap"
weight = 10
+++

# Roadmap

Track envcheck development progress.

## Phase 1: Core ✅

Complete CLI with essential features:

| Feature | Status |
|---------|--------|
| Linting (E001-E003, W001-W003) | ✅ |
| Compare (W004) | ✅ |
| Kubernetes Sync (W005-W006) | ✅ |
| Auto-fix | ✅ |
| Multiple output formats | ✅ |
| Shell completions | ✅ |

## Phase 2: DevSecOps Expansion 🚧

Extended infrastructure support:

| Feature | Status | Description |
|---------|--------|-------------|
| Terraform | 🔄 In Progress | Parse `.tf` files for `var.foo` and `TF_VAR_foo` |
| Ansible | 🔄 In Progress | Check `{{ lookup('env', 'FOO') }}` |
| GitOps | 🚀 Planned | ArgoCD Application manifest checking |

## Phase 2.5: Cloud Native 📋

Cloud platform integration:

| Feature | Status | Description |
|---------|--------|-------------|
| ArgoCD | 📋 Planned | Check `spec.source.plugin.env` |
| Helm | 📋 Planned | Lint `values.yaml` for env var conventions |
| GitHub Actions | 📋 Planned | Check `.github/workflows/*.yml` for `env:` |
| Flux | 📋 Planned | Kustomization substitution variables |

## Phase 3: Developer Experience 🔮

Enhanced UX features:

| Feature | Status | Description |
|---------|--------|-------------|
| Rayon Parallelism | 🔮 Planned | Process multiple files concurrently |
| Zero-Copy Parsing | 🔮 Planned | Use `Cow<str>` for performance |
| Criterion Benchmarks | 🔮 Planned | Performance vs dotenv-linter |
| Auto-commit/PR | 🔮 Planned | `--fix --commit` and `--fix --pr` |
| SARIF Reporter | 🔮 Planned | GitHub Security tab integration |
| HTML Report | 🔮 Planned | Standalone report file |
| Interactive TUI | 🔮 Planned | `envcheck tui` for conflict resolution |

## Distribution

| Platform | Status |
|----------|--------|
| Cargo | ✅ |
| Binary releases | ✅ |
| npm | ✅ |
| Homebrew | 📋 Planned |
| Docker | ✅ |

## Contributing

Want to help? Check out:

- [Contributing Guide](https://github.com/envcheck/envcheck/blob/main/CONTRIBUTING.md)
- [Good First Issues](https://github.com/envcheck/envcheck/issues?q=is%3Aissue+is%3Aopen+label%3A%22good+first+issue%22)
- [Roadmap Discussion](https://github.com/envcheck/envcheck/discussions/categories/roadmap)
