+++
title = "E001: Duplicate Key"
weight = 1
+++

**Severity:** Error | **Category:** Linting

## Description

Detects when the same key is defined multiple times in a single `.env` file.

## Why It's an Error

Duplicate keys create ambiguity:
1. Which value should the application use?
2. Different parsers may choose first or last occurrence
3. Can lead to unexpected behavior in production

## Examples

### ❌ Problematic

```bash
DATABASE_URL=postgres://localhost:5432/db
DATABASE_URL=postgres://prod-server:5432/db
```

### ✅ Fixed

```bash
DATABASE_URL=postgres://prod-server:5432/db
```

## Detection

```bash
$ envcheck lint .env

.env:5: E001: Duplicate key: DATABASE_URL (first defined at line 3)
```

## Auto-Fix

The `fix` command cannot automatically resolve duplicate keys (requires manual decision).

```bash
$ envcheck fix .env

⚠ E001: Duplicate keys must be resolved manually:
   - DATABASE_URL at lines 3, 5
```

## Configuration

```yaml
# .envcheckrc.yaml
rules:
  E001:
    severity: error  # Can downgrade to warning if needed
    auto_fix: false  # Always false for this rule
```

## See Also

- [E002: Invalid Syntax](invalid-syntax.md)
