+++
title = "Kubernetes"
weight = 1
+++

# Kubernetes Sync

The `k8s-sync` command verifies that your Kubernetes manifests reference environment variables that exist in your `.env` files, and vice versa.

## Usage

```bash
envcheck k8s-sync k8s/*.yaml --env .env.example
```

## What it checks

### SecretKeyRef
```yaml
env:
  - name: DATABASE_URL
    valueFrom:
      secretKeyRef:
        name: app-secrets
        key: DATABASE_URL  # ← Must exist in .env
```

### ConfigMapKeyRef
```yaml
env:
  - name: LOG_LEVEL
    valueFrom:
      configMapKeyRef:
        name: app-config
        key: LOG_LEVEL  # ← Must exist in .env
```

### Direct env values
```yaml
env:
  - name: NODE_ENV
    value: "production"  # ← Compared against .env
```

## Example Output

```
$ envcheck k8s-sync k8s/*.yaml --env .env.example

Checking k8s/deployment.yaml
  W005: DATABASE_URL referenced in Secret but missing in .env.example
  W006: LEGACY_API_KEY in .env.example but unused in K8s manifests

Found 2 issues
```

## Supported Resource Types

- Deployments
- StatefulSets
- DaemonSets
- Jobs / CronJobs
- Secrets
- ConfigMaps

## CI Integration

```yaml
- uses: envcheck/action-envcheck@v1
  with:
    command: k8s-sync
    args: "k8s/*.yaml"
    env_file: ".env.example"
```
