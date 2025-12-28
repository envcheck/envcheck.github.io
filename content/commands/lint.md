+++
title = "envcheck lint"
weight = 1
+++

# envcheck lint

Lint `.env` files for common issues.

## Usage

```bash
envcheck lint [OPTIONS] [FILES]...
```

## Arguments

- `<FILES>...` - One or more `.env` files to lint

## Options

| Option | Description |
|--------|-------------|
| `-f, --format <FORMAT>` | Output format: `text`, `json`, `github`, `sarif` |
| `-q, --quiet` | Suppress output, use exit codes |
| `-i, --ignore <RULES>` | Comma-separated list of rules to ignore |
| `--fix` | Auto-fix issues (alias for `envcheck fix`) |
| `-h, --help` | Print help |

## Exit Codes

| Code | Meaning |
|------|---------|
| 0 | No issues found |
| 1 | Errors found |
| 2 | Warnings only (no errors) |

## Examples

### Lint single file
```bash
envcheck lint .env
```

### Lint multiple files
```bash
envcheck lint .env .env.local .env.prod
```

### JSON output
```bash
envcheck lint .env --format=json
```

### GitHub Actions format
```bash
envcheck lint .env --format=github
```

Output:
```
::error file=.env,line=5,col=1::E001: Duplicate key: DATABASE_URL
::warning file=.env,line=12,col=1::W001: Empty value: DEBUG_MODE
```

### Ignore specific rules
```bash
envcheck lint .env --ignore=W001,W002
```

## Detected Issues

The `lint` command detects the following:

| Code | Rule | Severity | Description |
|------|------|----------|-------------|
| E001 | Duplicate Key | Error | Key defined multiple times |
| E002 | Invalid Syntax | Error | Line is not valid `KEY=VALUE` |
| W001 | Empty Value | Warning | Key has no value assigned |
| W002 | Trailing Whitespace | Warning | Line ends with whitespace |
| W003 | Unsorted Keys | Info | Keys not in alphabetical order |

## See Also

- [envcheck fix](fix.md) - Auto-fix linting issues
- [Rules Reference](../rules/) - Detailed rule documentation
