+++
title = "W005: K8s Missing Env"
weight = 7
+++

**Severity:** Warning | **Category:** Kubernetes

## Description

Environment variable is referenced in a Kubernetes manifest but not defined in the `.env` file.

## Examples

### Kubernetes manifest
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp
spec:
  template:
    spec:
      containers:
        - name: app
          env:
            - name: API_ENDPOINT
              value: "https://api.example.com"
            - name: DATABASE_URL
              valueFrom:
                secretKeyRef:
                  name: db-secret
                  key: url
```

### .env.example - ❌ Missing DATABASE_URL
```bash
API_ENDPOINT=https://api.example.com
# DATABASE_URL is missing!
```

### Detection

```bash
$ envcheck k8s-sync k8s/**/*.yaml --env .env.example

k8s/deployment.yaml: W005: Key in K8s but missing in .env: DATABASE_URL
```

## Supported Kubernetes Resources

- Deployments
- StatefulSets
- DaemonSets
- CronJobs/Jobs
- Secrets
- ConfigMaps

## Configuration

```yaml
# .envcheckrc.yaml
rules:
  W005:
    severity: warning
    ignore_namespaces:
      - kube-system
      - monitoring
```

## See Also

- [envcheck k8s-sync](../commands/k8s-sync.md)
- [W006: Unused Env](unused-env.md)
