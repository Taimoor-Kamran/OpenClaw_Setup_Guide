# Connect Discord to OpenClaw

This guide walks you through creating a Discord bot and connecting it to OpenClaw so your agent can send and receive Discord messages.

> **Before you start**
> - OpenClaw must already be installed and running.
> - You need a Discord account at https://discord.com
> - You must have a Discord server where you have admin permissions.
> - Node.js 22 or higher is required. Check with: `node --version`

---

Run the channel wizard and select **Discord (Bot API)**.

```bash
openclaw configure --section channels
```

![Select Discord Bot API in wizard](screenshots/discord_1.png)

The wizard needs three things: a **server name (guild)**, a **channel name**, and a **bot token**. You will create the server and channel first, then create the bot.

---

## Step 1: Create a Discord Server

If you already have a server you want to use, skip to Step 2.

Open Discord and click the **+** icon on the left sidebar.

![Plus icon on Discord left sidebar](screenshots/discord_2.png)

Select **Create My Own**.

![Create My Own option](screenshots/discord_3.png)

Choose **For me and my friends** (or any option — it does not matter for this setup).

![For me and my friends selected](screenshots/discord_4.png)

Give your server a name (example: `AI Office`) and click **Create**.

![Server name entered and Create button](screenshots/discord_5.png)

![Server created](screenshots/discord_6.png)

> Remember this name — the wizard will ask for it.

---

## Step 2: Create a Channel

Your server comes with a `#general` channel by default. You can use that, or create a dedicated channel.

Click the **+** next to **Text Channels** in your server.

![Plus next to Text Channels](screenshots/discord_7.png)

Select **Text Channel**.

![Text Channel selected](screenshots/discord_8.png)

Name it (example: `ai-employee`) and click **Create Channel**.

![Channel name entered and Create Channel button](screenshots/discord_9.png)

![Channel created in server](screenshots/discord_10.png)

> Remember this channel name. If you are using the default, the channel name is `general`.

---

## Step 3: Enable Developer Mode

Open **User Settings** by clicking the gear icon near your name at the bottom left.

![Gear icon — User Settings](screenshots/discord_11.png)

Scroll down and click **Developer**.

![Advanced section in settings](screenshots/discord_12.png)

Toggle **Developer Mode** to **ON**.

![Developer Mode toggled ON](screenshots/discord_13.png)

---

## Step 4: Create the Bot and Get the Token

Go to https://discord.com/developers/applications

![Discord Developer Portal](screenshots/discord_14.png)

Click **New Application** and give it a name (example: `My AI Employee`).

![New Application name entered](screenshots/discord_15.png)

In the left sidebar, click **Bot**.

![Bot in left sidebar](screenshots/discord_16.png)

Find the **Token** section and click **Reset Token**.

![Reset Token button](screenshots/discord_17.png)

Click **Yes, do it!** to confirm, then click **Copy**.

> ⚠️ **Keep this token secret.** Never share it or commit it to GitHub. This is the password for your bot.

![Token copied](screenshots/discord_18.png)

Scroll down to **Privileged Gateway Intents**. Toggle **ON** all three:

**Presence Intent**

![Presence Intent toggled ON](screenshots/discord_19.png)

**Server Members Intent**

![Server Members Intent toggled ON](screenshots/discord_20.png)

**Message Content Intent** — this is the most important. Without it the bot cannot read your messages.

![Message Content Intent toggled ON](screenshots/discord_21.png)

Click **Save Changes**.

![Save Changes clicked](screenshots/discord_22.png)

---

## Step 5: Invite the Bot to Your Server

In the left sidebar, click **OAuth2**, then select **URL Generator**.

![OAuth2 URL Generator](screenshots/discord_23.png)

Under **Scopes**, check `bot`.

![bot scope checked](screenshots/discord_24.png)

Under **Bot Permissions**, check:
- `Read Messages / View Channels`
- `Send Messages`
- `Read Message History`

![Bot permissions checked](screenshots/discord_25.png)

Copy the URL generated at the bottom.

![Generated URL copied](screenshots/discord_26.png)

Paste it into a new browser tab and follow the prompts to add the bot to your server.

![Bot invite page in browser](screenshots/discord_27.png)

Click **Authorize**.

![Authorize button clicked](screenshots/discord_28.png)

---

## Step 6: Enter the Details in the Wizard

Go back to the OpenClaw wizard. It asks for three values:

**Guild (server name)** — enter your server name (example: `AI Office`).

![Guild server name entered in wizard](screenshots/discord_29.png)

**Channel** — enter your channel name (default is `general`).

![Channel name entered in wizard](screenshots/discord_30.png)

**Bot Token** — paste the token you copied in Step 4.

![Bot token pasted in wizard](screenshots/discord_31.png)

The wizard writes the configuration for you. No manual editing required.

![Wizard configuration complete](screenshots/discord_32.png)

---

## Step 7: Test the Bot

> **DM the Bot First — Do Not Message the Channel.**
>
> The wizard asked for a server and a channel, so it feels like the bot now lives inside `#ai-employee`. It does not. By default, Discord bots only reply to **direct messages**. If you send "hello" in `#ai-employee` after setup, the bot will stay silent.

In your server, look at the right-hand member list and click your bot's name.

![Bot name in member list](screenshots/discord_33.png)

Click **Message** to open a DM.

![Message button to open DM](screenshots/discord_34.png)

Send a message:

```
hello
```

![hello sent in bot DM](screenshots/discord_35.png)

The bot should reply.

![Bot reply in DM](screenshots/discord_36.png)

> To make the bot reply inside a channel, you need to change `groupPolicy` to `open` and `@mention` the bot. For now, use DM.

---

## Quick Checklist

| Issue | Fix |
|---|---|
| Bot not in server | Re-invite using OAuth2 URL Generator in Step 5 |
| Gateway not running | `openclaw gateway run` |
| Bot not replying in channel | Use DM instead — bots reply to DMs by default |
| Token invalid | Reset token in Developer Portal and re-run wizard |
| Missing intents | Enable all 3 Privileged Gateway Intents in Step 4 |
| Bot silent in DM | Check Message Content Intent is ON |

---

## Troubleshooting

**Bot is online but not reading messages** — Check that **Message Content Intent** is enabled in the Discord Developer Portal.

**Invalid token error** — Reset your token and re-run the wizard:

```bash
openclaw configure --section channels
```

**Bot was removed from server** — Re-invite it using the OAuth2 URL Generator in Step 5.

**Gateway never started** — Start it with:

```bash
openclaw gateway start
```
