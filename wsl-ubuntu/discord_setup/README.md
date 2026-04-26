# Connect Discord to OpenClaw

This guide walks you through creating a Discord bot and connecting it to OpenClaw so your agent can send and receive Discord messages.

> **Before you start**
> - OpenClaw must already be installed and running.
> - You need a Discord account at https://discord.com
> - You must have a Discord server where you have admin permissions.
> - Node.js 22 or higher is required. Check with: `node --version`

---

## Step 1: Create a Discord Application

Go to https://discord.com/developers/applications

![Discord Developer Portal homepage](screenshots/discord_1.png)

Click **New Application** in the top right.

![New Application button highlighted](screenshots/discord_2.png)

Give your application a name (example: "My OpenClaw Bot") and click **Create**.

![Application name entered and Create button](screenshots/discord_3.png)

![New application created — overview page](screenshots/discord_4.png)

---

## Step 2: Create a Bot User

In the left sidebar, click **Bot**.

![Bot option selected in left sidebar](screenshots/discord_5.png)

Click **Add Bot**.

![Add Bot button](screenshots/discord_6.png)

A confirmation dialog appears. Click **Yes, do it!**

![Yes do it confirmation dialog](screenshots/discord_7.png)

Under **Token**, click **Reset Token**.

![Reset Token button](screenshots/discord_8.png)

Click **Yes, do it!** again to confirm, then click **Copy** to copy your bot token. Save it somewhere safe — you will need it in Step 4.

> ⚠️ **Keep your token secret!** Never share it or commit it to GitHub. If leaked, reset it immediately.

![Token copied](screenshots/discord_9.png)

Scroll down to **Privileged Gateway Intents**.

![Privileged Gateway Intents section](screenshots/discord_10.png)

Enable **Presence Intent**.

![Presence Intent toggled ON](screenshots/discord_11.png)

Enable **Server Members Intent**.

![Server Members Intent toggled ON](screenshots/discord_12.png)

Enable **Message Content Intent**.

![Message Content Intent toggled ON](screenshots/discord_13.png)

Click **Save Changes**.

![Save Changes button clicked](screenshots/discord_14.png)

---

## Step 3: Invite the Bot to Your Server

In the left sidebar, click **OAuth2**, then click **URL Generator**.

![OAuth2 URL Generator in left sidebar](screenshots/discord_15.png)

Under **Scopes**, check `bot`.

![bot scope checked](screenshots/discord_16.png)

Also check `applications.commands`.

![applications.commands scope checked](screenshots/discord_17.png)

Under **Bot Permissions**, check:
- `Send Messages`
- `Read Message History`
- `View Channels`
- `Use Slash Commands`

![Bot permissions checked](screenshots/discord_18.png)

Scroll to the bottom and copy the generated URL.

![Generated invite URL at the bottom](screenshots/discord_19.png)

Open the URL in your browser. Select your server from the dropdown.

![Bot invite page — server selected](screenshots/discord_20.png)

Click **Authorize**.

![Authorize button clicked](screenshots/discord_21.png)

Complete the CAPTCHA if prompted. You will see a confirmation that the bot was authorized.

![Bot authorized successfully](screenshots/discord_22.png)

---

## Step 4: Add the Discord Channel — via Dashboard

Make sure the gateway is running:

```bash
openclaw gateway run
```

![openclaw gateway run command](screenshots/discord_23.png)

Open your browser and go to `http://127.0.0.1:18789/`

![OpenClaw dashboard open in browser](screenshots/discord_24.png)

Click **Channels** in the left sidebar.

![Channels section in dashboard](screenshots/discord_25.png)

Find **Discord** and click **Configure**.

![Discord — Configure button](screenshots/discord_26.png)

Paste your bot token in the **Token** field.

![Bot token pasted in Token field](screenshots/discord_27.png)

Also while you're there, enable these two intents by toggling them ON:

**Discord Guild Members Intent**

![Discord Guild Members Intent toggled ON](screenshots/discord_27a.png)

**Discord Presence Intent**

![Discord Presence Intent toggled ON](screenshots/discord_27b.png)

Click **Save**.

![Save button clicked](screenshots/discord_28.png)

---

## Step 4 (Alternative): Add the Discord Channel — via CLI

If you prefer the terminal, run:

```bash
openclaw channels add --channel discord
```

![openclaw channels add --channel discord command](screenshots/discord_29.png)

When prompted, paste your bot token and press Enter.

![Token entered in terminal prompt](screenshots/discord_30.png)

---

## Step 5: Restart the Gateway

After adding the channel, restart the gateway so it picks up Discord:

```bash
openclaw gateway stop
```

![openclaw gateway stop command](screenshots/discord_31.png)

```bash
openclaw gateway run
```

![openclaw gateway run command](screenshots/discord_32.png)

> Wait a few seconds after restart before testing.

---

## Step 6: Allow Your Discord User ID

Before testing, add your Discord user ID to the **Allow From** list so the bot accepts your messages.

**How to get your Discord user ID:**

Open Discord and go to **Settings**.

![Discord Settings](screenshots/discord_33.png)

Click **Advanced**.

![Advanced settings tab](screenshots/discord_34.png)

Enable **Developer Mode**.

![Developer Mode toggled ON](screenshots/discord_35.png)

Go back to any server. Right-click your username and click **Copy User ID**.

![Right-click menu — Copy User ID](screenshots/discord_36.png)

Now go back to the dashboard. Click **Channels** → **Discord**.

![Channels → Discord in dashboard](screenshots/discord_37.png)

Find the **Allow From** section and click **Add**.

![Allow From — Add button](screenshots/discord_38.png)

Paste your Discord user ID and click **Save**.

![User ID entered and Save clicked](screenshots/discord_39.png)

---

## Step 7: Test the Bot

Open Discord and go to your server. Send a message in any channel where the bot has access:

```
hello
```

![Sending hello message in Discord](screenshots/discord_40.png)

Or mention the bot directly:

```
@YourBotName what can you do?
```

The bot should reply.

![Bot reply in Discord](screenshots/discord_41.png)

---

## Step 8: Fix Raw JSON Replies (If Needed)

If the bot replies with raw JSON instead of normal text, fix it from the dashboard.

Go to **Channels** → **Discord**.

![Channels → Discord in dashboard](screenshots/discord_42.png)

Enable **Block Streaming** — toggle it **ON**.

![Block Streaming toggled ON](screenshots/discord_43.png)

Set **Chunk Mode** to `newline`.

![Chunk Mode set to newline](screenshots/discord_44.png)

Set **Reaction Level** to `minimal`.

![Reaction Level set to minimal](screenshots/discord_45.png)

Click **Save**.

![Save button clicked](screenshots/discord_46.png)

---

## Quick Checklist

| Issue | Fix |
|---|---|
| Bot not appearing in server | Re-invite using OAuth2 URL Generator |
| Plugin not installed | `openclaw channels add --channel discord` |
| Gateway not running | `openclaw gateway run` |
| Bot not replying | Check Allow From list has your user ID |
| Raw JSON replies | Enable Block Streaming, set Chunk Mode to newline |
| Token invalid | Reset token in Discord Developer Portal and update config |
| Missing intents | Enable all 3 Privileged Gateway Intents in Developer Portal |

---

## Troubleshooting

**Bot is online but not replying** — Check that Message Content Intent is enabled in the Discord Developer Portal. Without it the bot cannot read messages.

**Invalid token error in logs** — Reset your token in the Discord Developer Portal and update it:

```bash
openclaw config set channels.discord.token YOUR_NEW_TOKEN
openclaw gateway restart
```

**Bot was removed from server** — Re-invite it using the OAuth2 URL Generator in Step 3.

**Gateway never started** — Start it with:

```bash
openclaw gateway start
```
