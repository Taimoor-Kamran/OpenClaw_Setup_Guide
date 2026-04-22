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

![wsl -l -v output showing Ubuntu Running VERSION 2](screenshots/02-wsl-verify.png)

You should see Ubuntu with `STATE: Running` and `VERSION: 2`.

> **Reopen Ubuntu Later**
> Open Ubuntu from the Start menu, or just type `wsl` in any PowerShell window to drop into your default distro.

---

## Step 2: Install OpenClaw Inside Ubuntu

Inside the **Ubuntu terminal**, run the same Linux installer:

```bash
curl -fsSL https://openclaw.ai/install.sh | bash
```

![OpenClaw installer running in Ubuntu terminal](screenshots/03-install-script.png)

The installer detects Linux, installs Node.js if needed, installs the OpenClaw npm package, and creates `~/.openclaw/` inside your Ubuntu home directory (not your Windows `C:\Users\...`).

When it finishes, you see a version confirmation:

```
OpenClaw vX.X.X (latest)
```

![OpenClaw version confirmation after install](screenshots/04-version-confirmation.png)

> **npm Fallback**
> If the install script fails, install directly via npm:
> ```bash
> npm install -g openclaw@latest
> ```
> Then run `openclaw` to start the wizard. To restart the wizard later, use `openclaw setup --wizard`.

---

## Step 3: Fix PATH if `openclaw` is Not Found

If running `openclaw --version` returns `command not found`, add OpenClaw to your shell PATH:

```bash
echo 'export PATH="$HOME/.openclaw/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

![Adding openclaw to PATH in bashrc](screenshots/05-path-fix.png)

Then verify:

```bash
openclaw --version
```

![openclaw --version showing OpenClaw vX.X.X](screenshots/06-version-check.png)

You should see a version line like `OpenClaw vX.X.X (latest)`.

---

## Step 4: Start the Onboarding Wizard

On WSL, the installer does not auto-launch the wizard. Start it yourself, and install the background daemon at the same time:

```bash
openclaw onboard --install-daemon
```

![openclaw onboard --install-daemon running in Ubuntu](screenshots/07-onboard-daemon.png)

The `--install-daemon` flag registers the gateway as a systemd user service inside Ubuntu so it restarts automatically when you reopen WSL. The wizard then walks you through:

- Security acknowledgment
- LLM provider setup
- Channel pairing

> **Stay Inside WSL**
> From this point on, run every OpenClaw command from your **Ubuntu terminal**, not from PowerShell. Files in `~/.openclaw/` live inside the Linux filesystem and are not visible to native Windows tools.

---

Continue with the **Security Warning** section below. The wizard, Gemini setup, and channel pairing all work identically inside WSL.
