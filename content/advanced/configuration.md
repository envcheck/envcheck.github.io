+++
title = "Configuration"
weight = 1
+++

# Configuration

Configure envcheck behavior using `.envcheckrc.yaml`.

## Configuration File

Place `.envcheckrc.yaml` in your project root:

```yaml
# envcheck configuration file

# Rule configuration
rules:
  # Ignore specific rules
  ignore:
    - W001  # Allow empty values
    - W003  # Don't care about sorting

  # Rule-specific settings
  E001:
    severity: error
  W002:
    severity: warning
    auto_fix: true

# Fix behavior
fix:
  sort_keys: true
  trim_whitespace: true
  normalize_spacing: true
  create_backup: true

# Ignore patterns
ignore:
  files:
    - ".env.local"
    - ".env.*.local"
  keys:
    - "*_SECRET"
    - "FEATURE_FLAG_*"

# Output settings
output:
  format: text
  color: auto
  verbose: false

# Kubernetes settings
kubernetes:
  ignore_namespaces:
    - kube-system
    - monitoring
  ignore_resources:
    - Event
    - Endpoint

# Doctor settings
doctor:
  run_all: false
  skip_slow: true
```

## JSON Schema

A JSON schema is available for IDE autocompletion:

```bash
# Download the schema
curl -o .envcheckrc.schema.json \
  https://raw.githubusercontent.com/envcheck/envcheck/main/.envcheckrc.schema.json
```

Add to your VSCode settings:

```json
{
  "yaml.schemas": {
    "./.envcheckrc.schema.json": [".envcheckrc.yaml"]
  }
}
```

## Command Line Options

Command-line options take precedence over config file:

```bash
# Override config file format
envcheck lint .env --format=json

# Override ignored rules
envcheck lint .env --ignore=W001,W002

# Override quiet setting
envcheck lint .env --verbose
```

## Environment Variables

```bash
# Disable color output
ENVCHECK_NO_COLOR=1 envcheck lint .env

# Set default format
ENVCHECK_FORMAT=json envcheck lint .env

# Disable backup creation
ENVCHECK_NO_BACKUP=1 envcheck fix .env
```

## Ignore File

Create `.envcheckignore` to ignore specific files:

```
# Ignore files
.env.local
.env.*.local
.env.test

# Ignore directories
tests/
fixtures/
```

## Project-Level vs Global Config

### Project level (recommended)
```
project/.envcheckrc.yaml
```

### Global level
```bash
~/.config/envcheck/config.yaml
```

### Hierarchy
1. Command-line flags (highest priority)
2. Project `.envcheckrc.yaml`
3. Global config
4. Built-in defaults (lowest priority)

## See Also

- [Rules Reference](../rules/)
- [Commands](../commands/)
