+++
title = "Terraform"
weight = 2
+++

# Terraform Integration

The `terraform` command detects `TF_VAR_*` environment variables used in your Terraform configurations and verifies they exist in your `.env` files.

## Usage

```bash
envcheck terraform . --env .env
envcheck terraform infra/ --env .env.prod
```

## How it works

Terraform allows setting variables via environment variables with the `TF_VAR_` prefix:

```hcl
# variables.tf
variable "database_url" {
  type = string
}
```

```bash
# .env
TF_VAR_database_url=postgres://localhost:5432/db
```

envcheck scans your `.tf` files for variable declarations and ensures matching `TF_VAR_*` entries exist in your `.env`.

## Example Output

```
$ envcheck terraform infra/ --env .env

Scanning infra/variables.tf
  W007: Variable 'api_key' declared but TF_VAR_api_key missing in .env
  W008: TF_VAR_legacy_token in .env but no matching variable declaration

Found 2 issues
```

## CI Integration

```yaml
- uses: envcheck/action-envcheck@v1
  with:
    command: terraform
    args: "infra/"
    env_file: ".env"
```
