# Connect WhatsApp to OpenClaw

This guide walks you through linking your WhatsApp account to OpenClaw so your agent can send and receive WhatsApp messages.

> **Before you start**
> - OpenClaw must already be installed and running.
> - You need a WhatsApp account on your phone.
> - **Node.js 22 or higher** is required. Check with: `node --version`
> - Recommended: use a secondary phone number or eSIM — not your main personal number.

---

## Step 1: Open the OpenClaw Dashboard

First make sure the gateway is running:

```bash
openclaw gateway start
```

![openclaw gateway start command](screenshots/installation_23.png)

Then open the dashboard:

```bash
openclaw dashboard
```

![openclaw dashboard command in terminal](screenshots/installation_23.png)

> **WSL Note**
> On WSL the browser may not open automatically. If nothing opens, go to your browser manually and visit:
> ```
> http://127.0.0.1:18789/
> ```

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

## Step 3: Add the WhatsApp Channel (CLI)

In your Ubuntu terminal, run:

```bash
openclaw channels add --channel whatsapp
```

![openclaw channels add --channel whatsapp command](screenshots/installation_26.png)

You will see:

```
Added WhatsApp account "default".
```

![WhatsApp account added confirmation](screenshots/installation_27.png)

---

## Step 4: Restart the Gateway

After adding the channel, restart the gateway so the dashboard picks it up:

```bash
openclaw gateway restart
```

![openclaw gateway restart command](screenshots/installation_28.png)

Or stop and start manually:

```bash
openclaw gateway stop
openclaw gateway start
```

> **Wait a few seconds** after the gateway restarts before clicking Show QR. If you click too early you may get a "login provider not available" error.

---

> **Check before continuing**
> If your WhatsApp already has 4 linked devices, go to **WhatsApp → Settings → Linked Devices** on your phone and remove one before scanning the QR code. WhatsApp allows a maximum of 4 linked devices.

---

## Step 5a: Link WhatsApp — via Dashboard

Once the gateway is back up, go to your browser:

1. Refresh the dashboard page.
2. Go to **Channels** → **WhatsApp**.
3. Click **Show QR**.

> **QR codes expire in about 20 seconds.** Have your phone ready and open before clicking Show QR.

![WhatsApp channel — Show QR button](screenshots/installation_29.png)

4. A QR code appears on your screen.

![QR code displayed in dashboard](screenshots/installation_30.png)

5. On your phone: open **WhatsApp** → **Settings** → **Linked Devices** → **Link a Device** → scan the QR code.

After scanning, the dashboard shows:

```
Linked: Yes
Running: Yes
Connected: Yes
```

![WhatsApp linked and running status in dashboard](screenshots/installation_31.png)

---

## Step 5b: Link WhatsApp — via Terminal (Alternative)

If the dashboard QR does not work, scan directly from the terminal instead:

```bash
openclaw channels login --channel whatsapp
```

![QR code displayed in terminal](screenshots/installation_32.png)

Scan the QR code that appears in the terminal with your phone using the same steps above.

---

## Step 6: Allow Your Phone Number

Before testing, add your phone number to the **Allow From** list so the bot accepts your messages.

In the dashboard, find the **Allow From** section:

1. Click **Add**.
2. Enter your number with country code — example: `+923XXXXXXXXX`.
3. Click **Save**.

![Allow From section in dashboard](screenshots/installation_33.png)

![Phone number added to Allow From list](screenshots/installation_34.png)

---

## Step 7: Send a Test Message

Open WhatsApp on your phone and send a message to the number you linked. Try:

```
hello
```

or

```
what can you do?
```

OpenClaw should reply.

![Test message reply in WhatsApp](screenshots/installation_35.png)

---

## Step 8: Fix Raw JSON Replies (If Needed)

If OpenClaw replies with raw JSON instead of normal text, fix it from the dashboard:

**1. Enable Block Streaming** — find **Block Streaming** and toggle it **ON**.

![Block Streaming toggle ON](screenshots/installation_36.png)

**2. Set Chunk Mode to newline** — find **Chunk Mode** and select **newline**.

![Chunk Mode set to newline](screenshots/installation_37.png)

**3. Set Reaction Level to minimal** — find **Reaction Level** and select **minimal**.

![Reaction Level set to minimal](screenshots/installation_38.png)

**4. Click Save.**

![Save button in dashboard](screenshots/installation_39.png)

If replies are still raw JSON after saving, update OpenClaw and restart:

```bash
openclaw update
openclaw gateway restart
```

![openclaw update and restart commands](screenshots/installation_40.png)

---

## Step 9: Approve the Pairing Code (First DM)

The first time someone new messages your agent, OpenClaw sends back a **pairing code** — this code appears as a WhatsApp reply message to the sender, and also in your terminal logs. To approve it, run:

```bash
openclaw pairing approve whatsapp <code>
```

Replace `<code>` with the code from the WhatsApp reply or terminal output.

![Pairing code in WhatsApp reply and terminal logs](screenshots/installation_41.png)

> To allow anyone to message without a pairing code (open mode):
> ```bash
> openclaw config set channels.whatsapp.dmPolicy open
> openclaw config set channels.whatsapp.allowFrom '["*"]'
> ```

---

## Quick Checklist

| Issue | Fix |
|---|---|
| Plugin not installed | `openclaw channels add --channel whatsapp` |
| Gateway not running | `openclaw gateway start` |
| QR expired | Click **Relink** and scan again quickly |
| Already 4 linked devices | Remove an old device in WhatsApp first |
| web login provider is not available | Run `openclaw channels add --channel whatsapp` then restart gateway |
| Raw JSON replies | Enable Block Streaming, set Chunk Mode to newline, Reaction Level to minimal |
| Still broken after settings | `openclaw update` then `openclaw gateway restart` |

---

## Troubleshooting

**WhatsApp disconnected after a while** — Re-scan the QR code:

```bash
openclaw channels login --channel whatsapp
```

**Agent not replying** — Check gateway status:

```bash
openclaw gateway status
```

If the gateway was never started, start it:

```bash
openclaw gateway start
```

If it was running before but stopped, restart it:

```bash
openclaw gateway restart
```
