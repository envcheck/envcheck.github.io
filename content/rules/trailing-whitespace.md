+++
title = "W002: Trailing Whitespace"
weight = 4
+++

**Severity:** Warning | **Category:** Linting

## Description

Detects lines that end with whitespace characters (spaces or tabs).

## Why It Matters

- Git diffs become noisy
- Can cause issues in some parsers
- Generally considered poor style

## Examples

### ⚠️ Warning

```bash
DATABASE_URL=postgres://localhost␣
API_KEY=secret␣␣␣
```

### ✅ Fixed

```bash
DATABASE_URL=postgres://localhost
API_KEY=secret
```

## Detection

```bash
$ envcheck lint .env

.env:5: W002: Trailing whitespace on line 5
```

## Auto-Fix

The `fix` command automatically removes trailing whitespace:

```bash
$ envcheck fix .env

Fixed .env:
  - Trimmed trailing whitespace from 3 lines
```

## Configuration

```yaml
# .envcheckrc.yaml
rules:
  W002:
    severity: warning  # Can set to info
    auto_fix: true
```

## Editor Configuration

Prevent trailing whitespace in your editor:

### VSCode
```json
{
  "files.trimTrailingWhitespace": true,
  "files.insertFinalNewline": true
}
```

### Vim
```vim
set listchars+=trail:·
set list
autocmd BufWritePre * :%s/\s\+$//e
```

## See Also

- [W001: Empty Value](empty-value.md)
- [W003: Unsorted Keys](unsorted-keys.md)
