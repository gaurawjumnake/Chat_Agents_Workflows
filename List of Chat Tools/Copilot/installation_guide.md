# GitHub Copilot — Installation Guide

**Type:** IDE Extension + Terminal CLI
**Docs:** https://docs.github.com/en/copilot

---

## Prerequisites

- GitHub account with an active Copilot subscription (Free, Pro, Business, or Enterprise)
- VS Code 1.67+ or a supported JetBrains IDE (2025.3+)
- For terminal CLI: Node.js installed

> As of April 2026, new sign-ups for Copilot Pro and Pro+ are temporarily paused. The Free plan is still available.

---

## VS Code / Cursor / Windsurf / Antigravity

1. Open Extensions (`Cmd+Shift+X` on Mac, `Ctrl+Shift+X` on Windows/Linux)
2. Search **GitHub Copilot** → install the extension by GitHub
3. Search **GitHub Copilot Chat** → install this too (required for chat and Agent mode)
4. Click the Copilot icon in the Status Bar → **Sign in to use Copilot**
5. Complete the OAuth flow in your browser
6. Restart the editor

```bash
# Or install both extensions via terminal
code --install-extension GitHub.copilot
code --install-extension GitHub.copilot-chat
```

> For Cursor: the activity bar displays horizontally by default — pin Copilot if it's hidden in the collapsed section.

---

## JetBrains IDEs

*(IntelliJ IDEA, PyCharm, WebStorm, GoLand, Rider, etc.)*

1. Open **File → Settings → Plugins → Marketplace**
2. Search **GitHub Copilot** → click **Install**
3. Restart the IDE when prompted
4. Go to **File → Settings → Tools → GitHub Copilot**
5. Click **Sign In** → complete the OAuth flow in your browser

---

## Terminal / CLI

```bash
# Install Copilot CLI via npm
npm install -g @github/copilot

# Authenticate using GitHub CLI
gh auth login
gh extension install github/gh-copilot

# Verify
gh copilot --version
```

Copilot CLI is included in all Copilot plans (Free, Pro, Business, Enterprise). It also works inside VS Code's integrated terminal.

---

## Notes

- **April 2026:** GitHub silently opted all users into AI training — go to `github.com/settings/copilot` and opt out manually if needed
- **June 2026:** Billing is moving to a credit model — credits do not roll over month to month
- Chat is available in VS Code, JetBrains, and Visual Studio. Inline suggestions work in all supported editors: Vim, Neovim, Azure Data Studio, Xcode
- Run `/init` in a new Copilot Chat session to let it analyse your codebase and auto-generate a `.github/copilot-instructions.md` file
