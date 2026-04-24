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

## Step 3: Select WhatsApp

Find **WhatsApp** in the channel list and click **Configure**.

![WhatsApp channel in the list](screenshots/installation_26.png)

---

## Step 4: WhatsApp Configuration Page

After clicking Configure, the WhatsApp setup page opens in the dashboard.

![WhatsApp configuration page](screenshots/installation_27.png)

---

## Step 5: Click "Link WhatsApp"

Click the **Link WhatsApp** button (or **Connect** / **QR Login** — the label depends on your OpenClaw version).

![Link WhatsApp button in dashboard](screenshots/installation_28.png)

---

## Step 6: QR Code Appears on Screen

OpenClaw generates and displays a QR code on your laptop screen.

![QR code displayed in OpenClaw dashboard](screenshots/installation_29.png)

> **QR code expires in ~60 seconds.** If it expires before you scan it, click **Refresh** or reload the page to get a new one.

---

## Step 7: Scan the QR Code with Your Phone

Open WhatsApp on your phone and scan the QR code shown on your laptop screen. After scanning, the dashboard updates automatically.

![QR code being scanned — dashboard waiting for scan](screenshots/installation_30.png)

---

## Step 8: WhatsApp Linked Successfully

Once scanned, the dashboard shows WhatsApp status as **Connected**.

![WhatsApp connected status in dashboard](screenshots/installation_31.png)

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
