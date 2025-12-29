+++
title = "Installation"
weight = 1
+++



Install envcheck using any of the following methods:

## Cargo

```bash
cargo install envcheck
```

## Binary Releases

Download pre-built binaries from the [GitHub Releases](https://github.com/envcheck/envcheck/releases) page:

- Linux (x86_64, aarch64)
- macOS (x86_64, arm64 / Apple Silicon)
- Windows (x86_64)

## Homebrew

```bash
brew tap envcheck/tap
brew install envcheck
```

## npm

```bash
npm install -g @envcheck/cli
# Or use without installing:
npx @envcheck/cli lint .env
```

## Docker

```bash
docker pull ghcr.io/envcheck/envcheck:latest
docker run --rm -v $(pwd):/workdir ghcr.io/envcheck/envcheck lint .env
```

## Building from Source

```bash
git clone https://github.com/envcheck/envcheck.git
cd envcheck
cargo install --path .
```

## Verify Installation

```bash
envcheck --version
envcheck --help
```

## Shell Completions

Enable shell autocompletion:

### Bash
```bash
envcheck completions bash > /etc/bash_completion.d/envcheck
source /etc/bash_completion.d/envcheck
```

### Zsh
```bash
envcheck completions zsh > ~/.zfunc/_envcheck
```

Add to `~/.zshrc`:
```zsh
fpath=(~/.zfunc $fpath)
autoload -U compinit && compinit
```

### Fish
```bash
envcheck completions fish > ~/.config/fish/completions/envcheck.fish
```

### PowerShell
```powershell
envcheck completions powershell | Out-File -Encoding UTF8 envcheck.ps1
# Then add to your PowerShell profile
```
