# Connect Microsoft Teams to OpenClaw

This guide walks you through creating a Teams bot and connecting it to OpenClaw so your agent can send and receive Microsoft Teams messages.

> **Before you start**
> - OpenClaw must already be installed and running.
> - You need a Microsoft account and access to the Azure Portal at https://portal.azure.com
> - You need a Microsoft Teams account.
> - Node.js 22 or higher is required. Check with: `node --version`

---

## Step 1: Create an Azure Bot

Go to https://portal.azure.com and sign in.

![Azure Portal homepage](screenshots/teams_1.png)

Click **Create a resource** in the top left.

![Create a resource button](screenshots/teams_2.png)

Search for `Azure Bot` and click **Azure Bot** in the results.

![Azure Bot in search results](screenshots/teams_3.png)

Click **Create**.

![Create button on Azure Bot page](screenshots/teams_4.png)

Fill in the required fields:
- **Bot handle** — give your bot a name (example: `MyOpenClawBot`)
- **Subscription** — select your subscription
- **Resource group** — create new or select existing
- **Pricing tier** — select **F0** (free)
- **Microsoft App ID** — select **Create new Microsoft App ID**

![Azure Bot creation form filled out](screenshots/teams_5.png)

Click **Review + create**.

![Review + create button](screenshots/teams_6.png)

Click **Create** to confirm.

![Final Create button](screenshots/teams_7.png)

Wait for the deployment to complete, then click **Go to resource**.

![Deployment complete — Go to resource](screenshots/teams_8.png)

---

## Step 2: Get Your App ID and Password

In your Azure Bot resource, click **Configuration** in the left sidebar.

![Configuration in left sidebar](screenshots/teams_9.png)

Copy the **Microsoft App ID** and save it — you will need it in Step 5.

![Microsoft App ID copied](screenshots/teams_10.png)

Click **Manage Password** (next to the App ID).

![Manage Password link](screenshots/teams_11.png)

This opens the App Registration page. Click **Certificates & secrets** in the left sidebar.

![Certificates and secrets section](screenshots/teams_12.png)

Click **New client secret**.

![New client secret button](screenshots/teams_13.png)

Give it a description and select an expiry, then click **Add**.

![Client secret description and expiry filled](screenshots/teams_14.png)

Copy the **Value** immediately — it will be hidden after you leave this page.

> ⚠️ **Save this secret now!** You cannot view it again after leaving this page.

![Client secret value copied](screenshots/teams_15.png)

---

## Step 3: Enable the Teams Channel in Azure

Go back to your Azure Bot resource. Click **Channels** in the left sidebar.

![Channels in left sidebar](screenshots/teams_16.png)

Click **Microsoft Teams**.

![Microsoft Teams channel option](screenshots/teams_17.png)

Accept the terms of service and click **Agree**.

![Terms of service — Agree button](screenshots/teams_18.png)

Click **Apply**.

![Apply button](screenshots/teams_19.png)

---

## Step 4: Add the Teams Channel — via Dashboard

Make sure the gateway is running:

```bash
openclaw gateway run
```

![openclaw gateway run command](screenshots/teams_20.png)

Open your browser and go to `http://127.0.0.1:18789/`

![OpenClaw dashboard open in browser](screenshots/teams_21.png)

Click **Channels** in the left sidebar.

![Channels section in dashboard](screenshots/teams_22.png)

Find **Microsoft Teams** and click **Configure**.

![Microsoft Teams — Configure button](screenshots/teams_23.png)

Enter your **Microsoft App ID** in the App ID field.

![App ID entered](screenshots/teams_24.png)

Enter your **Client Secret** in the App Password field.

![Client Secret entered](screenshots/teams_25.png)

Click **Save**.

![Save button clicked](screenshots/teams_26.png)

---

## Step 4 (Alternative): Add the Teams Channel — via CLI

```bash
openclaw channels add --channel teams --app-id YOUR_APP_ID --app-password YOUR_CLIENT_SECRET
```

Replace `YOUR_APP_ID` and `YOUR_CLIENT_SECRET` with the values from Step 2.

![openclaw channels add --channel teams command in terminal](screenshots/teams_27.png)

---

## Step 5: Restart the Gateway

After adding the channel, restart the gateway so it picks up Teams:

```bash
openclaw gateway stop
```

![openclaw gateway stop command](screenshots/teams_28.png)

```bash
openclaw gateway run
```

![openclaw gateway run command](screenshots/teams_29.png)

> Wait a few seconds after restart before testing.

---

## Step 6: Set the Messaging Endpoint in Azure

OpenClaw needs to receive messages from Teams. Go back to your Azure Bot resource and click **Configuration**.

![Configuration in left sidebar](screenshots/teams_30.png)

In the **Messaging endpoint** field, enter:

```
https://YOUR_OPENCLAW_URL/channels/teams/webhook
```

> If you are running locally with WSL, you will need a tunnel tool like `ngrok` to expose your local gateway. Run:
> ```bash
> ngrok http 18789
> ```
> Then use the `https://` URL that ngrok gives you.

![Messaging endpoint entered](screenshots/teams_31.png)

Click **Apply**.

![Apply button](screenshots/teams_32.png)

---

## Step 7: Install the Bot in Teams

Open Microsoft Teams. Click **Apps** in the left sidebar.

![Apps in Teams left sidebar](screenshots/teams_33.png)

Click **Manage your apps** at the bottom.

![Manage your apps button](screenshots/teams_34.png)

Click **Upload an app**.

![Upload an app button](screenshots/teams_35.png)

Click **Upload a customized app**.

![Upload a customized app option](screenshots/teams_36.png)

> If you do not see this option, your Teams admin may need to enable sideloading. Ask your admin or use a personal Teams account.

Select your app manifest file and click **Open**.

![App manifest file selected](screenshots/teams_37.png)

Click **Add** to install the bot.

![Add button to install bot](screenshots/teams_38.png)

---

## Step 8: Test the Bot

The bot opens as a chat. Send a message:

```
hello
```

![hello message sent in Teams](screenshots/teams_39.png)

The bot should reply.

![Bot reply in Teams](screenshots/teams_40.png)

---

## Step 9: Fix Raw JSON Replies (If Needed)

If the bot replies with raw JSON instead of normal text, fix it from the dashboard.

Go to **Channels** → **Microsoft Teams**.

![Channels → Microsoft Teams in dashboard](screenshots/teams_41.png)

Enable **Block Streaming** — toggle it **ON**.

![Block Streaming toggled ON](screenshots/teams_42.png)

Set **Chunk Mode** to `newline`.

![Chunk Mode set to newline](screenshots/teams_43.png)

Set **Reaction Level** to `minimal`.

![Reaction Level set to minimal](screenshots/teams_44.png)

Click **Save**.

![Save button clicked](screenshots/teams_45.png)

---

## Quick Checklist

| Issue | Fix |
|---|---|
| Bot not responding | Check messaging endpoint is set correctly in Azure |
| Plugin not installed | `openclaw channels add --channel teams` |
| Gateway not running | `openclaw gateway run` |
| Invalid App ID or Secret | Re-check values from Azure Bot Configuration |
| Raw JSON replies | Enable Block Streaming, set Chunk Mode to newline |
| Sideloading not available | Ask Teams admin to enable custom app uploads |
| Client secret expired | Create a new secret in Azure App Registration |

---

## Troubleshooting

**Bot installed but not replying** — Check the messaging endpoint in Azure is correct and the gateway is running.

**Authentication error in logs** — Your App ID or Client Secret may be wrong. Re-check them:

```bash
openclaw config set channels.teams.appId YOUR_APP_ID
openclaw config set channels.teams.appPassword YOUR_CLIENT_SECRET
openclaw gateway restart
```

**Client secret expired** — Go to Azure Portal → App Registration → Certificates & secrets → create a new secret and update OpenClaw.

**Gateway never started** — Start it with:

```bash
openclaw gateway start
```
