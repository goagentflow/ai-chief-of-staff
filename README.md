# AI Chief of Staff

A system for using Claude Code as your persistent AI chief of staff and executive assistant.

## What This Is

This template provides a folder structure and prompt framework for Claude Code to act as your external memory and executive assistant. It can:

- **Track everything**: clients, projects, prospects, decisions, follow-ups
- **Write in your voice**: drafts emails, LinkedIn posts, documents in your style
- **Maintain context**: uses markdown files as persistent memory between sessions
- **Run daily routines**: morning check-ins, standup processing, end-of-day reconciliation
- **Integrate with Microsoft 365**: calendar, email, Teams transcripts, files (via MCP server)

The system evolves with use. Start simple, add what you need.

---

## Quick Start

### 1. Clone This Repo

```bash
git clone https://github.com/goagentflow/ai-chief-of-staff.git ~/Desktop/chief-of-staff
cd ~/Desktop/chief-of-staff
```

### 2. Run the Guided Setup

Open Claude Code in this folder and say:

```
Let's set up my AI Chief of Staff
```

Claude will guide you through personalisation:
- Who you are and how you work
- Analysing your writing samples to create a tone-of-voice skill
- Setting up permissions
- Configuring optional systems (expenses, recurring tasks)

### 3. Start Using It

After setup, start with:

```
Morning check-in
```

Claude will walk you through your day.

---

## What's Included

### Core Files

| File | Purpose |
|------|---------|
| `CLAUDE.md` | Stable context - who you are, style guide, permissions (read when drafting) |
| `CLAUDE-ROUTINES.md` | Daily operations - what Claude reads every morning |
| `INBOX.md` | Quick capture for unprocessed items |
| `PROJECTS.md` | Active projects with Quick Status table + full details |
| `WAITING_FOR.md` | Things blocked on others (who, what, when to chase) |
| `DECISIONS.md` | Decisions with full reasoning (your external memory for "why") |
| `CLIENTS.md` | Client contacts with Quick Status table + relationship history |
| `PROSPECTS.md` | Sales pipeline summary |
| `IDEAS.md` | Sparks and thoughts that need to marinate |
| `IDEAS-ARCHIVE.md` | Ideas that were acted on or dismissed |

### Folders

| Folder | Purpose |
|--------|---------|
| `prospects/` | Detailed files for each prospect |
| `work-orders/` | Specific briefs or work packages |
| `skills/` | Reusable capability packages (tone-of-voice, etc.) |
| `setup/` | Guided setup prompts |
| `office-365-mcp-server/` | Microsoft 365 integration (optional) |

---

## Key Principles

### 1. Files ARE the Memory

Claude has no memory between sessions. Everything important must be written to files. If it's not in a file, it doesn't exist.

### 2. Update as You Go

Don't batch updates. Claude updates files immediately when relevant information comes up. After every client call, after every decision, after every email - the files get updated.

### 3. CLAUDE.md is the Instruction Manual

Keep it stable. Update rarely but keep it accurate. This is where Claude learns who you are and how you work.

### 4. Context Window Efficiency

The system is designed to minimize what Claude reads for routine operations:

- **Quick Status tables** at the top of PROJECTS.md and CLIENTS.md - Claude scans these every morning instead of reading full details
- **CLAUDE-ROUTINES.md** contains just the operational stuff - what Claude needs to run your day
- **CLAUDE.md** contains stable context (who you are, style guide, permissions) - read only when drafting content or working with specific people
- **Two-layer architecture** - summary at top, details below. Update both when you update anything.

This reduces morning check-in context from ~60KB to ~15KB (75% reduction).

### 5. The System Evolves

Start simple. Add complexity only when you need it. The best version of this system is the one you actually use.

---

## Daily Routines

### Morning Check-in

Say: *"Morning check-in"* or *"What's on for today?"*

Claude will:
- Pull your calendar
- Check for overdue follow-ups
- Review active projects
- Scan for expenses to log
- Surface anything needing attention
- Help you prioritise

### Standup Processing

After your daily standup, say: *"Process the standup"*

Claude will:
- Fetch the meeting transcript
- Extract key items (actions, decisions, ideas, follow-ups)
- Propose where each item belongs in the system
- Update files after your confirmation

### End of Day

Say: *"End of day"* or *"Let's reconcile"*

Claude will:
- Review what got done
- Capture loose threads
- Update waiting-for items
- Set up tomorrow's priorities

---

## The Skills System

Skills are reusable capability packages. The most important one is your **tone-of-voice skill**, created during setup from your writing samples.

When Claude drafts content, it reads your skill file and writes in your voice - not generic AI-speak.

You can create additional skills for:
- Presentation style (brand colours, templates)
- Proposal writing
- Code review standards
- Anything you do repeatedly

See `skills/README.md` for details.

---

## Microsoft 365 Integration

For calendar, email, Teams and file access:

1. Copy `office-365-mcp-server/` to your home directory
2. Follow `office-365-mcp-server/SETUP.md`
3. Add the MCP server to Claude Code settings

This enables:
- Reading and creating calendar events
- Searching and reading emails
- Creating email drafts
- Fetching Teams meeting transcripts
- Accessing OneDrive/SharePoint files

---

## Two-Layer Prospect Management

For sales pipelines, there's a two-layer system:

- **PROSPECTS.md**: Quick summary with status table and brief context per prospect
- **prospects/[name].md**: Full detail - relationship history, needs, commercial context

This scales better than cramming everything into one file.

---

## Technical References

When you figure something out the hard way (API quirks, workarounds, non-obvious processes), Claude creates `REFERENCE-*.md` files automatically.

These save time on repeated tasks and capture institutional knowledge.

---

## File Structure

```
chief-of-staff/
├── CLAUDE.md                    # Core instructions - who you are, style guide, permissions
├── CLAUDE-ROUTINES.md           # Daily operations - what Claude reads every morning
├── INBOX.md                     # Quick capture
├── PROJECTS.md                  # Active projects (Quick Status table + full details)
├── WAITING_FOR.md               # Blocked items
├── DECISIONS.md                 # Decisions with reasoning
├── CLIENTS.md                   # Client reference (Quick Status table + full details)
├── PROSPECTS.md                 # Sales pipeline summary
├── IDEAS.md                     # Ideas to marinate
├── IDEAS-ARCHIVE.md             # Acted/dismissed ideas
├── REFERENCE-TEMPLATE.md        # Template for technical references
├── SETUP.md                     # Setup entry point
├── prospects/
│   └── TEMPLATE.md              # Prospect detail template
├── work-orders/
│   └── TEMPLATE-work-order.md   # Work order template
├── skills/
│   ├── README.md                # How skills work
│   └── tone-of-voice/
│       └── SKILL.md             # Your writing style (generated during setup)
├── setup/
│   └── setup-prompt.md          # Guided setup flow
└── office-365-mcp-server/       # Microsoft 365 MCP (optional)
```

---

## About the Creator

**Hi, I'm Hamish Nicklin, CEO of [AgentFlow](https://goagentflow.com).**

I built this system because my brain is excellent at problem-solving and creativity but terrible at remembering what I decided and why. After 25 years in media and advertising (Google, The Guardian, Dentsu), I co-founded AgentFlow to help businesses actually use AI - not just talk about it.

This AI Chief of Staff is what I use every day. It runs my morning check-ins, processes my standup transcripts, drafts my emails, and keeps track of everything I'd otherwise forget. It's not theoretical - it evolved through real daily use.

**Why give it away free?** Because the best way to understand AI isn't reading about it - it's using it. If this system helps you see what's possible, maybe one day you'll want help building something custom for your business. That's what AgentFlow does.

### What If I Don't Have Prospects/Clients?

This template includes files for clients, prospects, and sales pipelines because that's what I need. You don't have to use all of it.

**Use what fits your workflow:**
- **Freelancer/consultant?** CLIENTS.md and PROJECTS.md are your core
- **Internal role?** PROJECTS.md and WAITING_FOR.md might be all you need
- **Solo founder?** IDEAS.md and DECISIONS.md could be most valuable
- **Creative work?** The tone-of-voice skill might be your killer feature

Delete the files you don't need. Add new ones for what you do need. The system adapts - that's the point.

### Get in Touch

- **Website:** [goagentflow.com](https://goagentflow.com)
- **LinkedIn:** [Hamish Nicklin](https://www.linkedin.com/in/hamishnicklin/)
- **Email:** hamish@goagentflow.com

If you build something interesting with this, I'd love to hear about it.

---

## Credits

Built by [AgentFlow](https://goagentflow.com) - AI consulting and implementation.

This system evolved through daily use. The best features came from actual problems, not theoretical design.

---

## Contributing

Issues and pull requests welcome. If you've evolved improvements in your own setup, share them back.
