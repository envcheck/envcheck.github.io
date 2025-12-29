+++
title = "🚀 Commands"
weight = 2
sort_by = "weight"
template = "section.html"
+++

# Powerful Tools at Your Fingertips

`envcheck` provides a suite of commands designed to handle every aspect of environment variable management, from local development to production validation.

## Core Commands

*   **[Lint](/commands/lint)**: The workhorse. Scans your `.env` files for syntax errors, duplication, and best-practice violations.
*   **[Compare](/commands/compare)**: The team saver. Checks your `.env` against a reference (like `.env.example`) to ensure you haven't missed any new keys.
*   **[Fix](/commands/fix)**: The time saver. Automatically parses and corrects common formatting issues safely.
*   **[TUI](/commands/tui)**: The visualizer. An interactive terminal interface for exploring and comparing environment files.

## Philosophy

All commands follow the **Unix philosophy**: do one thing well. They are designed to be composable, with standardized exit codes and optional JSON output for machine parsing.

```bash
# Example: Pipe findings to jq
envcheck lint .env --json | jq '.errors[]'
```
