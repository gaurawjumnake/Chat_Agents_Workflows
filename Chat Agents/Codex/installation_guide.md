# OpenAI Codex CLI — Installation Guide

**Type:** Terminal / CLI Agent + IDE Extension
**Docs:** https://developers.openai.com/codex/cli

---

## Prerequisites

- Node.js 22 recommended (`node --version` to check)
- A ChatGPT Plus, Pro, Business, Edu, or Enterprise subscription — OR an OpenAI API key
- Free plan does not include Codex access

> **Important:** The npm package is `@openai/codex` — not `codex`. The unscoped `codex` package on npm is an unrelated 2012 project. Installing the wrong one causes confusing errors.

---

## Terminal / CLI (all platforms)

```bash
# Install via npm (recommended)
npm install -g @openai/codex

# Or via Homebrew (macOS)
brew install --cask codex

# Verify
codex --version

# Launch — prompts for browser authentication on first run
codex

# Or set your API key directly
export OPENAI_API_KEY="sk-..."
codex
```

**Windows:** Run natively in PowerShell. WSL2 is optional — use it only if your project is in the WSL filesystem.

---

## VS Code / Cursor / Windsurf

1. Open Extensions (`Cmd+Shift+X` / `Ctrl+Shift+X`)
2. Search **Codex** by OpenAI → click **Install**
3. Codex appears in the right sidebar after install
4. If not visible after install, restart the editor
5. Sign in with your ChatGPT account or OpenAI API key

> For Cursor: the activity bar is horizontal by default — pin Codex or check the collapsed section.

---

## JetBrains IDEs

*(IntelliJ IDEA, PyCharm, WebStorm, Rider — version 2025.3 or later)*

Codex is natively integrated into JetBrains AI chat as of January 2026.

1. Open **Settings → Plugins → Marketplace**
2. Search **AI Assistant** (by JetBrains) → install if not already installed
3. Restart the IDE
4. Click the AI widget in the top-right corner
5. In the agent picker, select **Codex**
6. Sign in with your ChatGPT account, OpenAI API key, or JetBrains AI subscription

> Search for "AI Assistant" — not "OpenAI Codex" or "ChatGPT". Third-party plugins with those names are unofficial.

---

## Notes

- **Wrong package:** `npm install -g codex` installs an unrelated 2012 project. Always use `npm install -g @openai/codex`
- **Auth loop:** If the browser opens repeatedly, run `codex auth logout` and re-authenticate. Or use an API key instead of browser auth
- **Sandbox:** On Windows, Codex uses an AppContainer-based sandbox. Network access is blocked by default — use `-c 'sandbox_workspace_write.network_access=true'` when needed
- **JetBrains free promo:** Codex was free via JetBrains AI for a limited promotional period starting January 2026 — this may have expired; check the AI widget for current credit status
