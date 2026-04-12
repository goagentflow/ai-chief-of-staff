# Wiki Memory Setup

*How to give your Chief of Staff a memory that builds over time.*

---

## What This Does

Your Chief of Staff has no memory between sessions. Every conversation starts blank. The wiki fixes that - it's a folder of markdown files that your Chief of Staff reads at the start of each session and updates as you work together.

After a week, it knows who the key people are. After a month, it has genuine institutional memory - your decisions, your preferences, your relationships, your context. You stop repeating yourself and start building on what came before.

**Cost:** Zero. It's just markdown files in your repo.

**Time to set up:** About 5 minutes.

---

## How It Works

- A `wiki/` folder in your repo with simple markdown files
- `index.md` acts as a table of contents - one line per page, so your Chief of Staff can find things quickly
- `hot.md` holds current priorities and urgent context (kept short - read every session)
- Subfolders organise pages by type: people, organisations, decisions, feedback, reference material
- Your Chief of Staff creates and updates pages as you work together - you don't need to write them yourself

---

## Step 1: Create the Folder Structure

From your repo root:

```bash
mkdir -p wiki/people wiki/orgs wiki/decisions wiki/feedback wiki/reference
```

This gives you:

```
wiki/
├── claude.md          # Operating instructions for the wiki
├── index.md           # Table of contents - one line per page
├── hot.md             # Current priorities (read every session)
├── people/            # Key people you work with
├── orgs/              # Organisations and companies
├── decisions/         # Important decisions with reasoning
├── feedback/          # What works, what doesn't in your collaboration
└── reference/         # Technical references, how-tos, workarounds
```

---

## Step 2: Create the Wiki Instructions

Create `wiki/claude.md` with the following. This tells your Chief of Staff how to use the wiki:

```markdown
# Wiki Operating Instructions

This wiki is your long-term memory. Read it at the start of every session. Update it as you learn new things.

## Rules

1. **Search before creating.** Always check index.md before making a new page. Never create duplicates.
2. **Update, don't duplicate.** If a page already exists for a person, org or topic, update it rather than creating a new one.
3. **Keep hot.md short.** Under 500 tokens. Only current priorities and urgent context. Move things out when they're no longer hot.
4. **Date-stamp everything.** Every update gets today's date so we can track when things were last touched.
5. **Use frontmatter.** Every page starts with a title and last-updated date.

## When to Create a New Page

- You learn about a new person who matters (client, colleague, partner)
- A new organisation comes into play (prospect, client, supplier)
- An important decision is made (capture the reasoning, not just the outcome)
- You figure something out the hard way (save it in reference/ so you don't repeat the pain)
- You get feedback on how to work better together (save it in feedback/)

## When to Update an Existing Page

- New information about someone or something already in the wiki
- A decision changes or evolves
- Context shifts on a current priority

## Page Format

Every wiki page should look like this:

    ---
    title: Page Title
    last-updated: 2026-01-15
    ---

    Content here. Keep it concise and useful.
```

---

## Step 3: Create the Index

Create `wiki/index.md`:

```markdown
# Wiki Index

Master table of contents. One line per page. Search here before creating anything new.

## Hot Context
- [Hot](hot.md) - Current priorities and urgent context

## People

## Organisations

## Decisions

## Feedback

## Reference
```

Pages get added here as they're created. After a few weeks it might look like:

```markdown
## People
- [Sarah Chen](people/sarah-chen.md) - CTO at Meridian, main technical contact
- [James Wright](people/james-wright.md) - CEO at Northfield, introduced via LinkedIn

## Organisations
- [Meridian Digital](orgs/meridian-digital.md) - Active client, AI strategy project
- [Northfield Partners](orgs/northfield-partners.md) - Prospect, initial conversations
```

---

## Step 4: Create the Hot Context File

Create `wiki/hot.md`:

```markdown
---
title: Hot Context
last-updated: 2026-01-15
---

# Current Priorities

1. [Your top priority right now]
2. [Second priority]
3. [Third priority]

# Urgent Context

[Anything your Chief of Staff needs to know right now - a big meeting coming up, a deadline, a sensitive situation. Remove items when they're no longer urgent.]
```

Keep this file short. Your Chief of Staff reads it every session, so anything here costs attention. If something isn't actively urgent, it belongs on a wiki page instead.

---

## Step 5: Add to Your CLAUDE.md

Add this section to your `CLAUDE.md` so your Chief of Staff knows to use the wiki:

```markdown
## Memory (Wiki)

All long-term memory lives in `wiki/`. Read `wiki/claude.md` for operating instructions.

- `wiki/hot.md` - current priorities, urgent context (~500 tokens). Read every session.
- `wiki/index.md` - full table of contents with one-line summaries. Search before creating.
- Wiki pages in: people/, orgs/, decisions/, feedback/, reference/

Do NOT create separate memory files outside the wiki. Everything goes in the wiki.

When you learn something new about a person, organisation, decision or process, create or update the relevant wiki page. Always search index.md first - never duplicate.
```

Also add the wiki to your Claude Code memory file (`~/.claude/CLAUDE.md` or your project's memory settings):

```markdown
All memory lives in the wiki at [your-repo-path]/wiki/.
Read wiki/index.md for the full table of contents.
Read wiki/hot.md for current priorities and hot context.
Read wiki/claude.md for operating instructions.
```

This ensures your Chief of Staff checks the wiki even in quick sessions.

---

## How It Builds Over Time

You don't need to populate the wiki manually. It builds through normal use:

- **Day 1:** You mention a client by name. Your Chief of Staff creates a page in `people/` and adds them to the index.
- **Week 1:** After a few sessions, the wiki knows your key people, your active projects and your current priorities. You stop explaining who Sarah is every time.
- **Week 2:** Decisions start accumulating with reasoning attached. When you ask "why did we decide X?", the answer is there.
- **Month 1:** You have genuine institutional memory. Your Chief of Staff knows your preferences, your relationships, your patterns. It catches things you've forgotten and connects dots across sessions.
- **Month 3:** New topics automatically link to existing context. Your Chief of Staff spots when a new prospect is connected to someone it already knows about.

The only maintenance you need to do is occasionally reviewing `hot.md` to make sure it reflects what's actually urgent. Everything else happens in the background as you work.

---

## Tips

**Keep hot.md ruthlessly short.** It's read every session. If it grows beyond 500 tokens, move things to proper wiki pages and just reference them.

**Don't worry about perfection.** A messy wiki that gets used beats a perfect one that doesn't. Your Chief of Staff will tidy pages as it updates them.

**Review the index occasionally.** During a weekly review or end-of-day session, glance at `wiki/index.md`. You'll spot gaps and outdated pages quickly.

**Use feedback/ actively.** When your Chief of Staff does something you like or don't like, tell it to save that in `wiki/feedback/`. This is how it learns your preferences across sessions.

---

## Troubleshooting

**Wiki not being read:**
- Check your CLAUDE.md includes the Memory section above
- Check your memory file references the wiki path
- Make sure `wiki/claude.md` and `wiki/index.md` exist

**Pages getting duplicated:**
- Reinforce in CLAUDE.md: "Always search index.md before creating a new page"
- Check that index.md is being kept up to date (every new page should be listed)

**hot.md getting too long:**
- Move anything that isn't actively urgent to a proper wiki page
- hot.md should be a signpost, not a document - point to pages rather than containing full context

---

## Want Help Going Further?

This guide gets you the basics. If you want help customising this for your team, integrating with your specific tools, or building something more ambitious - that's what [AgentFlow](https://goagentflow.com) does. We help businesses figure out where AI actually fits and build the things that make a difference. [Get in touch](mailto:hamish@goagentflow.com).
