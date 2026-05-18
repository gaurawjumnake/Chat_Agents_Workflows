# Claude Code — Installation Guide

**Type:** Terminal / CLI Agent + IDE Extension
**Docs:** https://docs.anthropic.com/en/docs/claude-code/getting-started

---

## Prerequisites

- Node.js 18 or higher (`node --version` to check)
- An Anthropic account (claude.ai) — for OAuth sign-in
- OR an Anthropic API key — for direct API access
- For enterprise: Amazon Bedrock or Google Vertex AI credentials

---

## Terminal / CLI (all platforms)

```bash
# Install globally via npm
npm install -g @anthropic-ai/claude-code

# Verify
claude --version

# Launch and authenticate (browser OAuth)
claude

# Or authenticate with API key
export ANTHROPIC_API_KEY="sk-ant-..."
claude
```

Run `claude` inside your project folder. It reads your repository from the current directory.

---

## VS Code / Cursor / Windsurf / Antigravity

1. Open Extensions (`Cmd+Shift+X` / `Ctrl+Shift+X`)
2. Search **Claude Code** by Anthropic → click **Install**
3. The extension appears in the sidebar
4. Sign in with your Anthropic account or API key
5. Open the Claude panel via the sidebar icon or `Ctrl+Shift+P` → **Claude Code: Open Panel**

```bash
# Or install via terminal
code --install-extension anthropic.claude-code

# Verify existing install
code --list-extensions | grep claude
```

> For Cursor: Claude Code appears in the right sidebar. If not visible, restart Cursor after installation.

---

## JetBrains IDEs

*(IntelliJ IDEA, PyCharm, WebStorm, GoLand, Rider, PhpStorm, Android Studio, etc.)*

1. Open **Settings → Plugins → Marketplace**
2. Search **Claude Code** (by Anthropic PBC) → click **Install**
3. Restart the IDE
4. Open the Claude panel from the tool window on the right side
5. Authenticate — the plugin uses the same credentials as the CLI

```bash
# If Claude Code CLI is already installed, connect it to the IDE
claude /ide
```

> The JetBrains plugin is a wrapper around the CLI. If Claude isn't on your PATH, set the custom path in **Settings → Tools → Claude Code [Beta]**.

---

## Notes

- **WSL (Windows):** Set a custom claude command path in JetBrains: `wsl -d Ubuntu -- bash -lic "claude"`
- **Enterprise (Bedrock):** Set `ANTHROPIC_API_KEY` to your Bedrock credentials and configure the endpoint before running `claude`
- Claude Code requires an active Claude Pro or Max subscription, or an Anthropic API key with sufficient credits
- Context window: 1M tokens with automatic smart compaction for long sessions
