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

![openclaw dashboard command in terminal](screenshots/dashboard_cmd.png)

This opens the dashboard in your browser automatically. If it does not open, go to:

```
http://127.0.0.1:18789/
```

If prompted for a token, paste your gateway token. To get it run:

```bash
openclaw config get gateway.auth.token
```

![OpenClaw dashboard in browser](screenshots/dashboard.png)

---

## Step 2: Go to Channels

In the dashboard, click **Channels** in the left sidebar.

![Channels section in dashboard](screenshots/channels.png)

---

## Step 3: Select WhatsApp

Find **WhatsApp** in the channel list and click **Configure**.

![WhatsApp channel in the list](screenshots/whatsapp_channel.png)

---

## Step 4: Scan the QR Code

OpenClaw generates a QR code. Open WhatsApp on your phone and link it:

1. Open WhatsApp on your phone.
2. Tap the **three dots** menu (Android) or **Settings** (iPhone).
3. Tap **Linked Devices**.
4. Tap **Link a Device**.
5. Scan the QR code shown in the OpenClaw dashboard.

![QR code shown in OpenClaw dashboard](screenshots/qr_code.png)

![Scanning QR code from WhatsApp on phone](screenshots/qr_scan.png)

---

## Step 5: Wait for Connection

After scanning, WhatsApp shows **Device linked** and the dashboard updates to show WhatsApp as **connected**.

![WhatsApp connected status in dashboard](screenshots/connected.png)

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
