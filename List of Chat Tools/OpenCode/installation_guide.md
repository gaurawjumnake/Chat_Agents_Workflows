# OpenCode — Installation Guide

**Type:** Terminal / CLI Agent + IDE Extension
**Docs:** https://opencode.ai/docs

---

## Prerequisites

- macOS, Linux, or Windows (WSL2 recommended on Windows)
- An API key for at least one provider: OpenAI, Gemini, Mistral, or a local Ollama instance
- Note: Anthropic blocked OpenCode from using Claude models in January 2026 — use GPT-5.4, Gemini 3.1 Pro, or Ollama as alternatives

---

## Terminal / CLI

```bash
# Recommended — official install script (auto-detects platform)
curl -fsSL https://opencode.ai/install | bash

# Or via npm
npm install -g opencode-ai

# Or via Homebrew (macOS/Linux — always up to date)
brew install anomalyco/tap/opencode

# Windows (Scoop)
scoop install opencode

# Windows (Chocolatey)
choco install opencode

# Verify
opencode --version

# Launch
opencode
```

On first launch, OpenCode prompts you to authenticate with a provider:
```bash
opencode auth login
# Select your provider (OpenAI, Gemini, Mistral, Ollama, etc.)
# Enter your API key
```

---

## VS Code / Cursor / Windsurf / Antigravity

```bash
# Run opencode INSIDE the IDE's integrated terminal (not your system terminal)
# Open the integrated terminal: Ctrl+~ (Windows/Linux) or Cmd+~ (Mac)
opencode
# OpenCode auto-detects the IDE and installs its extension automatically
```

> The extension does not appear in the VS Code Marketplace — it only installs when you run `opencode` from inside the IDE's integrated terminal.

Quick launch shortcuts after extension is installed:
- `Cmd+Esc` / `Ctrl+Esc` — open OpenCode in a split terminal view
- `Cmd+Shift+Esc` / `Ctrl+Shift+Esc` — start a new OpenCode session

---

## JetBrains IDEs

1. Open **Settings → Plugins → Marketplace**
2. Search **OpenCode** → install the plugin
3. Restart the IDE
4. The plugin reads context from active file, cursor position, and open tabs

Plugin link: https://plugins.jetbrains.com/plugin/30681-opencode

---

## Desktop App

```bash
# macOS
brew install --cask opencode-desktop

# Windows
scoop bucket add extras
scoop install extras/opencode-desktop

# Or download from: opencode.ai/download
```

---

## Notes

- **Anthropic block (Jan 2026):** Claude models are unavailable in OpenCode. Use GPT-5.4, Gemini 3.1 Pro, or a local Ollama model instead
- **VS Code extension:** Must be launched from the IDE's integrated terminal — marketplace search will not find it
- **WSL on Windows:** OpenCode works better in a WSL environment than native Windows for most Linux-based projects
- **Ollama (local, zero cloud):** `opencode auth login` → select Ollama → no API key needed
