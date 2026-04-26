# Connect Discord to OpenClaw

This guide walks you through creating a Discord bot and connecting it to OpenClaw so your agent can send and receive Discord messages.

> **Before you start**
> - OpenClaw must already be installed and running.
> - You need a Discord account at https://discord.com
> - You must have a Discord server where you have admin permissions.
> - Node.js 22 or higher is required. Check with: `node --version`

---

## Step 1: Create a Discord Application

1. Go to https://discord.com/developers/applications
2. Click **New Application** in the top right.
3. Give your application a name (example: "My OpenClaw Bot").
4. Click **Create**.

![Discord Developer Portal — New Application](screenshots/discord_1.png)

---

## Step 2: Create a Bot User

1. In the left sidebar, click **Bot**.
2. Click **Add Bot** then confirm by clicking **Yes, do it!**
3. Under **Token**, click **Reset Token** then **Copy** to copy your bot token.

> ⚠️ **Keep your token secret!** Never share it or commit it to GitHub. If leaked, reset it immediately.

![Bot page — Reset Token and Copy](screenshots/discord_2.png)

4. Scroll down to **Privileged Gateway Intents** and enable all three:
   - **Presence Intent**
   - **Server Members Intent**
   - **Message Content Intent**
5. Click **Save Changes**.

![Privileged Gateway Intents enabled](screenshots/discord_3.png)

---

## Step 3: Invite the Bot to Your Server

1. In the left sidebar, click **OAuth2** → **URL Generator**.
2. Under **Scopes**, check:
   - `bot`
   - `applications.commands`
3. Under **Bot Permissions**, check:
   - `Send Messages`
   - `Read Message History`
   - `View Channels`
   - `Use Slash Commands`
4. Copy the generated URL at the bottom and open it in your browser.
5. Select your server and click **Authorize**.

![OAuth2 URL Generator with scopes and permissions](screenshots/discord_4.png)

![Bot authorization screen](screenshots/discord_5.png)

---

## Step 4: Add the Discord Channel — via Dashboard

1. Make sure the gateway is running:

```bash
openclaw gateway run
```

2. Open the dashboard in your browser at `http://127.0.0.1:18789/`
3. Click **Channels** in the left sidebar.
4. Find **Discord** and click **Configure**.
5. Paste your bot token in the **Token** field.
6. Click **Save**.

![Discord channel configuration in dashboard](screenshots/discord_6.png)

---

## Step 4 (Alternative): Add the Discord Channel — via CLI

If you prefer the terminal, run:

```bash
openclaw channels add --channel discord
```

When prompted, paste your bot token and press Enter.

![openclaw channels add --channel discord command](screenshots/discord_7.png)

---

## Step 5: Restart the Gateway

After adding the channel, restart the gateway so it picks up Discord:

```bash
openclaw gateway stop
openclaw gateway run
```

> Wait a few seconds after restart before testing.

![Gateway restart command](screenshots/discord_8.png)

---

## Step 6: Allow Your Discord User ID

Before testing, add your Discord user ID to the **Allow From** list.

**How to get your Discord user ID:**
1. Open Discord.
2. Go to **Settings** → **Advanced** → enable **Developer Mode**.
3. Right-click your username anywhere and click **Copy User ID**.

Then in the dashboard:
1. Go to **Channels** → **Discord**.
2. Find **Allow From** and click **Add**.
3. Paste your user ID.
4. Click **Save**.

![Allow From section with Discord user ID added](screenshots/discord_9.png)

---

## Step 7: Test the Bot

1. Open Discord and go to your server.
2. Send a message in any channel where the bot has access:
hello

or mention the bot directly:
@YourBotName what can you do?

The bot should reply!

![Bot replying in Discord server](screenshots/discord_10.png)

---

## Step 8: Fix Raw JSON Replies (If Needed)

If the bot replies with raw JSON instead of normal text, fix it from the dashboard:

1. Go to **Channels** → **Discord**.
2. Enable **Block Streaming** — toggle it **ON**.
3. Set **Chunk Mode** to `newline`.
4. Set **Reaction Level** to `minimal`.
5. Click **Save**.

![Block Streaming and Chunk Mode settings](screenshots/discord_11.png)

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
