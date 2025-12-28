+++
title = "Output Formats"
weight = 2
+++

# Output Formats

envcheck supports multiple output formats for different use cases.

## Available Formats

| Format | Use Case |
|--------|----------|
| `text` | Human-readable terminal output (default) |
| `json` | Machine-readable, for parsing/scripts |
| `github` | GitHub Actions annotations |
| `sarif` | GitHub Security tab (Phase 3) |
| `html` | Standalone report (doctor command) |

## Text Format

Default human-readable output with colors:

```bash
envcheck lint .env
```

Output:
```
.env:5: E001: Duplicate key: DATABASE_URL
.env:12: W001: Empty value: API_KEY

Summary: 1 error, 1 warning
```

### Color control

```bash
# Auto-detect (default)
envcheck lint .env --color=auto

# Always color
envcheck lint .env --color=always

# Never color
envcheck lint .env --color=never
```

## JSON Format

Machine-readable output:

```bash
envcheck lint .env --format=json
```

Output:
```json
{
  "success": false,
  "errors": [
    {
      "code": "E001",
      "rule": "Duplicate Key",
      "file": ".env",
      "line": 5,
      "column": 1,
      "message": "Duplicate key: DATABASE_URL",
      "severity": "error"
    }
  ],
  "warnings": [
    {
      "code": "W001",
      "rule": "Empty Value",
      "file": ".env",
      "line": 12,
      "column": 1,
      "message": "Empty value: API_KEY",
      "severity": "warning"
    }
  ],
  "summary": {
    "errors": 1,
    "warnings": 1,
    "files_checked": 1
  }
}
```

### Parsing with jq

```bash
envcheck lint .env --format=json | jq '.errors[] | .code'
# Output: "E001"

envcheck lint .env --format=json | jq '.summary'
# Output: {"errors": 1, "warnings": 1, "files_checked": 1}
```

## GitHub Format

GitHub Actions workflow annotations:

```bash
envcheck lint .env --format=github
```

Output:
```
::error file=.env,line=5,col=1::E001: Duplicate key: DATABASE_URL
::warning file=.env,line=12,col=1::W001: Empty value: API_KEY
```

### Annotation Format

```
::LEVEL file=FILE,line=LINE,col=COL::MESSAGE
```

## SARIF Format (Phase 3)

Static Analysis Results Interchange Format for GitHub Security tab:

```bash
envcheck lint .env --format=sarif > results.sarif
```

## HTML Format

Standalone HTML report from `doctor` command:

```bash
envcheck doctor --format=html > report.html
```

Features:
- Summary dashboard
- Expandable sections
- Filter by severity
- Copy-paste fix suggestions

### HTML Report Example

```html
<!DOCTYPE html>
<html>
<head>
  <title>envcheck Report</title>
  <style>
    .error { color: red; }
    .warning { color: orange; }
    .info { color: blue; }
  </style>
</head>
<body>
  <h1>envcheck Report</h1>
  <div class="summary">
    <span class="error">1 error</span>
    <span class="warning">2 warnings</span>
  </div>
  ...
</body>
</html>
```

## Quiet Mode

Suppress output, use exit codes only:

```bash
envcheck lint .env --quiet
echo $?
# Output: 1 (errors found)
```

## Verbose Mode

Detailed output including file info and runtime:

```bash
envcheck lint .env --verbose
```

Output:
```
Checking: .env
Parsed: 25 entries in 0.5ms
Found: 2 issues
Runtime: 2.1ms
```

## See Also

- [GitHub Actions Integration](../integration/github-actions.md)
- [Commands](../commands/)
