+++
title = "W006: Unused Env"
weight = 8
+++

**Severity:** Info | **Category:** Kubernetes

## Description

Environment variable is defined in `.env` but never referenced in Kubernetes manifests.

## Examples

### .env.example
```bash
DATABASE_URL=postgres://localhost
API_KEY=secret
LOCAL_DEV_MODE=true  # ← Only used locally
```

### Kubernetes manifest
```yaml
env:
  - name: DATABASE_URL
    valueFrom:
      secretKeyRef:
        name: db-secret
        key: url
  - name: API_KEY
    valueFrom:
      secretKeyRef:
        name: api-secret
        key: key
# LOCAL_DEV_MODE is not referenced
```

### Detection

```bash
$ envcheck k8s-sync k8s/**/*.yaml --env .env.example

.env.example: W006: Key in .env but unused in K8s: LOCAL_DEV_MODE
```

## When This Is OK

Unused variables are sometimes intentional:
- Local development settings
- Feature flags for specific environments
- Documentation purposes

## Configuration

```yaml
# .envcheckrc.yaml
rules:
  W006:
    severity: info  # Downgrade from warning
    ignore_patterns:
      - "*_LOCAL"
      - "DEV_*"
      - "TEST_*"
```

## See Also

- [envcheck k8s-sync](../commands/k8s-sync.md)
- [W005: K8s Missing Env](k8s-missing-env.md)
