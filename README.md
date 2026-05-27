# slackbotAI — Slack Trigger Bot Demo

A Slack app that auto-replies to trigger phrases you define from the Home tab. If a message doesn't match any trigger, Claude answers as a fallback.

**Stack:** Node + Slack Bolt (Socket Mode) · Anthropic Claude SDK

---

## What's in the code

| File | Purpose |
| --- | --- |
| `src/index.js` | Bolt app entry — wires up events, actions, and the Home tab. |
| `src/homeView.js` | Builds the Home tab UI and modals (add/view triggers). |
| `src/triggerStore.js` | Stores trigger phrases → responses per user. |
| `src/playback.js` | Plays back scripted multi-turn responses. |
| `src/claudeFallback.js` | Calls the Anthropic API when no trigger matches. |
| `src/scriptLoader.js` / `parseResponse.js` | Loads and parses scripted conversations. |
| `manifest.json` | Slack app manifest — paste this when creating the app. |
| `.env.example` | Required environment variables — copy to `.env` and fill in. |

---

## 1. Install

```bash
npm install
```

Requires Node.js 18+.

---

## 2. Create the Slack app

1. Go to [api.slack.com/apps](https://api.slack.com/apps) → **Create New App** → **From a manifest**
2. Pick your workspace
3. Delete everything in the text box and paste the contents of `manifest.json` from this repo
4. Click **Next** → **Create**

This sets up the bot user, scopes, slash command, event subscriptions, and Home tab in one shot.

---

## 3. Enable Socket Mode and grab your tokens

You'll need **four** Slack values for `.env`. Here's where each one lives:

### a. App-level token (`SLACK_APP_TOKEN`, starts with `xapp-`)
1. Left sidebar → **Socket Mode**
2. Toggle **Enable Socket Mode** on
3. Name the token (e.g. `my-app-token`) → **Generate**
4. Copy the `xapp-…` token

### b. Bot token + User token (`SLACK_BOT_TOKEN`, `SLACK_USER_TOKEN`)
1. Left sidebar → **OAuth & Permissions**
2. Click **Install to Workspace** → **Allow**
3. Copy the **Bot User OAuth Token** (`xoxb-…`)
4. Copy the **User OAuth Token** (`xoxp-…`)

### c. Signing secret (`SLACK_SIGNING_SECRET`)
1. Left sidebar → **Basic Information**
2. Scroll to **App Credentials** → copy **Signing Secret**

### d. (Optional) make the bot always show as online
**App Home** → check **Always Show My Bot as Online**.

---

## 4. Configure your `.env`

```bash
cp .env.example .env
```

Fill in every value. **Never commit this file** — it's already in `.gitignore`.

| Variable | Where it comes from |
| --- | --- |
| `SLACK_BOT_TOKEN` | OAuth & Permissions → **Bot User OAuth Token** (`xoxb-…`) |
| `SLACK_APP_TOKEN` | Socket Mode → app-level token (`xapp-…`) |
| `SLACK_USER_TOKEN` | OAuth & Permissions → **User OAuth Token** (`xoxp-…`) |
| `SLACK_SIGNING_SECRET` | Basic Information → **App Credentials** |
| `ANTHROPIC_API_KEY` | [console.anthropic.com](https://console.anthropic.com) → API keys |
| `PORT` | Local port (default `3000`) |

---

## 5. Run it

```bash
npm start
```

You should see:

```
Scriptbot is running on port 3000
Now connected to Slack
```

---

## 6. Use it in Slack

1. In Slack, search for your app by name (the one you set in `manifest.json`)
2. Open it → **Home** tab → **+ Add trigger collection**
3. Set a trigger phrase and the response you want
4. DM the bot that phrase in the **Messages** tab — it replies automatically
5. Anything that doesn't match a trigger gets answered by Claude

---

## 7. Stop the server

`Ctrl+C` in the terminal.

---

## Troubleshooting

- **No response?** Make sure `npm start` is still running and Socket Mode is enabled.
- **`dispatch_failed`?** Wrong `SLACK_SIGNING_SECRET` — recheck Basic Information.
- **Bot shows wrong name/icon?** App settings → **App Home** → enable "Always Show My Bot as Online".

Full step-by-step walkthrough with screenshots-style detail: [SETUP.md](SETUP.md).
