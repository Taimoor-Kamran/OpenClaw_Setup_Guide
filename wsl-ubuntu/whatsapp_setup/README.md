# Connect WhatsApp to OpenClaw

This guide walks you through linking your WhatsApp account to OpenClaw so your agent can send and receive WhatsApp messages.

> **Before you start**
> - OpenClaw must already be installed and running.
> - You need a WhatsApp account on your phone.
> - **Node.js 22 or higher** is required. Check with: `node --version`
> - Recommended: use a separate phone or eSIM — not your main personal number.

---

## Step 1: Make Sure the Gateway is Running

```bash
openclaw gateway start
```

![openclaw gateway start command](screenshots/whatsapp_1.png)

---

## Step 2: Open the Channel Configuration Wizard

```bash
openclaw configure --section channels
```

![openclaw configure --section channels command](screenshots/whatsapp_2.png)

The wizard starts and shows your existing config.

![Existing config detected](screenshots/whatsapp_3.png)

---

## Step 3: Select Gateway Location

When asked **Where will the Gateway run?**, select **Local (this machine)**.

![Gateway location — Local selected](screenshots/whatsapp_4.png)

---

## Step 4: Open Channel Setup

When asked **Channels**, select **Configure/link**.

![Channels — Configure/link selected](screenshots/whatsapp_5.png)

The wizard shows an info box explaining how channels work. Read it, then continue.

![How channels work info box](screenshots/whatsapp_6.png)

---

## Step 5: Select WhatsApp

When asked **Select a channel**, select **WhatsApp (QR link)**.

![Select a channel — WhatsApp selected](screenshots/whatsapp_7.png)

If WhatsApp is already configured, the wizard asks what you want to do. Select **Modify settings**.

![WhatsApp already configured — Modify settings selected](screenshots/whatsapp_5b.png)

If WhatsApp is already linked, the wizard asks if you want to re-link. Select **No** to keep the existing link, or **Yes** to scan a new QR code.

![WhatsApp already linked — Re-link now prompt](screenshots/whatsapp_5c.png)

---

## Step 6: Install the WhatsApp Plugin

When asked **Install WhatsApp plugin?**, select **Download from npm (@openclaw/whatsapp)**.

![Install WhatsApp plugin — Download from npm selected](screenshots/whatsapp_8.png)

> **If the install fails** with "Package not found on npm", you will see this error:

![Plugin install failed message](screenshots/whatsapp_9.png)

When asked **Use local plugin path instead?**, select **Yes**.

![Use local plugin path — Yes selected](screenshots/whatsapp_10.png)

---

## Step 7: Link Your WhatsApp

The wizard shows a linking info box. Read it, then continue.

![WhatsApp linking info box](screenshots/whatsapp_11.png)

When asked **Link WhatsApp now (QR)?**, select **Yes**.

![Link WhatsApp now — Yes selected](screenshots/whatsapp_12.png)

A QR code appears in the terminal.

![QR code displayed in terminal](screenshots/whatsapp_13.png)

> **The QR code refreshes every ~20 seconds.** Have your phone ready before selecting Yes.

On your phone:
1. Open **WhatsApp**
2. Tap **Settings** → **Linked Devices**
3. Tap **Link a Device**
4. Scan the QR code on your screen

After scanning, the terminal shows:

```
WhatsApp asked for a restart after pairing (code 515); waiting for creds to save…
✅ Linked after restart; web session ready.
```

![WhatsApp linked successfully in terminal](screenshots/whatsapp_14.png)

---

## Step 8: Configure DM Settings

The wizard shows the **WhatsApp DM access** info box explaining DM policies.

![WhatsApp DM access info box](screenshots/whatsapp_15.png)

When asked **WhatsApp phone setup**, select **Separate phone just for OpenClaw**.

![Phone setup — Separate phone selected](screenshots/whatsapp_16.png)

When asked **WhatsApp DM policy**, select **Pairing (recommended)**.

![DM policy — Pairing selected](screenshots/whatsapp_17.png)

When asked **WhatsApp allowFrom**, select **Unset allowFrom (default)**.

![allowFrom — Unset selected](screenshots/whatsapp_18.png)

When asked **Select a channel**, select **Finished**.

![Select a channel — Finished selected](screenshots/whatsapp_19.png)

The wizard shows the selected channels summary.

![Selected channels summary](screenshots/whatsapp_20.png)

---

## Step 9: Complete Setup

The wizard shows your Control UI URL. Save it.

![Control UI info box with dashboard URL](screenshots/whatsapp_21.png)

The wizard finishes.

![Configure complete](screenshots/whatsapp_22.png)

---

## Step 10: Approve the First Message (Pairing)

The first time someone messages your agent, OpenClaw sends them a **pairing code**. You will also see the code in your terminal logs.

To approve it, run:

```bash
openclaw pairing approve whatsapp <code>
```

Replace `<code>` with the code from the terminal output.

![openclaw pairing approve command](screenshots/whatsapp_23.png)

You will see:

```
Approved whatsapp sender +923XXXXXXXXX.
```

![Pairing approved confirmation](screenshots/whatsapp_24.png)

That sender can now chat with your agent freely.

---

> **Open Mode (Optional)**
> To allow anyone to message without a pairing code, run:
> ```bash
> openclaw config set channels.whatsapp.dmPolicy open
> openclaw config set channels.whatsapp.allowFrom '["*"]'
> ```

---

## Quick Checklist

| Issue | Fix |
|---|---|
| Plugin install failed | Select **Use local plugin path instead → Yes** |
| QR code expired | Run the wizard again and scan quickly |
| Already 4 linked devices | Remove one in WhatsApp → Linked Devices first |
| Gateway not running | `openclaw gateway start` |
| Bot not replying | Run `openclaw pairing approve whatsapp <code>` |

---

## Troubleshooting

**WhatsApp disconnected after a while** — Re-run the wizard to re-scan:

```bash
openclaw configure --section channels
```

**Agent not replying** — Check gateway status:

```bash
openclaw gateway status
```

Start or restart as needed:

```bash
openclaw gateway start
openclaw gateway restart
```
