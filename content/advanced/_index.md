+++
title = "⚙️ Configuration"
weight = 5
sort_by = "weight"
template = "section.html"
+++

# Tailored to Your Workflow

Every project is unique. While `envcheck` comes with sensible defaults, it offers deep configuration options to match your team's specific needs.

## Customization Options

*   **[.envcheckrc](/configuration/envcheckrc)**: Define global rules, excluded keys, and varying severity levels (Error, Warning, Info) in a simple TOML or YAML file.
*   **Ignore Files**: Use `.envcheckignore` (similar to `.gitignore`) to skip specific files or directories.
*   **Rule Toggling**: Enable or disable specific checks. Legacy project with lowercase keys? No problem—just disable `KeyFormat` check.

## Scalable Configuration

For large organizations, you can share configuration presets across multiple repositories, ensuring a consistent security posture across your entire engineering organization.
