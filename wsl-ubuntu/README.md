# OpenClaw AI — WSL2 + Ubuntu Setup Guide

If you prefer a Linux environment on Windows (recommended for the rest of this book), install OpenClaw inside WSL2 + Ubuntu. WSL gives you a real Linux terminal next to Windows, and every Linux command in this chapter then works exactly as written.

---

## Step 1: Enable WSL and Install Ubuntu

Open **Windows PowerShell as Administrator**.
Press the Windows key, type `powershell`, right-click **Windows PowerShell**, and select **Run as administrator**.

Then run:

```powershell
wsl --install -d Ubuntu
```

![WSL install command in PowerShell](screenshots/powershell.png)

This single command enables the WSL feature, installs the WSL2 kernel, and downloads Ubuntu. **Reboot when prompted.**

After reboot, Ubuntu launches automatically and asks you to create a UNIX username and password. This account is separate from your Windows login. Pick something you will remember — you will need the password for `sudo`.

Verify the install from PowerShell:

```powershell
wsl -l -v
```

![wsl -l -v output showing Ubuntu Running VERSION 2](screenshots/powershell_1.png)

You should see Ubuntu with `STATE: Running` and `VERSION: 2`.

> **Reopen Ubuntu Later**
> Open Ubuntu from the Start menu, or just type `wsl` in any PowerShell window to drop into your default distro.

---

## Step 2: Install Channel SDK Dependencies (Optional but Recommended)

Before running the OpenClaw installer, install the messaging platform SDKs that OpenClaw uses for channels like WhatsApp, Slack, Discord, Telegram, and Nostr. This lets the installer detect them automatically.

Inside the **Ubuntu terminal**, run:

```bash
npm install -g @larksuiteoapi/node-sdk @whiskeysockets/baileys @slack/web-api @slack/bolt @slack/socket-mode nostr-tools discord.js telegraf
```

![npm install of channel SDKs in Ubuntu terminal](screenshots/installation_1.png)

You should see output like:

```
added 327 packages in 21s

54 packages are looking for funding
  run `npm fund` for details
```

![npm install complete](screenshots/npm_install_done.png)

> **What these packages are for**
> - `@whiskeysockets/baileys` — WhatsApp Web
> - `@slack/web-api`, `@slack/bolt`, `@slack/socket-mode` — Slack
> - `discord.js` — Discord
> - `telegraf` — Telegram
> - `nostr-tools` — Nostr (decentralized messaging)
> - `@larksuiteoapi/node-sdk` — Feishu / Lark

---

## Step 3: Install OpenClaw Inside Ubuntu

Inside the **Ubuntu terminal**, run the OpenClaw installer:

```bash
curl -fsSL https://openclaw.ai/install.sh | bash
```

![OpenClaw installer running in Ubuntu terminal](screenshots/installation.png)
![OpenClaw installer running in Ubuntu terminal](screenshots/installation_1.png)
![OpenClaw installer running in Ubuntu terminal](screenshots/installation_2.png)
![OpenClaw installer running in Ubuntu terminal](screenshots/installation_4.png)

The installer runs three stages:

**[1/3] Preparing environment** — detects your OS as `linux`, finds your Node.js and npm versions.

**[2/3] Installing OpenClaw** — checks that Git is installed, then installs the OpenClaw npm package.

**[3/3] Finalizing setup** — refreshes the gateway service metadata.

When it finishes, you will see:

```
🦞 OpenClaw installed successfully (OpenClaw 2026.X.XX (xxxxxxx))!
```

![OpenClaw version confirmation after install](screenshots/version.png)

> **Gateway Restart Warning**
> You may see a message like `Gateway service restart failed — continuing`. This is normal on a fresh WSL install. The gateway will start correctly when you run the setup wizard in the next step.

> **npm Fallback**
> If the install script fails, install directly via npm:
> ```bash
> npm install -g openclaw@latest
> ```
> Then run `openclaw` to start the wizard. To restart the wizard later, use `openclaw setup --wizard`.

---

## Step 4: Fix PATH if `openclaw` is Not Found

If running `openclaw --version` returns `command not found`, add OpenClaw to your shell PATH:

```bash
echo 'export PATH="$HOME/.openclaw/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

![Adding openclaw to PATH in bashrc](screenshots/error_fix.png)

Then verify:

```bash
openclaw --version
```

![openclaw --version showing OpenClaw vX.X.X](screenshots/version.png)

You should see a version line like `OpenClaw vX.X.X (latest)`.

---

## Step 5: Run the Setup Wizard

The installer launches the setup wizard automatically after installation. If it does not start, run:

```bash
openclaw
```

### 5a — Security Disclaimer

The first screen is a security notice. Read it carefully, then select **Yes** to continue.

![OpenClaw security disclaimer screen](screenshots/setup_security.png)

Key points from this screen:
- OpenClaw is in **Beta** — expect rough edges.
- By default it is a **personal agent** with one trusted operator boundary.
- If multiple users can message one tool-enabled agent, they share delegated tool authority.
- Run `openclaw security audit --deep` regularly.

### 5b — Setup Mode

Select **QuickStart** to get up and running quickly.

![Setup mode selection — QuickStart highlighted](screenshots/setup_mode.png)

QuickStart keeps the default gateway settings:

| Setting | Value |
|---|---|
| Gateway port | 18789 |
| Gateway bind | Loopback (127.0.0.1) |
| Gateway auth | Token (default) |
| Tailscale exposure | Off |

### 5c — Config Handling

If a config file already exists, you will be asked how to handle it. Select **Update values** to keep your existing settings and only change what the wizard asks about.

![Config handling — Update values selected](screenshots/setup_config.png)

### 5d — Model / Auth Provider

Select your LLM provider. In this guide we use **Anthropic**.

![Model provider selection — Anthropic highlighted](screenshots/setup_provider.png)

For the auth method, select **Anthropic Claude CLI** if you have Claude Code already logged in. OpenClaw will detect your existing Claude CLI session automatically.

![Anthropic auth method — Claude CLI selected](screenshots/setup_auth.png)

After selection you will see a confirmation:

```
Claude CLI auth detected; switched Anthropic model selection to the local Claude CLI backend.
Default model set to claude-cli/claude-opus-4-7
```

![Model configured confirmation](screenshots/setup_model_confirmed.png)

### 5e — Select a Channel

The wizard shows the status of every supported channel, then asks you to pick one to configure first.

![Channel status overview](screenshots/setup_channel_status.png)

Use the arrow keys to highlight a channel and press **Enter** to configure it. If you want to skip channel setup for now, scroll down and select **Skip for now**.

![Channel selection list](screenshots/setup_channel_select.png)

> **Supported Channels**
> Discord, Slack, Telegram, WhatsApp, Signal, iMessage, Matrix, Nostr, LINE, IRC, and many more. You can add or change channels at any time by running `openclaw config`.

> **Stay Inside WSL**
> From this point on, run every OpenClaw command from your **Ubuntu terminal**, not from PowerShell. Files in `~/.openclaw/` live inside the Linux filesystem and are not visible to native Windows tools.

---

Continue with the **channel configuration** section for the messaging platform you want to connect first.
