+++
title = "envcheck k8s-sync"
weight = 3
+++

# envcheck k8s-sync

Ensure Kubernetes manifests match your `.env` files.

## Usage

```bash
envcheck k8s-sync [OPTIONS] <MANIFESTS> --env <ENV_FILE>
```

## Arguments

- `<MANIFESTS>...` - Kubernetes YAML files (supports glob patterns)

## Options

| Option | Description |
|--------|-------------|
| `-e, --env <FILE>` | Reference `.env` file (required) |
| `-f, --format <FORMAT>` | Output format: `text`, `json`, `github` |
| `-q, --quiet` | Suppress output, use exit codes |
| `--ignore-namespaces <NS>` | Comma-separated namespaces to ignore |

## Supported Resources

- **Deployments** - `spec.template.spec.containers[*].env`
- **StatefulSets** - `spec.template.spec.containers[*].env`
- **DaemonSets** - `spec.template.spec.containers[*].env`
- **CronJobs** - `spec.jobTemplate.spec.template.spec.containers[*].env`
- **Secrets** - `stringData` and `data` keys
- **ConfigMaps** - `data` keys
- **Multi-document YAML** - Files with `---` separator

## Exit Codes

| Code | Meaning |
|------|---------|
| 0 | No mismatches |
| 1 | Missing keys found |
| 2 | Unused keys found (info) |

## Examples

### Check all manifests
```bash
envcheck k8s-sync k8s/base/*.yaml --env .env.example
```

Output:
```
W005: Key in K8s but missing in .env: API_ENDPOINT
W006: Key in .env but unused in K8s: LOCAL_DEV_KEY
```

### Recursive directory scan
```bash
envcheck k8s-sync k8s/**/*.yaml --env .env.example
```

### GitHub Actions format
```bash
envcheck k8s-sync k8s/base/*.yaml --env .env.example --format=github
```

Output:
```
::warning file=k8s/deployment.yaml,line=45,col=1::W005: Key in K8s but missing in .env: DATABASE_URL
```

### Ignore specific namespace
```bash
envcheck k8s-sync k8s/**/*.yaml --env .env.example --ignore-namespaces=kube-system
```

## Detected Issues

| Code | Rule | Severity | Description |
|------|------|----------|-------------|
| W005 | K8s Missing Env | Warning | Environment variable used in K8s but not defined in `.env` |
| W006 | Unused Env | Info | Key in `.env` but never referenced in K8s manifests |

## How It Works

The command parses YAML files and extracts:

1. **Container environment variables** from `env:` and `envFrom:`
2. **Secret references** from `valueFrom.secretKeyRef`
3. **ConfigMap references** from `valueFrom.configMapKeyRef`
4. **Secret data** from `Secret.stringData` and `Secret.data`
5. **ConfigMap data** from `ConfigMap.data`

These are then compared against keys in your `.env` file.

## Use Cases

### Pre-commit hook
```bash
#!/bin/sh
git diff --name-only --cached | grep -E '\.ya?ml$' | \
    xargs envcheck k8s-sync --env .env.example
```

### CI/CD validation
```yaml
- name: Validate K8s manifests
  run: envcheck k8s-sync k8s/**/*.yaml --env .env.example
```

### Helm chart validation (post-render)
```bash
helm template myapp ./chart > /tmp/rendered.yaml
envcheck k8s-sync /tmp/rendered.yaml --env .env.example
```

## See Also

- [envcheck compare](compare.md) - Compare multiple `.env` files
- [Integration Guide](../integration/k8s.md) - Kubernetes workflows
