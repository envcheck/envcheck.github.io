+++
title = "envcheck fix"
weight = 4
+++

# envcheck fix

Automatically fix formatting issues in `.env` files.

## Usage

```bash
envcheck fix [OPTIONS] [FILES]...
```

## Arguments

- `<FILES>...` - One or more `.env` files to fix

## Options

| Option | Description |
|--------|-------------|
| `--dry-run` | Show changes without modifying files |
| `--commit` | Auto-commit fixes to current branch |
| `--pr` | Create a PR with fixes (requires `gh` CLI) |
| `--no-sort` | Skip key sorting |
| `--no-trim` | Skip whitespace trimming |
| `-h, --help` | Print help |

## What Gets Fixed

| Issue | Fix |
|-------|-----|
| Trailing whitespace | Trim whitespace from end of lines |
| Unsorted keys | Sort keys alphabetically |
| Extra blank lines | Normalize to single blank line between sections |
| Spacing around `=` | Ensure `KEY=value` format (no spaces around `=`) |

## Examples

### Fix single file
```bash
envcheck fix .env
```

Output:
```
Fixed .env:
  - Sorted 15 keys
  - Trimmed 3 trailing whitespaces
```

### Dry run
```bash
envcheck fix .env --dry-run
```

### Fix multiple files
```bash
envcheck fix .env .env.local .env.prod
```

### Auto-commit fixes
```bash
envcheck fix .env --commit
```

Creates a commit:
```
[envcheck] Auto-fix .env

- Sorted keys alphabetically
- Trimmed trailing whitespace
```

### Create PR
```bash
envcheck fix .env --pr
```

Creates a branch and PR:
```
Branch: envcheck/auto-fix
PR: "envcheck: Auto-fix .env formatting"
```

## Safety

The `fix` command:
- ✅ Preserves comments and empty lines
- ✅ Preserves key values (only fixes formatting)
- ✅ Creates backups before modifying
- ⚠️ Does NOT fix duplicate keys (requires manual resolution)
- ⚠️ Does NOT fix empty values (may be intentional)

## Backups

Before fixing, a backup is created:
```
.env          →  .env.backup
.env.local    →  .env.local.backup
```

Remove backups after confirming fixes:
```bash
rm .env.backup
```

## Git Integration

### Pre-commit auto-fix
```yaml
# .pre-commit-config.yaml
repos:
  - repo: https://github.com/envcheck/envcheck
    rev: v0.1.0
    hooks:
      - id: envcheck-fix
        args: ["--commit"]
```

### CI/CD with PR
```yaml
- name: Fix and create PR
  run: envcheck fix .env --pr
  if: failure()
```

## Configuration

Create `.envcheckrc.yaml` for project-specific settings:

```yaml
fix:
  sort_keys: true
  trim_whitespace: true
  normalize_spacing: true
  create_backup: true
```

## See Also

- [envcheck lint](lint.md) - Detect issues without fixing
- [Configuration](../advanced/configuration.md) - Project-level config
