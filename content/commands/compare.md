+++
title = "envcheck compare"
weight = 2
+++

# envcheck compare

Compare multiple `.env` files to find missing or extra keys.

## Usage

```bash
envcheck compare [OPTIONS] <BASE> <TARGET> [TARGETS...]
```

## Arguments

- `<BASE>` - Reference file (e.g., `.env.example`)
- `<TARGET>` - File(s) to compare against base

## Options

| Option | Description |
|--------|-------------|
| `-f, --format <FORMAT>` | Output format: `text`, `json`, `github` |
| `-q, --quiet` | Suppress output, use exit codes |
| `--ignore <KEYS>` | Comma-separated keys to ignore |

## Exit Codes

| Code | Meaning |
|------|---------|
| 0 | All keys present |
| 1 | Missing keys found |
| 2 | Extra keys found (info) |

## Examples

### Compare example with production
```bash
envcheck compare .env.example .env.prod
```

Output:
```
W004: Missing key in .env.prod: DATABASE_URL
W004: Missing key in .env.prod: REDIS_URL
I001: Extra key in .env.prod: PRODUCTION_SECRET
```

### Compare multiple environments
```bash
envcheck compare .env.example .env.staging .env.prod
```

### JSON output
```bash
envcheck compare .env.example .env.prod --format=json
```

### Ignore specific keys
```bash
envcheck compare .env.example .env.prod --ignore=FEATURE_FLAG_X
```

## Use Cases

### Pre-deployment validation
```bash
#!/bin/bash
if envcheck compare .env.example .env.prod --quiet; then
    echo "All required keys present"
    kubectl apply -f deployment.yaml
else
    echo "Missing keys in production!"
    exit 1
fi
```

### CI/CD pipeline
```yaml
- name: Validate production environment
  run: envcheck compare .env.example .env.prod --format=github
```

## Detected Issues

| Code | Rule | Severity | Description |
|------|------|----------|-------------|
| W004 | Missing Key | Warning | Key present in base but missing in target |
| I001 | Extra Key | Info | Key present in target but not in base |

## See Also

- [envcheck k8s-sync](k8s-sync.md) - Compare with Kubernetes manifests
- [envcheck lint](lint.md) - Lint individual files
