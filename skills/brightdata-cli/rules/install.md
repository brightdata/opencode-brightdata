---
name: brightdata-cli-installation
description: |
  Install the Bright Data CLI and handle authentication errors.
---

# Bright Data CLI Installation

## Quick Install

**macOS / Linux:**
```bash
curl -fsSL https://cli.brightdata.com/install.sh | bash
```

**Any platform (requires Node.js >= 20):**
```bash
npm install -g @brightdata/cli
```

**Without installing (one-off usage):**
```bash
npx --yes --package @brightdata/cli brightdata <command>
```

## Verify Installation

Check if installed and authenticated:

```bash
bdata config
```

## Authentication

If not authenticated, run:

```bash
bdata login
```

This opens the browser for secure OAuth authentication. After login:
1. API key is saved locally (never needs to be entered again)
2. Required proxy zones (`cli_unlocker`, `cli_browser`) are auto-created
3. Default configuration is set

For headless/SSH environments where no browser is available:
```bash
bdata login --device
```

For direct API key authentication (non-interactive):
```bash
bdata login --api-key <key>
```

## If you fail to authenticate, use the following error handling instructions:

If ANY command returns an authentication error after you ran `bdata login` (e.g., "not authenticated", "unauthorized", "API key"), use an ask user question tool if available:

**Question:** "How would you like to authenticate with Bright Data?"

**Options:**

1. **Login with browser (Recommended)** — Opens your browser to authenticate with Bright Data
2. **Login with device flow** — For SSH/headless environments
3. **Enter API key manually** — Paste an existing API key from brightdata.com

### If user selects browser login:

Run `bdata login` to automatically open the browser. Wait for them to confirm authentication, then retry the original command.

### If user selects device flow:

Run `bdata login --device` and guide them through the device code flow.

### If user selects manual API key:

Ask for their API key, then run:

```bash
bdata login --api-key "<their-key>"
```

Or set the environment variable:

```bash
export BRIGHTDATA_API_KEY="<their-key>"
```

Tell them to add this export to `~/.zshrc` or `~/.bashrc` for persistence, then retry the original command.

## Troubleshooting

### Command not found

If `bdata` command is not found after installation:

1. Make sure npm global bin is in PATH
2. Try: `npx --yes --package @brightdata/cli brightdata --version`
3. Or reinstall: `npm install -g @brightdata/cli`

### Permission errors

If you get permission errors during installation:

```bash
# Option 1: Use the curl installer (recommended)
curl -fsSL https://cli.brightdata.com/install.sh | bash

# Option 2: Fix npm permissions
mkdir ~/.npm-global
npm config set prefix '~/.npm-global'
export PATH=~/.npm-global/bin:$PATH
# Add the export to your shell profile
```

### Zone errors

If you get "No Web Unlocker zone specified":

```bash
bdata config set default_zone_unlocker <zone>
# Or re-run login to auto-create zones:
bdata login
```
