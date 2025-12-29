+++
title = "🔄 CI/CD Integration"
weight = 4
sort_by = "weight"
template = "section.html"
+++

# Seamless Integration, Everywhere

Security isn't something you do once; it's a continuous process. `envcheck` is designed to "shift left"—catching configuration issues as early as possible in your development lifecycle.

## Workflow Integrations

1.  **Local Development**: Use [pre-commit hooks](/integration/pre-commit) to prevent bad env files from ever being committed.
2.  **CI Pipelines**: Run `envcheck` in GitHub Actions, GitLab CI, or CircleCI to block broken builds.
3.  **Deployment**: Verify Kubernetes Secrets or Docker containers before they go live.

## The "Shift Left" Advantage

By integrating `envcheck` into your CI/CD pipeline, you gain:

*   **Zero-Config Security**: Block commits that contain `AWS_SECRET_KEY` or other sensitive patterns.
*   **Production Parity**: Ensure that the keys defined in `env.production` match what your application expects code-wise.
*   **Audit Trail**: Generate [SARIF reports](/integration/output-formats) for GitHub Security tab integration.
