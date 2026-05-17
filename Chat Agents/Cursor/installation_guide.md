# Cursor — Installation Guide

**Type:** AI-native IDE (VS Code fork)
**Docs:** https://cursor.com/en/docs/get-started/installation

---

## Prerequisites

- macOS 12+, Windows 10/11, or Linux (Ubuntu 20.04+)
- A Cursor account (free or paid)
- No additional extensions needed — AI is built in

---

## Install Cursor (replaces VS Code — or run alongside it)

**macOS:**
```bash
# Download from cursor.com and drag to Applications
# Or via Homebrew
brew install --cask cursor
```

**Windows:**
1. Download the installer from cursor.com
2. Run `CursorSetup.exe`
3. Launch Cursor from Start Menu

**Linux:**
```bash
# Download the .AppImage or .deb from cursor.com
chmod +x cursor-*.AppImage
./cursor-*.AppImage

# Or .deb install
sudo dpkg -i cursor-*.deb
```

---

## First Launch and Sign In

1. Open Cursor
2. Click **Sign In** → authenticate with GitHub, Google, or email
3. Choose your AI model in **Settings → Models** (Claude, GPT-5, Gemini)
4. Open a folder with **File → Open Folder**

---

## Migrate Settings from VS Code

```bash
# Cursor imports VS Code extensions, keybindings, and settings automatically
# On first launch: File → Import VS Code Settings
# Or manually: Cursor > Preferences > Import VS Code Settings
```

All your VS Code extensions, themes, and keybindings carry over automatically.

---

## JetBrains IDEs

Not supported. Cursor is a standalone VS Code fork — there is no JetBrains plugin.

---

## Notes

- Cursor is a full IDE, not a plugin — install it alongside VS Code, not as a replacement, until you're ready to switch
- Extensions from the VS Code Marketplace work in Cursor — your existing extension library is compatible
- Cursor bills on a credit model — heavy use of frontier models (Claude Opus, GPT-5) consumes credits faster
- Cursor does not support JetBrains IDEs
