+++
title = "Helm"
weight = 5
+++

# Helm Integration

The `helm` command scans Helm `values.yaml` files for `SCREAMING_SNAKE_CASE` keys that look like environment variables and verifies they exist in your `.env`.

## Usage

```bash
envcheck helm charts/myapp --env .env
envcheck helm charts/ --env .env.prod
```

## What it checks

### Environment-like keys in values.yaml
```yaml
# values.yaml
image:
  repository: myapp
  tag: v1.0.0

env:
  DATABASE_URL: ""  # ← Checked
  API_KEY: ""       # ← Checked
  REDIS_HOST: ""    # ← Checked
```

### envFrom references
```yaml
envFrom:
  - secretRef:
      name: app-secrets  # Keys will be checked
  - configMapRef:
      name: app-config
```

## Example Output

```
$ envcheck helm charts/myapp --env .env

Scanning charts/myapp/values.yaml
  W011: env.STRIPE_SECRET_KEY not found in .env
  W011: env.SENDGRID_API_KEY not found in .env

Found 2 issues
```

## CI Integration

```yaml
- uses: envcheck/action-envcheck@v1
  with:
    command: helm
    args: "charts/myapp"
    env_file: ".env.example"
```
