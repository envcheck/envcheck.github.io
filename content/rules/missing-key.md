+++
title = "W004: Missing Key"
weight = 6
+++

**Severity:** Warning | **Category:** Comparison

## Description

Detects keys present in a reference file (e.g., `.env.example`) but missing in the target file.

## Examples

### .env.example (reference)
```bash
DATABASE_URL=postgres://localhost
API_KEY=your-key-here
REDIS_URL=redis://localhost
```

### .env.prod (target) - ❌ Missing keys
```bash
DATABASE_URL=postgres://prod-server
```

### Detection

```bash
$ envcheck compare .env.example .env.prod

.env.prod: W004: Missing key: API_KEY
.env.prod: W004: Missing key: REDIS_URL
```

## Use Cases

### Pre-deployment validation
```bash
#!/bin/bash
if ! envcheck compare .env.example .env.prod --quiet; then
    echo "ERROR: Production environment is missing required keys!"
    exit 1
fi
```

### CI/CD pipeline
```yaml
- name: Validate environments
  run: |
    envcheck compare .env.example .env.staging --format=github
    envcheck compare .env.example .env.prod --format=github
```

## Configuration

```yaml
# .envcheckrc.yaml
rules:
  W004:
    severity: warning
    ignore_keys:
      - OPTIONAL_FEATURE_*  # Don't warn about missing optional keys
```

## See Also

- [envcheck compare](../commands/compare.md)
- [W005: K8s Missing Env](k8s-missing-env.md)
