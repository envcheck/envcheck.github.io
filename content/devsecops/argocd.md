+++
title = "ArgoCD"
weight = 6
+++

# ArgoCD Integration

The `argo` command checks ArgoCD Application manifests for environment variables in plugin configurations and kustomize settings.

## Usage

```bash
envcheck argo argocd/apps/ --env .env
envcheck argo argocd/*.yaml --env .env.prod
```

## What it checks

### Plugin Environment Variables
```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
spec:
  source:
    plugin:
      env:
        - name: DATABASE_URL  # ← Checked against .env
          value: "..."
        - name: API_KEY
          value: "..."
```

### Kustomize Common Environment
```yaml
spec:
  source:
    kustomize:
      commonEnv:
        - name: ENVIRONMENT
          value: production
      images:
        - name: myapp
```

## Example Output

```
$ envcheck argo argocd/apps/ --env .env

Checking argocd/apps/backend.yaml
  W010: plugin.env 'STRIPE_KEY' missing in .env

Checking argocd/apps/frontend.yaml
  W010: kustomize.commonEnv 'CDN_URL' missing in .env

Found 2 issues
```

## Multi-Source Applications

envcheck supports ArgoCD's multi-source applications (v2.6+):

```yaml
spec:
  sources:
    - repoURL: https://github.com/org/config
      path: base
    - repoURL: https://github.com/org/app
      path: overlays/prod
      kustomize:
        commonEnv:
          - name: APP_ENV
            value: production
```

## CI Integration

```yaml
- uses: envcheck/action-envcheck@v1
  with:
    command: argo
    args: "argocd/apps/"
    env_file: ".env.example"
```
