# Scheduled Agents Setup - Email Alerts & Meeting Prep

*A cloud agent that checks your email and preps you for meetings, automatically.*

---

## What This Does

A scheduled agent runs every hour on weekdays and does two things:

1. **Email scan** - checks your Outlook for client or prospect emails you haven't replied to, then pings you via Telegram with a summary and suggested reply
2. **Meeting prep** - checks your calendar for meetings in the next 60-90 minutes, searches your Chief of Staff files for anything relevant to the people you're meeting, and sends you a prep brief via Telegram

Most hours you hear nothing. Silence means nothing needs your attention.

**Cost:** Included in your Claude subscription. Supabase free tier is enough.

---

## What You Need

- A Claude account with scheduled agents access (Claude Code)
- A Supabase project (free tier works)
- The Telegram bot from the [Telegram setup guide](telegram-setup.md)
- Microsoft 365 MCP connector connected in claude.ai

---

## How It Works (The Architecture)

The agent runs in Anthropic's cloud, not on your machine. Each hour it:

1. Clones your repo and reads your `CLIENTS.md` and `prospects/` files to know who matters
2. Uses the Microsoft 365 MCP connector to check your email and calendar
3. Sends you Telegram messages when something needs your attention

**One critical thing to understand:** The cloud environment blocks direct HTTP calls. The agent cannot use `curl`, `fetch`, or any direct web requests. Everything that needs to reach the outside world goes through Supabase - specifically, a Supabase Edge Function called via the Postgres HTTP extension through the Supabase MCP connector.

This sounds convoluted, but the setup is straightforward. You deploy a tiny function to Supabase that sends Telegram messages, and the agent calls it via SQL. This is the only way it works in the cloud environment - skip this and nothing will send.

---

## Step 1: Set Up Supabase

1. Go to [supabase.com](https://supabase.com) and create a free account (or log in)
2. Create a new project - call it whatever you like (e.g. "chief-of-staff")
3. Choose a region close to you and set a database password
4. Once created, note your **Project URL** (looks like `https://abcdefghijkl.supabase.co`) and **Project ID** (the `abcdefghijkl` part)

---

## Step 2: Deploy the Telegram Edge Function

You need a small function that receives a message and sends it to Telegram. This keeps your bot token secure inside Supabase rather than exposed in prompts.

Connect the Supabase MCP in claude.ai (Settings > MCP Connectors > Supabase), then ask Claude to deploy this edge function, or deploy it manually via the Supabase dashboard (Edge Functions > Deploy a new function).

**Function name:** `send-telegram`

**Code:**

```typescript
import { serve } from "https://deno.land/std@0.177.0/http/server.ts";

serve(async (req) => {
  // Auth check - reject requests without the correct secret
  const authHeader = req.headers.get("X-Edge-Auth");
  const expectedSecret = Deno.env.get("EDGE_AUTH_SECRET");

  if (!authHeader || authHeader !== expectedSecret) {
    return new Response(JSON.stringify({ error: "Unauthorised" }), {
      status: 401,
      headers: { "Content-Type": "application/json" },
    });
  }

  const { message } = await req.json();

  if (!message) {
    return new Response(JSON.stringify({ error: "No message provided" }), {
      status: 400,
      headers: { "Content-Type": "application/json" },
    });
  }

  const botToken = Deno.env.get("TELEGRAM_BOT_TOKEN");
  const chatId = Deno.env.get("TELEGRAM_CHAT_ID");

  if (!botToken || !chatId) {
    return new Response(JSON.stringify({ error: "Missing Telegram config" }), {
      status: 500,
      headers: { "Content-Type": "application/json" },
    });
  }

  const telegramUrl = `https://api.telegram.org/bot${botToken}/sendMessage`;

  const telegramRes = await fetch(telegramUrl, {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({
      chat_id: chatId,
      text: message,
      parse_mode: "Markdown",
    }),
  });

  const result = await telegramRes.json();

  if (!result.ok) {
    return new Response(JSON.stringify({ error: "Telegram send failed", detail: result }), {
      status: 502,
      headers: { "Content-Type": "application/json" },
    });
  }

  return new Response(JSON.stringify({ success: true }), {
    status: 200,
    headers: { "Content-Type": "application/json" },
  });
});
```

---

## Step 3: Set Supabase Secrets

Go to your Supabase dashboard > Edge Functions > Secrets, and add three secrets:

| Secret name | Value |
|---|---|
| `TELEGRAM_BOT_TOKEN` | The token from BotFather (from the Telegram setup guide) |
| `TELEGRAM_CHAT_ID` | Your personal Telegram user ID (see "Finding Your Telegram Chat ID" below) |
| `EDGE_AUTH_SECRET` | A random string for auth - generate one by running `openssl rand -hex 32` in your terminal |

Save that `EDGE_AUTH_SECRET` value somewhere - you'll need it in Step 7.

---

## Step 4: Enable the Postgres HTTP Extension

In the Supabase dashboard, go to **SQL Editor** and run:

```sql
CREATE EXTENSION IF NOT EXISTS http WITH SCHEMA extensions;
```

This lets the agent send HTTP requests through SQL - the workaround for the cloud environment's network restrictions.

---

## Step 5: Test the Edge Function

Still in the SQL Editor, run this (replacing the two placeholder values):

```sql
SELECT status, content::text AS body
FROM extensions.http((
  'POST',
  'https://YOUR_PROJECT_ID.supabase.co/functions/v1/send-telegram',
  ARRAY[
    extensions.http_header('Content-Type', 'application/json'),
    extensions.http_header('X-Edge-Auth', 'YOUR_EDGE_AUTH_SECRET')
  ],
  'application/json',
  '{"message": "Test - if you see this in Telegram, the setup is working."}'
)::extensions.http_request);
```

You should get a row back with `status: 200` and see the message pop up in Telegram on your phone. If not, check your secrets are correct and the function deployed without errors.

---

## Step 6: Connect MCP Connectors

In [claude.ai](https://claude.ai), go to Settings > MCP Connectors and connect:

- **Microsoft 365** - this gives the agent access to your Outlook email and calendar
- **Supabase** - this gives the agent access to your edge function via SQL

Follow the prompts for each. You'll need to authorise with your Microsoft account and your Supabase account.

---

## Step 7: Create the Scheduled Agent

You can create a scheduled trigger via `claude.ai/code/scheduled` or through the Claude Code CLI. Either way, you need two things: a schedule and a prompt.

**Schedule:** Every hour on weekdays, e.g. `0 * * * 1-5` (cron format - runs at the top of every hour, Monday to Friday).

**Prompt template** (replace the three placeholders with your actual values):

```
You are a scheduled assistant. You have two jobs: email alerts and meeting prep.

## CRITICAL: How to Send Telegram Messages

The cloud environment blocks direct HTTP calls. You MUST send Telegram messages
via Supabase SQL using the Postgres HTTP extension. This is the ONLY method that works.

Use the Supabase MCP to run this SQL pattern:

SELECT status, content::text AS body
FROM extensions.http((
  'POST',
  'https://YOUR_SUPABASE_URL/functions/v1/send-telegram',
  ARRAY[
    extensions.http_header('Content-Type', 'application/json'),
    extensions.http_header('X-Edge-Auth', 'YOUR_AUTH_SECRET')
  ],
  'application/json',
  '{"message": "YOUR MESSAGE TEXT HERE"}'
)::extensions.http_request);

If the status is not 200, log the error and move on. Do not retry more than once.

## Job 1: Email Scan

1. Read CLIENTS.md and the prospects/ folder from the repo. Build a list of
   important names and email domains.
2. Use the Microsoft 365 MCP to search Outlook for emails received in the last
   2 hours from anyone matching those names or domains.
3. For each email found, check Sent Items to see if there is already a reply.
   Skip any email that has been replied to.
4. For unreplied emails, send ONE Telegram message summarising all of them.
   Format:
   - Who it is from and the subject
   - One-line summary of what they want
   - A suggested reply angle (one sentence)
   Keep it under 5 lines per email. If there are no unreplied emails, send nothing.

## Job 2: Meeting Prep

1. Use the Microsoft 365 MCP to check the calendar for events starting in the
   next 60-90 minutes.
2. For each upcoming meeting, extract the attendee names and email domains.
3. Search the repo files for any mentions of those people or their companies:
   CLIENTS.md, PROSPECTS.md, prospects/ folder, WAITING_FOR.md, DECISIONS.md.
4. Send ONE Telegram message per meeting with a prep brief:
   - Meeting title and time
   - Who you are meeting
   - Key context (what you last discussed, what is pending, any open items)
   - Any suggested talking points
   Keep it concise - aim for 5-8 lines maximum.
   If there are no meetings in the window, send nothing.

## Rules

- If both jobs produce nothing, do not send any Telegram messages. Silence is fine.
- Never send more than 3 Telegram messages in a single run.
- If something fails (MCP error, no access), skip that job silently. Do not
  send error messages to Telegram.
- Keep all Telegram messages short. This is a phone notification, not a report.
```

---

## Step 8: Test It

To verify everything works end to end:

1. Go to your scheduled agents (claude.ai/code/scheduled or `claude schedule list` in the CLI)
2. Manually trigger a run
3. Check your Telegram - you should receive messages if you have unreplied emails or upcoming meetings
4. If you hear nothing and you know you have unreplied emails, check the run logs for errors

Once you're happy it works, leave it running. It'll quietly check every hour and only bother you when something needs your attention.

---

## Finding Your Telegram Chat ID

1. Open Telegram on your phone
2. Search for **@userinfobot** and start a chat
3. Send any message (even just "hi")
4. It replies with your user ID - a number like `123456789`
5. That number is your `TELEGRAM_CHAT_ID`

---

## Troubleshooting

**Agent not sending Telegram messages:**
- Check your edge function secrets in Supabase (TELEGRAM_BOT_TOKEN, TELEGRAM_CHAT_ID, EDGE_AUTH_SECRET)
- Check that the `X-Edge-Auth` value in the prompt matches the `EDGE_AUTH_SECRET` in Supabase
- Run the test SQL from Step 5 manually to isolate whether the problem is the function or the agent

**No email alerts even though you have unreplied emails:**
- Check that CLIENTS.md has recognisable names or email domains for your contacts
- Check the Microsoft 365 MCP connector is connected and authorised in claude.ai
- The agent only looks back 2 hours - older unreplied emails won't trigger alerts

**No meeting prep briefs:**
- Check the calendar MCP connector is connected
- The agent only looks 60-90 minutes ahead - it won't prep you for meetings later in the day
- Check that attendee names or companies appear somewhere in your Chief of Staff files

**Edge function returning 401:**
- The `X-Edge-Auth` header in the SQL doesn't match the `EDGE_AUTH_SECRET` in Supabase secrets
- Secrets are case-sensitive - check for trailing spaces

**"extensions.http" not found in SQL:**
- You haven't enabled the HTTP extension yet - run the SQL from Step 4

---

## Want Help Going Further?

This guide covers email alerts and meeting prep. There's also an optional [content scanning add-on](content-scanning-setup.md) for monitoring YouTube channels, newsletters and RSS feeds.

If you want help customising this for your team, integrating with your specific tools, or building something more ambitious - that's what [AgentFlow](https://goagentflow.com) does. We help businesses figure out where AI actually fits and build the things that make a difference. [Get in touch](mailto:hamish@goagentflow.com).
