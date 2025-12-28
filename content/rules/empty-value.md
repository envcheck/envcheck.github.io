+++
title = "W001: Empty Value"
weight = 3
+++

**Severity:** Warning | **Category:** Linting

## Description

Detects keys that have no assigned value (key ends with `=` with nothing after it).

## Examples

### ⚠️ Warning

```bash
API_KEY=
DEBUG_MODE=
```

### ✅ Fixed

```bash
# Provide a default value
API_KEY=development-key

# Or use empty quotes to be explicit
API_KEY=""

# Or comment if not needed
# API_KEY=  # TODO: Set in production
```

## When Empty Values Are OK

Empty values may be intentional:
- Feature flags that are disabled
- Optional configuration
- Values populated by the application

## Detection

```bash
$ envcheck lint .env

.env:12: W001: Empty value: API_KEY
```

## Auto-Fix

The `fix` command does not auto-fix empty values (requires intent decision).

## Configuration

```yaml
# .envcheckrc.yaml
rules:
  W001:
    severity: warning  # Can set to info or ignore
    allow_for:
      - FEATURE_FLAG_*  # Allow empty for certain patterns
```

## Ignore Specific Keys

```bash
# Ignore for all feature flags
envcheck lint .env --ignore=W001

# Or use config to allow specific patterns
```

## See Also

- [E002: Invalid Syntax](invalid-syntax.md)
- [W002: Trailing Whitespace](trailing-whitespace.md)
