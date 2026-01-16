# CLAUDE-ROUTINES.md - Daily Operations

*This file contains what Claude needs to run your day. For full context (who you are, style guide, permissions), see CLAUDE.md.*

*Last updated: [DATE]*

---

## Quick Morning Read List

For morning check-in, Claude reads these in this order:

| File | What to Scan |
|------|-------------|
| **This file** | Operations and routines |
| **PROJECTS.md** | Quick Status table at top only |
| **CLIENTS.md** | Quick Status table at top only |
| **WAITING_FOR.md** | Full file (compact) |
| **PROSPECTS.md** | Full file (summary) |
| **INBOX.md** | Full file (compact) |

Claude only reads full details (CLAUDE.md, full project entries, client history) when:
- Drafting content (need style guide)
- Working on a specific project/client
- You ask for it

---

## After Updating Project/Client Status

When Claude updates any project or client details, it must also update the Quick Status row at the top of that file. This keeps the summary current without needing separate files.

---

## Active Project Folders

These are your active builds. Check them during morning routine to get actual project state.

| Project | Path | What to Check |
|---------|------|---------------|
| [Project Name] | `[/path/to/project/]` | README.md, PROGRESS.md |
| [Project Name] | `[/path/to/project/]` | [Status files] |

**Status indicators:**
- Last Updated dates
- Roadmap/checklist progress
- "What's working" vs "In Progress" sections

---

## Daily Routines

### Morning Check-in (5-10 minutes)

Start your day by saying: *"Morning check-in"* or *"What's on for today?"*

Claude will:

1. **If no EOD check-in yesterday** - Start by asking: "What did you get done yesterday?" (so Claude has context before planning today)
2. **Pull your calendar** for the day - meetings, calls, deadlines
3. **Check meeting transcription** - For any meetings today, remind: "Check that [meeting name] has Record & Transcribe enabled in meeting options."
4. **Review INBOX.md** - anything urgent captured yesterday that needs attention
5. **Scan PROJECTS.md Quick Status** - surface the strategic work and active workstreams
6. **Check DECISIONS.md** - anything needing a call today?
7. **Check WAITING_FOR.md** - anything due back from others today
8. **Review IDEAS.md** - anything captured recently worth revisiting?
9. **Scan recent emails** - flag anything needing response before your first meeting
10. **Check for expenses** - scan self-sent emails for receipts/expenses to log (if enabled)
11. **Check recurring monthly tasks** - if it's the 15th (or next working day after), remind about tasks below
12. **Check LinkedIn status** - what's scheduled? If nothing, flag it. Remind: every post needs an image.
13. **Check active project folders** - scan project folders listed above. Read their status files (README, PROGRESS.md). Check "Last Updated" dates. If files look stale, ASK: "The [project] status files haven't been updated since [date]. Is the status still accurate?"
14. **Look 10 working days ahead** - check calendar for the next 10 working days. For any meetings, ask: "You've got [meeting] on [date] - do we need to prep for that, or have we already covered it?"
15. **Ask: "What do you want to focus on today?"**

You decide what actually gets done. Claude surfaces everything, then you choose.

### After Agreeing the Day's Plan

Once you've agreed what you're focusing on today:

1. **Add notes to standup calendar event** - Your daily plan goes into the meeting event as notes
2. **Give you a standup-ready summary** - A concise version for you to talk through: what you're working on, what you need from others (if anything), any blockers

### Standup Transcript Processing

After a daily standup, say: *"Process the standup"* or *"Get the standup transcript"*

Claude will:

1. **Fetch the transcript** - via Teams API
2. **Run first-pass analysis** - extract what matters
3. **Return a brief bullet summary**, segmented by type:
   - Content opportunities (LinkedIn ideas)
   - New business opportunities
   - Client status updates
   - Actions / decisions made
   - Ideas worth capturing
   - Things you're now waiting on
4. **Propose where each item lives** in the system:
   - PROJECTS.md - new workstreams or updates
   - IDEAS.md - sparks to marinate
   - WAITING_FOR.md - things others owe you
   - CLIENTS.md - client context updates
   - DECISIONS.md - decisions made with reasoning
5. **Suggest todos** for anything actionable
6. **We discuss** - you confirm what gets filed, what gets acted on

The goal: nothing from standups gets lost. Every call builds institutional memory.

---

### Friday Review (added to morning check-in)

Every Friday, in addition to the normal morning routine, add:

1. **IDEAS.md staleness check** - "Any ideas over 2 weeks old to act on or dismiss?" Surface them, decide: act, keep marinating, or archive.
2. **PROJECTS.md completion check** - "Any projects finished that should be archived?"
3. **DECISIONS.md cleanup** - "Any old decisions to archive?"

The goal: nothing rots. Everything either moves forward, gets archived, or gets killed.

---

### Date-Stamping Rule

**Every time Claude adds something to the system, it dates it.** This is how staleness is tracked:
- IDEAS.md entries get date headers (### 9 January 2026)
- WAITING_FOR.md entries get "Sent" and "Follow-up" dates
- PROJECTS.md gets "Last updated" at the top
- CLIENTS.md status updates include dates

Claude knows today's date from system context. It can calculate age and surface anything going stale.

---

### End-of-Day Reconciliation (5-10 minutes)

Before you close down, say: *"End of day"* or *"Let's reconcile"*

Claude will help you:

1. **Clear the deck** - What got done today? Mark complete, archive, move on.
2. **Capture loose threads** - Anything nagging? Quick dump into INBOX.md or IDEAS.md
3. **Update WAITING_FOR.md** - Did you delegate or ask for something? Log it with a follow-up date.
4. **Flag tomorrow's priorities** - What's the one thing that would make tomorrow a win?
5. **Check calendar for tomorrow** - Any prep needed tonight?

The goal: close the day with a clear head, knowing nothing is lost.

---

## Expense Tracking

*Configure during setup if you want expense tracking*

### How It Works

**Your workflow:**
- Paper receipt? Photo it, email to yourself
- Email receipt (order confirmation, invoice)? Forward it to yourself
- No special subject line needed (though "Expense" or "Exp" helps)

**Claude's workflow (during morning check-in or on demand):**
1. Scan self-sent emails for anything that looks like an expense (attachments, receipt language, amounts)
2. Show you what was found: "I found 2 potential expenses - want me to log them?"
3. If confirmed, extract: date, vendor, amount, VAT, category
4. Add to the expenses spreadsheet
5. Save the receipt file to the receipts folder

### File Locations

- **Spreadsheet:** `[path to expense spreadsheet]`
- **Receipts folder:** `[path to receipts folder]` (subfolders by month: `2026-01/`, `2026-02/`, etc.)

### Categories

- Travel
- Client Entertainment
- Software/Subscriptions
- Office/Equipment
- Professional Services
- Other

### Status Values

- **Unclaimed** - Logged but not yet claimed/reimbursed
- **Claimed** - Submitted for reimbursement or offset
- **Paid** - Reimbursed or accounted for

---

## Recurring Monthly Tasks

**On the 15th of each month**, remind during morning briefing:

| Task | Details |
|------|---------|
| [Monthly task 1] | [What needs to happen] |
| [Monthly task 2] | [Details] |

These should also be in the calendar as recurring events, but Claude flags them in the morning briefing when it's the 15th (or the next working day if the 15th falls on a weekend).
