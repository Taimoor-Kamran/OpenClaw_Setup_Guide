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

## Step 2: Install Channel SDK Dependencies

Inside the **Ubuntu terminal**, run:

```bash
npm install -g @larksuiteoapi/node-sdk @whiskeysockets/baileys @slack/web-api @slack/bolt @slack/socket-mode nostr-tools discord.js telegraf
```

![npm install running in Ubuntu terminal](screenshots/installation_0.png)

---

## Step 3: Install OpenClaw

Inside the **Ubuntu terminal**, run:

```bash
curl -fsSL https://openclaw.ai/install.sh | bash
```

![Installer starting — detecting linux and checking Node.js](screenshots/installation_1.png)

![Installer stage 2 — installing OpenClaw npm package](screenshots/installation_2.png)

![Installer stage 3 — finalizing setup](screenshots/installation_3.png)

![OpenClaw installed successfully message](screenshots/installation_4.png)

> **Gateway Restart Warning**
> You may see `Gateway service restart failed — continuing`. This is normal on a fresh WSL install and is fixed in Step 7.

> **npm Fallback**
> If the install script fails:
> ```bash
> npm install -g openclaw@latest
> ```

---

## Step 4: Fix PATH if `openclaw` is Not Found

If `openclaw --version` returns `command not found`, run:

```bash
echo 'export PATH="$HOME/.openclaw/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

![Adding openclaw to PATH in bashrc](screenshots/error_fix.png)

---

## Step 5: Complete the Setup Wizard

The installer launches the setup wizard automatically. If you need to restart it later, run `openclaw`.

### 5a — Security Disclaimer

Read the notice, then select **Yes**.

![Security disclaimer screen](screenshots/installation_5.png)

### 5b — Setup Mode

Select **QuickStart**.

![Setup mode — QuickStart selected](screenshots/installation_6.png)

### 5c — Config Handling

Select **Update values**.

![Config handling screen](screenshots/installation_7.png)

### 5d — Model / Auth Provider

Select **Anthropic**, then select **Anthropic Claude CLI**.

![Model provider — Anthropic selected](screenshots/setup_provider.png)

![Auth method — Claude CLI selected](screenshots/setup_auth.png)

![Model configured confirmation](screenshots/setup_model_confirmed.png)

When asked **Default model**, select **Keep current**.

> **Model Not Found Warning**
> You may see `Model not found: claude-cli/claude-opus-4-7`. Ignore it for now — update the model later with `/models list` inside the agent.

### 5e — Channel Selection

Select **Skip for now**. You can connect channels any time by running `openclaw config`.

![Channel status overview](screenshots/setup_channel_status.png)

![Channel selection — Skip for now](screenshots/setup_channel_select.png)

### 5f — Web Search

Select **Skip for now**.

![Web search provider screen](screenshots/setup_web_search.png)

### 5g — Skills

Select **No**.

![Skills status screen](screenshots/setup_skills.png)

### 5h — Hooks

Select **Skip for now**.

![Hooks configuration screen](screenshots/setup_hooks.png)

### 5i — Gateway Service

Select **Restart**.

![Gateway service restart screen](screenshots/setup_gateway_restart.png)

### 5j — Control UI

The wizard shows your dashboard URL. Save it.

![Control UI screen with dashboard URL](screenshots/setup_control_ui.png)

### 5k — Start TUI

Select **Do this later**.

![Start TUI prompt](screenshots/setup_tui.png)

### 5l — Onboarding Complete

![Onboarding complete screen](screenshots/setup_complete.png)

---

## Step 6: Verify the Installation

```bash
openclaw --version
```

![openclaw --version output](screenshots/version.png)

---

## Step 7: Install the Background Daemon

```bash
openclaw onboard --install-daemon
```

![openclaw onboard --install-daemon wizard](screenshots/onboard_daemon.png)

This re-runs the same wizard. Follow the same choices as Step 5.

![Onboarding with daemon complete](screenshots/onboard_daemon_complete.png)

> **Stay Inside WSL**
> Run every OpenClaw command from your **Ubuntu terminal**, not from PowerShell.

---

## Step 8: Open the Dashboard

```bash
openclaw dashboard
```

![openclaw dashboard opening in browser](screenshots/dashboard_open.png)

![OpenClaw dashboard in browser](screenshots/dashboard_browser.png)

---

Continue with the **channel configuration** section to connect your first messaging platform.
