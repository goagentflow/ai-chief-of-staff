# Setting Up Your AI Chief of Staff

This guide walks you through personalising your AI Chief of Staff system. The setup takes about 15-20 minutes and creates a system tailored to how you work.

---

## Before You Start

**Requirements:**
- Claude Code (the CLI tool) installed
- This repo cloned to your local machine
- 3-5 examples of things you've written (emails, LinkedIn posts, reports - anything in your natural voice)

---

## Quick Start

1. Open Claude Code in this directory
2. Say: **"Let's set up my AI Chief of Staff"**
3. Claude will guide you through the setup process

That's it. Claude knows to use the setup prompt in `setup/setup-prompt.md` and will walk you through each step.

---

## What the Setup Covers

### Step 1: Who Are You?
- Your name, role, company
- What your day-to-day looks like
- Key people you work with

### Step 2: Writing Style Analysis
Claude will ask for writing samples and analyse your natural style:
- Sentence structure (long flowing vs short punchy)
- Punctuation preferences
- Tone and formality
- Signature patterns
- Things you avoid

This creates a personalised tone-of-voice skill so Claude writes like you.

### Step 3: Permissions
What Claude can do without asking, what needs approval, and what's off-limits.

### Step 4: Optional Systems
- Expense tracking (if you want it)
- Recurring monthly tasks
- Microsoft 365 integration (calendar, email, Teams) - guided setup via skill

---

## After Setup

Once setup completes, you'll have:

1. **CLAUDE.md** - Personalised with your context, style and permissions
2. **skills/tone-of-voice/SKILL.md** - Your unique writing style rules
3. **All template files** - Ready to use with your first entries

Start with: **"Morning check-in"** to begin your first daily routine.

---

## Manual Setup (Alternative)

If you prefer to set things up manually:

1. Edit `CLAUDE.md` directly with your information
2. Create your tone-of-voice skill manually in `skills/tone-of-voice/SKILL.md`
3. Read through each template file and customise as needed

The guided setup is recommended because it ensures nothing is missed and creates the tone-of-voice skill automatically from your writing samples.

---

## Re-running Setup

To re-run setup later (e.g., to update your writing style):

Say: **"Let's redo the setup"** or **"Update my tone of voice"**

Claude will know what to do.
