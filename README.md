# ShyNet — Discord AI Bot Manager

A local web dashboard to run and manage your Gemini-powered Discord bot with Google Search grounding.
Works on **macOS, Windows, and Linux** with a single command.

---

## Requirements

- **Python 3.11 or newer - needs to be installed on your machine**
  - macOS: `brew install python@3.11` or https://www.python.org/downloads/
  - Windows: https://www.python.org/downloads/ *(check "Add Python to PATH")*
  - Ubuntu/Debian: `sudo apt install python3.11 python3.11-venv`

---

## Quick Start

### macOS / Linux
```bash
chmod +x start.sh
./start.sh
```

### Windows
Double-click `start.bat` — or run in Command Prompt:
```
start.bat
```

The script will:
1. Detect your Python version
2. Create an isolated virtual environment (`.venv/`)
3. Install all dependencies automatically
4. Apply the macOS SSL certificate fix if needed
5. Open http://localhost:5000 in your browser

---

## Manual Setup (any platform)

```bash
python3 -m venv .venv

source .venv/bin/activate        # macOS/Linux
.venv\Scripts\activate           # Windows

pip install -r requirements.txt
python app.py
```

Then open http://localhost:5000

---

## Getting a Gemini API Key

You need a Gemini API key to power the AI responses. It's free to get.

**Step 1 — Sign in to Google AI Studio**

1. Go to https://aistudio.google.com
2. Sign in with your Google account

**Step 2 — Create an API Key**

1. Click **Get API Key** in the left sidebar
2. Click **Create API key**
3. Select an existing Google Cloud project or click **Create a new project**
4. Click **Create API key in existing project**
5. Copy the key that appears — it starts with `AIza`

> Keep your API key private. Do not share it or commit it to GitHub.

**Step 3 — Enter it in ShyNet**

1. Open http://localhost:5000
2. Go to **Configuration**
3. Paste the key into the **Gemini API Key** field
4. Click **Save Configuration**

**Free tier limits:**
- 1,500 requests per day
- 15 requests per minute

If you need more, enable billing at https://ai.google.dev — usage costs are minimal for personal bots.

---

## Adding Your Bot to a Discord Server

Before ShyNet can connect to Discord, you need to create a bot account and invite it to your server. This only needs to be done once.

**Step 1 — Create a Discord Application**

1. Go to https://discord.com/developers/applications
2. Click **New Application** in the top right
3. Give it a name (e.g. ShyNet) and click **Create**

**Step 2 — Create the Bot User**

1. Click **Bot** in the left sidebar
2. Click **Add Bot** → **Yes, do it!**
3. Under **Privileged Gateway Intents**, enable **Message Content Intent**
4. Click **Save Changes**
5. Click **Reset Token** → copy the token and paste it into ShyNet's Discord Token field

> Keep your token private. Anyone with it can control your bot.

**Step 3 — Invite the Bot to Your Server**

1. Click **OAuth2** in the left sidebar → **URL Generator**
2. Under **Scopes**, check `bot`
3. Under **Bot Permissions**, check:
   - View Channels
   - Send Messages
   - Read Message History
4. Copy the generated URL, paste it into your browser, select your server, click **Authorize**

**Step 4 — Start ShyNet**

1. Open http://localhost:5000
2. Go to **Configuration** and enter your Discord Token, Gemini API Key, and AI name
3. Click **Save Configuration**
4. Go to **Dashboard** and click **▶ Start Bot**
5. Watch the Logs — when you see `Logged on as YourBot#0000` the bot is live

**Triggering the bot in Discord:**
```
ShyNet what is the weather in Tokyo?
```
Or @mention the bot in any channel it has access to.

---

## Stopping ShyNet

**Closing the browser tab does NOT stop the app.** The bot keeps running in the terminal.

To fully stop everything:
- Click **■ Stop Bot** in the dashboard first
- Then press **Ctrl+C** in the terminal

---

## Project Structure

```
shynet/
├── app.py
├── bot.py
├── requirements.txt
├── start.sh
├── start.bat
└── templates/
    └── index.html
```

---

## Configuration

All settings are entered through the web UI:

| Field | Where to get it |
|---|---|
| Discord Bot Token | Discord Developer Portal → Your App → Bot → Token |
| Gemini API Key | https://aistudio.google.com/apikey |
| AI Name | Users trigger the bot by starting messages with this name |
| System Instruction | Optional persona/rules for the AI |

**Security:** Keys are held in memory only — never written to disk.

---

## How the Bot Works

- Users trigger the bot by starting a message with the AI's name or @mentioning it
- The message is sent to Gemini 3.1 Flash Lite Preview with Google Search grounding enabled
- The AI can search the web in real time before responding
- The response is sent back to the Discord channel

---

## Troubleshooting

**macOS SSL error** (`CERTIFICATE_VERIFY_FAILED`):
```bash
/Applications/Python\ 3.11/Install\ Certificates.command
```
Or: `pip install --upgrade certifi`

**Port already in use:**
Edit the last line of `app.py` and change `port=5000` to `port=5001`.

**Bot token invalid:**
Regenerate the token in the Discord Developer Portal and re-enter it in the UI.

**Gemini quota exceeded:**
Free tier allows 1,500 requests/day. Wait for the daily reset or enable billing at https://ai.google.dev.

**Message Content Intent error:**
Discord Developer Portal → Your App → Bot → enable Message Content Intent → Save.
