# Content Scanning Setup (Optional)

*Automated monitoring of YouTube channels, newsletters and RSS feeds - filtered ruthlessly so only the good stuff gets through.*

---

## What This Does

Adds content scanning to your scheduled agent. It checks your curated sources every hour, applies an analysis framework tuned to YOUR work, and only pings you on Telegram when something is genuinely worth your time. Once a day it compiles everything into a digest - an Outlook draft with the full summary and a calendar reading slot with the content right there in the event body.

Most scans find nothing worth alerting you about. That's correct behaviour. Silence means the filter is working.

**Prerequisites:** This guide assumes you've already completed the scheduled agents setup (email alerts and meeting prep are working).

**What you need (on top of the core setup):**
- Gmail MCP connector connected in claude.ai (for newsletter scanning)
- A Supadata API account at supadata.ai ($19/month Pro plan) for YouTube transcript access
- YouTube channels and/or newsletter sources you want to monitor

---

## The Analysis Framework

This is the brain of the scanner. It decides what's worth your time and what gets filtered out.

Every piece of content gets evaluated through lenses specific to YOUR work, plus a universal bullshit filter. You define the lenses. The bullshit filter stays the same for everyone.

### Lens 1: Your Workflow

Map content against your actual working steps. If a piece of content improves any step, it gets flagged. Here's a default set - edit these to match what you actually do:

1. **Research** - understanding your market, clients, competitors
2. **Outreach** - finding people, writing messages, following up
3. **Meetings** - running conversations, asking the right questions
4. **Synthesise** - turning messy inputs into structured outputs
5. **Propose** - writing proposals, building presentations, pricing
6. **Deliver** - managing projects, reviewing work, building
7. **Account manage** - staying close to clients, spotting opportunities

These should reflect YOUR workflow, not a generic business process. If you spend half your time doing product development and never write proposals, change the list accordingly.

### Lens 2: Business Functions

What business functions does your work touch? The scanner uses these to spot content relevant to the problems you solve for clients (or your own team). Default list:

- Marketing and creative
- Sales and business development
- Customer service and support
- Operations and supply chain
- Finance and reporting
- HR and recruitment
- Product and engineering
- Legal and compliance
- Data and analytics
- Strategy and planning

Again, trim this to what you actually care about. If you're in HR tech, you probably don't need legal and compliance in your scan.

### The Bullshit Filter (Universal - Don't Change These)

Before anything makes it into your digest, it must pass all four:

1. **Is this a course promo dressed as content?** Extract the real technique if there is one, skip the sales wrapper. If there's no substance underneath, skip entirely.
2. **Is someone overcomplicating something simple?** Skip or call it out. If the "AI solution" is basically a spreadsheet macro with extra steps, say so.
3. **Is this genuinely interesting?** Keep. Explain what it actually does and what's new.
4. **Could you reference this credibly in a meeting?** That's the bar. Not "here's a link" but "I understand this well enough to explain it in 30 seconds."

### Tags

Each item that passes the filter gets one or more tags:

- **TRY IT** - directly useful for your workflow. Something you could use this week.
- **CLIENT ANGLE** - relevant to business functions you serve. Capability to be aware of.
- **CONTENT** - material for your own content (posts, talks, newsletters).
- **THOUGHT SEED** - provocative idea that might connect to something later. Value is in the provocation, not the action.
- **INCOMING** - mainstream stories people will ask you about. Things that have broken out of the tech bubble.
- **BACKGROUND** - pure awareness. Lowest tier.

### Customising the Framework

If you'd rather not write this by hand, use this prompt with Claude:

"I want to set up my content scanning framework. My role is [describe your role]. My typical workflow steps are [list your daily activities]. The business functions I care about are [list them]. Please generate a personalised analysis framework for my SKILL.md file."

---

## Step-by-step Setup

### Step 1: Deploy the get-transcript Edge Function

This function lives in your Supabase project. It calls the Supadata API to fetch YouTube transcripts so your scheduled agent can read what's actually in a video rather than guessing from the title.

Deploy this as a Supabase Edge Function called `get-transcript`:

```typescript
import { serve } from "https://deno.land/std@0.168.0/http/server.ts";

serve(async (req) => {
  // Auth check - use a shared secret so only your agent can call this
  const authHeader = req.headers.get("X-Edge-Auth");
  if (authHeader !== Deno.env.get("EDGE_AUTH_SECRET")) {
    return new Response("Unauthorised", { status: 401 });
  }

  const { video_id } = await req.json();
  if (!video_id) {
    return new Response(JSON.stringify({ error: "video_id required" }), {
      status: 400,
      headers: { "Content-Type": "application/json" },
    });
  }

  const apiKey = Deno.env.get("SUPADATA_API_KEY");
  const response = await fetch(
    `https://api.supadata.ai/v1/transcript?url=https://youtu.be/${video_id}`,
    { headers: { "x-api-key": apiKey! } }
  );

  const data = await response.json();
  return new Response(JSON.stringify(data), {
    headers: { "Content-Type": "application/json" },
  });
});
```

Deploy it:
```bash
supabase functions deploy get-transcript
```

### Step 2: Add Supabase Secrets

You need two secrets:

```bash
supabase secrets set SUPADATA_API_KEY=your_supadata_api_key_here
supabase secrets set EDGE_AUTH_SECRET=any_random_string_you_choose
```

Get your Supadata API key from your account at supadata.ai after signing up for the Pro plan.

The `EDGE_AUTH_SECRET` is any string you choose - it just needs to match what your agent sends when calling the function.

### Step 3: Create Your Source Lists

Create `skills/youtube-monitor/channels.json` in your repo:

```json
{
  "channels": [
    {"id": "UCxxxxxxxxxxxxxxxxxxxxxx", "name": "Channel Name", "tier": 1},
    {"id": "UCyyyyyyyyyyyyyyyyyyyyyy", "name": "Another Channel", "tier": 2}
  ]
}
```

**Tier 1** = primary sources (AI, tech, your core domain). **Tier 2** = secondary (business strategy, adjacent topics).

**Finding a YouTube channel ID:** Go to the channel page, view page source (Ctrl+U / Cmd+U), and search for `channelId`. It starts with `UC` and is 24 characters. Alternatively, use a channel ID lookup tool - there are several free ones online.

Create `skills/youtube-monitor/newsletters.json`:

```json
{
  "newsletters": [
    {
      "sender": "newsletter@example.com",
      "name": "Example Newsletter",
      "priority": "high",
      "category": "ai_tools",
      "filter": null
    }
  ],
  "rss_sources": [
    {
      "url": "https://example.com/rss.xml",
      "name": "Example RSS",
      "filter": "topic_only"
    }
  ]
}
```

**Priority levels:** critical, high, medium, low. These help the scanner decide what to read first when there's a lot of content.

**Filter options:** `null` (keep everything), `"ai_only"` (only AI-related content), `"content_only"` (skip community notifications and admin emails).

### Step 4: Create the Analysis Framework File

Save your personalised framework to `skills/youtube-monitor/SKILL.md`. Structure it with:

- Your workflow steps (Lens 1) and business functions (Lens 2) from above
- The bullshit filter (copy it verbatim - don't soften it)
- The tag definitions
- A "What to Skip" section (course promos, motivational fluff, duplicates)
- A digest item format: what it is (one line), why YOU care, the bit to watch/read (timestamp or section), what you'd do with it, and tag(s)
- A "Go Deeper" section explaining that when you say "go deeper on #N", the agent pulls the full transcript or email body, gives you the substance, extracts specifics, applies to your context, and calls out the bullshit

### Step 5: Update the Scheduled Trigger Prompt

Add the following to your existing scheduled agent prompt (the one from the scheduled agents setup). This goes after your existing email and calendar scanning sections:

```
## Content Scanning (every run)

Read skills/youtube-monitor/SKILL.md for the full analysis framework.
Read skills/youtube-monitor/channels.json for YouTube sources.
Read skills/youtube-monitor/newsletters.json for newsletter and RSS sources.

### YouTube Scanning

For each channel in channels.json, check the YouTube RSS feed:
https://www.youtube.com/feeds/videos.xml?channel_id=CHANNEL_ID

Look for videos published since the last scan. For any new video that looks
relevant based on title and description, fetch the transcript via the
Supabase edge function:

SELECT content FROM http_post(
  'https://YOUR_PROJECT.supabase.co/functions/v1/get-transcript',
  '{"video_id": "VIDEO_ID_HERE"}',
  'application/json',
  '{"X-Edge-Auth": "YOUR_EDGE_AUTH_SECRET"}'
);

Never flag a video without reading the transcript first. If the transcript
is unavailable, say so honestly rather than guessing.

### Newsletter Scanning

Use Gmail MCP to search for emails from each sender in newsletters.json
received since the last scan. Read the email body and apply the analysis
framework.

### RSS Scanning

Fetch each RSS URL in newsletters.json rss_sources via Supabase SQL:

SELECT content FROM http_get('RSS_URL_HERE');

Parse the XML for recent items and apply any filter specified.

### After Scanning

Apply the analysis framework from SKILL.md to all items.
If anything passes the bullshit filter, send a batched Telegram alert:

[emoji] [Source] - [Title]
[One line: why YOU care]
[Link]

Reply "go deeper on 1", "2" etc.

If nothing passes, stay silent. Silence is correct.

Log all scanned items (passed or not) to memory/daily-intel/raw/YYYY-MM-DD.jsonl
as one JSON object per line:
{"time": "HH:MM", "source": "...", "title": "...", "url": "...", "passed": true/false, "tags": [...], "summary": "..."}
```

### Step 6: Add the Daily Digest Job

Add this to your scheduled agent prompt as a separate section. Set it to run at whatever time suits your day (lunchtime works well - gives you morning content to compile):

```
## Daily Digest (runs at [YOUR CHOSEN TIME])

Read all entries from memory/daily-intel/raw/YYYY-MM-DD.jsonl (today's date).

If there are items that passed the filter:

1. Create an Outlook draft:
   - To: your email address
   - Subject: "AI Intel Digest - [DATE]"
   - Body: HTML-formatted digest with all items, organised by tag
   - Include clickable links

2. Create a calendar event (NOT a Teams meeting):
   - Find a free 30-minute slot (check your preferred time window first, expand if needed)
   - Title: "Reading: AI Intel Digest"
   - Body: the full digest with clickable links (not a blank placeholder)

3. Send Telegram summary:
   - Top picks only (0-8 items)
   - Zero items on a quiet day is fine

4. Save to memory/daily-intel/digests/YYYY-MM-DD.md

If no items passed the filter today, skip the digest entirely. Don't create
an empty one.
```

---

## Starting Small

Don't try to monitor 40 channels on day one. Start with 5-10 YouTube channels and 3-5 newsletters that you already read (or wish you had time to read). Run it for a week. See what passes the filter and what doesn't.

If everything passes, your framework is too loose - tighten the bullshit filter criteria or narrow your workflow steps.

If nothing passes for several days, either your sources aren't publishing much (normal) or your framework is too strict. Check the JSONL log to see what got filtered and why.

Add more sources to `channels.json` and `newsletters.json` as you calibrate. The agent reads them fresh every run, so changes take effect immediately.

---

## Supadata Credits

The Pro plan gives you 3,000 credits per month. Each transcript costs 1 credit.

Budget accordingly: if you monitor 30 channels and each publishes twice a week, that's roughly 240 transcripts per month - well within budget. If you monitor 100 prolific channels, you might run out.

The scanner only fetches transcripts for videos that pass the initial title and description filter, so most videos won't cost a credit.

---

## Troubleshooting

**Transcripts coming back empty:**
- Some videos don't have captions (live streams, very new uploads). The scanner should note this honestly rather than skipping silently.
- Check your Supadata API key is correct and your account is active.

**Too many alerts:**
- Tighten your analysis framework. The bullshit filter question "could you reference this credibly in a meeting?" should be doing most of the heavy lifting.
- Remove lower-tier sources or move them to tier 2.

**No alerts for days:**
- Check the JSONL log (`memory/daily-intel/raw/`) to see what's being scanned and filtered. If items are being scanned but nothing passes, your framework might be too strict.
- If nothing is being scanned at all, check your channel IDs and newsletter sender addresses are correct.

**Gmail MCP not finding newsletters:**
- Make sure the sender addresses in `newsletters.json` exactly match what appears in Gmail. Check the "From" field in a recent email from that source.
- Some newsletters use different sending addresses for different editions.

---

## Want Help Going Further?

Content scanning is one of the more advanced capabilities. If you want help tuning your framework, integrating with additional sources, or building custom AI workflows for your business - that's what [AgentFlow](https://goagentflow.com) does. We help businesses figure out where AI actually fits and build the things that make a difference. [Get in touch](mailto:hamish@goagentflow.com).
