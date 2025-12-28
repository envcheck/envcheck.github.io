+++
title = "E002: Invalid Syntax"
weight = 2
+++

**Severity:** Error | **Category:** Linting

## Description

Detects lines that don't follow the `KEY=VALUE` format expected in `.env` files.

## Valid Syntax

| Format | Example | Valid |
|--------|---------|-------|
| Simple assignment | `KEY=value` | ✅ |
| With spaces in value | `KEY=value with spaces` | ✅ |
| Quoted value | `KEY="quoted value"` | ✅ |
| Empty value | `KEY=` | ⚠️ (W001) |
| Comment | `# This is a comment` | ✅ |
| Empty line | *(blank)* | ✅ |

## Examples

### ❌ Problematic

```bash
# Missing equals sign
DATABASE_URL postgres://localhost

# Invalid characters in key
123INVALID=value
MY-KEY=value  # hyphens not recommended

# Invalid escape sequence
KEY=value\nmore
```

### ✅ Fixed

```bash
DATABASE_URL=postgres://localhost

INVALID_123=value
MY_KEY=value  # Use underscores

KEY="value\nmore"  # Quote for escapes
```

## Key Naming Rules

Valid keys:
- Start with letter (A-Z, a-z) or underscore
- Followed by letters, numbers, or underscores
- Convention: `SCREAMING_SNAKE_CASE`

## Detection

```bash
$ envcheck lint .env

.env:8: E002: Invalid syntax: 'DATABASE_URL postgres://localhost'
```

## Configuration

```yaml
# .envcheckrc.yaml
rules:
  E002:
    severity: error
    allow_hyphens: false  # Set true to allow MY-KEY
```

## See Also

- [E001: Duplicate Key](duplicate-key.md)
- [W001: Empty Value](empty-value.md)
