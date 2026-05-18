# Kiro Installation Guide

Source basis: https://kiro.dev/docs/ and related official Kiro docs as of May 17, 2026.

## Overview

Kiro is documented in two main installation surfaces:

- Kiro IDE: a standalone IDE for macOS, Windows, and Linux
- Kiro CLI: a terminal-based agent for macOS, Windows, and Linux

JetBrains integration is documented through ACP using `kiro-cli`.

I did not find first-party documentation for running Kiro as an extension inside VS Code, Cursor, Windsurf, or Antigravity. The documented GUI path is to install the standalone Kiro IDE. During first run, Kiro can import VS Code settings and extensions.

## Kiro IDE

### System requirements

From the IDE installation docs:

- macOS: Intel and Apple silicon are supported with current security updates
- Windows: Windows 10 and 11, 64-bit only; ARM is not currently supported
- Linux: glibc 2.39 or higher, with examples including Ubuntu 24+, Debian 13+, Fedora 40+, Arch Linux, and Linux Mint 22+

### Install Kiro IDE

1. Go to https://kiro.dev/
2. Download the installer for your operating system.
3. Open the downloaded file and complete the platform installer.
4. Launch Kiro IDE.

### First run

On first launch, the docs describe this flow:

1. Sign in using one of the supported auth providers.
2. Optionally import your VS Code settings and extensions.
3. Pick a theme.
4. Allow shell integration so the agent can execute commands on your behalf.
5. Open a project and continue into the first-project workflow.

### VS Code, Cursor, Windsurf, Antigravity

The official Kiro docs I reviewed do not document a first-party extension install flow for these editors.

What is documented instead:

- Kiro IDE is the primary GUI product.
- Kiro can import VS Code settings and extensions on first run.
- Kiro CLI can be used through ACP-compatible editors when the editor supports ACP.

Practical takeaway:

- If you want the full documented IDE experience, install Kiro IDE.
- If you want to stay in another editor, use `kiro-cli` only where that editor supports ACP.

## JetBrains IDEs

JetBrains integration is documented through Kiro CLI's ACP support.

### Install the CLI first

Use one of the terminal installation methods in the Terminal section below.

### Configure ACP

Run Kiro as an ACP agent with:

```bash
kiro-cli acp
```

For JetBrains AI Assistant, add Kiro as a custom agent in `~/.jetbrains/acp.json`:

```json
{
  "agent_servers": {
    "Kiro Agent": {
      "command": "/full/path/to/kiro-cli",
      "args": ["acp"]
    }
  }
}
```

Then:

1. Open the AI Chat tool window.
2. Click settings.
3. Choose Add Custom Agent.
4. Select `Kiro Agent` from the AI Chat mode selector.

Important note from the docs: use the full path to `kiro-cli`, because IDEs may not inherit your shell `PATH`.

## Terminal / Kiro CLI

### macOS

```bash
curl -fsSL https://cli.kiro.dev/install | bash
```

After installation, Kiro directs you to authenticate in a browser.

### Windows

The CLI docs recommend Windows 11 and PowerShell or Windows Terminal.

```powershell
irm 'https://cli.kiro.dev/install.ps1' | iex
```

Notes from the docs:

- Run this in PowerShell or Windows Terminal, not Command Prompt.
- Windows Terminal is recommended for the best terminal UI experience.

### Linux AppImage

1. Download the AppImage:

```text
https://desktop-release.q.us-east-1.amazonaws.com/latest/kiro-cli.appimage
```

2. Make it executable:

```bash
chmod +x kiro-cli.appimage
```

3. Run it:

```bash
./kiro-cli.appimage
```

### Linux zip install

For standard Linux installs, the docs provide zip packages.

Check glibc first:

```bash
ldd --version
```

If `glibc` is `2.34+`, use the standard archive. If it is older, use the `-musl.zip` build.

Examples from the docs:

```bash
curl --proto '=https' --tlsv1.2 -sSf 'https://desktop-release.q.us-east-1.amazonaws.com/latest/kirocli-x86_64-linux.zip' -o 'kirocli.zip'
curl --proto '=https' --tlsv1.2 -sSf 'https://desktop-release.q.us-east-1.amazonaws.com/latest/kirocli-aarch64-linux.zip' -o 'kirocli.zip'
curl --proto '=https' --tlsv1.2 -sSf 'https://desktop-release.q.us-east-1.amazonaws.com/latest/kirocli-x86_64-linux-musl.zip' -o 'kirocli.zip'
curl --proto '=https' --tlsv1.2 -sSf 'https://desktop-release.q.us-east-1.amazonaws.com/latest/kirocli-aarch64-linux-musl.zip' -o 'kirocli.zip'
```

Then install:

```bash
unzip kirocli.zip
./kirocli/install.sh
```

By default, the files install to `~/.local/bin`.

### Ubuntu

```bash
wget https://desktop-release.q.us-east-1.amazonaws.com/latest/kiro-cli.deb
sudo dpkg -i kiro-cli.deb
sudo apt-get install -f
kiro-cli
```

### First run and login

After launching `kiro-cli`, Kiro directs you to the browser authentication flow.

You can also authenticate explicitly:

```bash
kiro-cli login
```

Supported auth methods in the CLI docs include Builder ID, IAM Identity Center, and social login with Google or GitHub.

Examples:

```bash
kiro-cli login
kiro-cli login --social google
kiro-cli login --use-device-flow
```

### Updates and uninstall

Update the CLI:

```bash
kiro-cli update
```

Windows auto-update can be disabled with:

```powershell
kiro-cli settings "app.disableAutoupdates" "true"
```

Uninstall examples from the docs:

```bash
kiro-cli uninstall
sudo apt-get remove kiro-cli
sudo apt-get purge kiro-cli
```

### Troubleshooting

The CLI docs recommend:

```bash
kiro-cli doctor
```

Useful follow-ups:

```bash
kiro-cli diagnostic --force
kiro-cli issue
```

Common help from the docs:

- Re-authenticate with `kiro-cli login` if auth breaks.
- Check log locations if startup or MCP fails.
- On Windows, run in PowerShell or Windows Terminal.

### Proxy configuration

Kiro CLI respects standard proxy variables:

```bash
export HTTP_PROXY=http://proxy.company.com:8080
export HTTPS_PROXY=http://proxy.company.com:8080
export NO_PROXY=localhost,127.0.0.1,.company.com
```

## First project setup

For the IDE workflow, the docs recommend:

1. Open a project with `File > Open Folder`, drag-and-drop, or `kiro .`
2. Open the Kiro panel.
3. Start a chat session.
4. Generate steering docs.
5. Create your first Spec or start with a Vibe session for quick work.