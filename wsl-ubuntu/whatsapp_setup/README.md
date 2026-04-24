# Connect WhatsApp to OpenClaw

This guide walks you through linking your WhatsApp account to OpenClaw so your agent can send and receive WhatsApp messages.

> **Before you start**
> - OpenClaw must already be installed and the dashboard must be open in your browser.
> - You need a WhatsApp account on your phone.
> - Recommended: use a secondary phone number or eSIM — not your main personal number.

---

## Step 1: Open the OpenClaw Dashboard

Run this command in your Ubuntu terminal to open the dashboard:

```bash
openclaw dashboard
```

![openclaw dashboard command in terminal](screenshots/installation_23.png)

This opens the dashboard in your browser automatically. If it does not open, go to:

```
http://127.0.0.1:18789/
```

If prompted for a token, paste your gateway token. To get it run:

```bash
openclaw config get gateway.auth.token
```

![OpenClaw dashboard in browser](screenshots/installation_24.png)

---

## Step 2: Go to Channels

In the dashboard, click **Channels** in the left sidebar.

![Channels section in dashboard](screenshots/installation_25.png)

---

## Step 3: Install the WhatsApp Plugin

In your Ubuntu terminal, run:

```bash
openclaw channels add --channel whatsapp
```

![openclaw channels add --channel whatsapp command](screenshots/installation_26.png)

OpenClaw will prompt you to install the `@openclaw/whatsapp` plugin. Confirm the install.

![Plugin install confirmation prompt](screenshots/installation_27.png)

---

## Step 4: Restart the Gateway

After the plugin installs, restart the gateway:

```bash
openclaw gateway restart
```

![openclaw gateway restart command](screenshots/installation_28.png)

Or stop and start it manually:

```bash
openclaw gateway stop
openclaw gateway start
```

---

## Step 5: Link WhatsApp via QR Code

Once the gateway is back up:

1. Go to your dashboard → **Channels** → **WhatsApp**.
2. Click **Show QR**.

![WhatsApp channel showing Show QR button](screenshots/installation_29.png)

3. A QR code appears on your screen.

![QR code displayed in dashboard](screenshots/installation_30.png)

4. On your phone: open **WhatsApp** → **Settings** → **Linked Devices** → **Link a Device** → scan the QR code.

The status changes to **Linked: Yes** and **Running: Yes**.

![WhatsApp linked and running status in dashboard](screenshots/installation_31.png)

---

## Step 6: Send a Test Message

Send a message to your own WhatsApp number (or ask someone to message the linked number). Your OpenClaw agent should reply.

![Test message received and replied to in WhatsApp](screenshots/test_message.png)

---

## Step 7: Approve the Pairing Code (First DM)

The first time someone new messages your agent, OpenClaw sends a **pairing code** for security. To approve it, run:

```bash
openclaw pairing approve whatsapp <code>
```

Replace `<code>` with the code shown in the message.

![Pairing code approval in terminal](screenshots/pairing.png)

> To allow anyone to message without a pairing code (open mode):
> ```bash
> openclaw config set channels.whatsapp.dmPolicy open
> openclaw config set channels.whatsapp.allowFrom '["*"]'
> ```

---

## Troubleshooting

**QR code expired** — QR codes expire after about 60 seconds. Refresh the dashboard page to get a new one.

**WhatsApp disconnected after a while** — Run `openclaw gateway status --deep` to check the connection. Re-scan the QR code if needed.

**Agent not replying** — Make sure the gateway service is running:

```bash
openclaw gateway status
```

If it is stopped, restart it:

```bash
openclaw gateway restart
```
