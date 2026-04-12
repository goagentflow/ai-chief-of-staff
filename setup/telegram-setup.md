# Telegram Setup Guide

*How to set up an old laptop as an always-on Chief of Staff you can message from your phone.*

---

## What This Does

Turns a spare laptop into an always-on assistant that responds via Telegram. You message it from your phone, Claude Code picks it up, responds with full context (your files, calendar, email - whatever you've connected), and pushes any changes to git. Your main machine picks up changes next session via git pull.

**Cost:** Zero extra. Uses your existing Claude subscription.

**What you need:**
- A spare laptop (an old MacBook works perfectly - doesn't need to be powerful)
- Your Claude subscription (same account as your main machine)
- A Telegram account (free)

---

## Step 1: Create a Telegram Bot

1. Open Telegram on your phone
2. Search for **@BotFather** and start a chat
3. Send `/newbot`
4. Choose a name (e.g. "My Chief of Staff")
5. Choose a username (must end in `bot`, e.g. `my_cos_bot`)
6. BotFather will give you a **bot token** - save this somewhere safe. It looks like `1234567890:ABCdefGHIjklMNOpqrsTUVwxyz`

---

## Step 2: Set Up the Spare Laptop

### Install Claude Code (if not already installed)

Open Terminal:
```bash
npm install -g @anthropic-ai/claude-code
```

### Log in to Claude

```bash
claude
```
Follow the login prompts. Use your existing Claude subscription.

Exit when done (Ctrl+C or type `/exit`).

### Clone your Chief of Staff repo

```bash
cd ~/Desktop
git clone https://github.com/YOUR_USERNAME/YOUR_REPO.git chief-of-staff
cd chief-of-staff
```

### Install and configure the Telegram plugin

Start Claude Code:
```bash
cd ~/Desktop/chief-of-staff
claude
```

Inside the session, run these commands:
```
/plugin install telegram@claude-plugins-official
/telegram:configure YOUR_BOT_TOKEN_HERE
```

Replace `YOUR_BOT_TOKEN_HERE` with the token from BotFather.

Exit the session.

---

## Step 3: Start the Telegram Session

```bash
cd ~/Desktop/chief-of-staff
claude --channels plugin:telegram@claude-plugins-official
```

### Pair your Telegram account

On your phone, open Telegram and send any message to your bot (the username you chose, e.g. `@my_cos_bot`).

It will reply with a pairing code.

Back in Claude Code on the spare laptop:
```
/telegram:access pair <the-code-from-telegram>
/telegram:access policy allowlist
```

This means only YOUR Telegram account can talk to the bot. Nobody else.

### Test it

From your phone, send: "What's in my INBOX.md?"

Claude should respond via Telegram with the contents of your inbox.

---

## Step 4: Keep It Running

### Prevent the laptop from sleeping

System Settings → Battery (or Energy Saver):
- **"Prevent automatic sleeping when the display is off"** → ON
- The display can sleep (saves the screen)
- Keep it plugged into power

Or from Terminal (quick fix):
```bash
caffeinate -s &
```

### Auto-start on login (optional)

Create a shell script:
```bash
cat > ~/start-chief-of-staff.sh << 'EOF'
#!/bin/bash
cd ~/Desktop/chief-of-staff
git pull origin main
caffeinate -s &
claude --channels plugin:telegram@claude-plugins-official
EOF
chmod +x ~/start-chief-of-staff.sh
```

Then add `~/start-chief-of-staff.sh` to System Settings → General → Login Items.

Now if the laptop restarts, it'll automatically start the Telegram session.

---

## Step 5: Sync Between Machines

The two machines share files through git. The pattern is simple:

- **Telegram laptop:** Runs `git pull` before responding, `git push` after changing files
- **Main machine:** Runs `git pull` at the start of each session

Add this to your `CLAUDE.md` under Telegram Channel Mode:

```markdown
## Telegram Channel Mode

If you are running with `--channels plugin:telegram@claude-plugins-official`, follow these rules:

### Before responding to any Telegram message
- Run `git pull origin main` to pick up changes from the main Mac

### After any file change
- `git add` the changed files, commit with a clear message, and `git push origin main` immediately
- Do not batch. Do not wait. Push as you go.
```

---

## Daily Use

### If the session dies or the laptop restarts

Just run this from Terminal:
```bash
cd ~/Desktop/chief-of-staff && git pull origin main && claude --channels plugin:telegram@claude-plugins-official
```

### What works over Telegram

Everything your Chief of Staff can do in a normal session works over Telegram:
- Check your calendar and email (if you've connected Office 365 / Gmail)
- Look up client or prospect information
- Capture ideas and add to INBOX.md
- Draft emails
- Check your pipeline
- Answer questions about your projects, decisions, waiting-for items

Keep messages concise - this is Telegram, not a terminal.

---

## Troubleshooting

**Bot not responding:**
- Check Claude Code is still running on the spare laptop (Terminal open, session active)
- Check the laptop hasn't gone to sleep (wake it, restart the session)
- Check Wi-Fi is connected

**Git conflicts:**
- If both machines edited the same file, you'll get a merge conflict on pull
- Fix it on whichever machine you're on, commit, push
- This is rare if the Telegram session pulls before responding and pushes after changes

**"Session limit reached":**
- Your Claude subscription may limit concurrent sessions
- The spare laptop is the priority (always-on). Use it for Telegram, main machine for interactive work.
- They shouldn't conflict since you're typically only actively using one at a time.

---

## Security Notes

- Your bot token is like a password - don't share it or commit it to a public repo
- The `allowlist` policy means only your paired Telegram account can talk to the bot
- Your files stay on your machines and in your private git repo - nothing goes through Telegram's servers except the chat messages themselves
- Save your bot token somewhere safe (e.g. `~/.telegram-bot-tokens.txt`) in case you need to reconfigure
