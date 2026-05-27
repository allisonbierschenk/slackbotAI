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
2. Pick your workspace, paste the contents of `manifest.json`, click **Create**
3. **Socket Mode** → toggle on → generate an app-level token (starts with `xapp-`)
4. **OAuth & Permissions** → **Install to Workspace** → copy the Bot Token (`xoxb-`) and User Token (`xoxp-`)
5. **Basic Information** → copy the **Signing Secret**

---

## 3. Configure your `.env`

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

## 4. Run it

```bash
npm start
```

You should see:

```
Scriptbot is running on port 3000
Now connected to Slack
```

Then in Slack: open the bot → **Home** tab → **+ Add trigger collection** → DM the bot the trigger phrase.

---

## 5. Stop the server

`Ctrl+C` in the terminal.

---

## Troubleshooting

- **No response?** Make sure `npm start` is still running and Socket Mode is enabled.
- **`dispatch_failed`?** Wrong `SLACK_SIGNING_SECRET` — recheck Basic Information.
- **Bot shows wrong name/icon?** App settings → **App Home** → enable "Always Show My Bot as Online".

Full step-by-step walkthrough: [SETUP.md](SETUP.md).
