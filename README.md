# envcheck Documentation

This is the official documentation website for [envcheck](https://github.com/envcheck/envcheck), built with [Zola](https://www.getzola.org/) static site generator.

## Local Development

### Install Zola

```bash
# macOS
brew install zola

# Linux
cargo install zola

# Or download from https://www.getzola.org/
```

### Serve Locally

```bash
zola serve
```

Visit http://127.0.0.1:1111/

### Build

```bash
zola build
```

Output is in `public/`.

## Project Structure

```
.
├── config.toml          # Zola configuration
├── content/             # Documentation content (Markdown)
│   ├── introduction/    # Getting started
│   ├── commands/        # CLI reference
│   ├── rules/           # Lint rules
│   ├── integration/     # CI/CD integration
│   └── advanced/        # Advanced topics
├── static/              # Static assets
├── themes/              # Zola themes (book)
└── .github/
    └── workflows/
        └── deploy.yml   # GitHub Pages deployment
```

## Deployment

The site is automatically deployed to GitHub Pages on push to `main`.

## Contributing

1. Edit content in `content/`
2. Test locally: `zola serve`
3. Submit a PR

## License

MIT
