# AI Chief of Staff - Template

A system for using Claude Code as a persistent AI chief of staff / executive assistant.

## What This Is

This template provides a folder structure and prompt framework for Claude Code to act as your external memory and executive assistant. It can:

- Track clients, projects, and follow-ups
- Draft emails and communications in your voice
- Maintain context between sessions via markdown files
- Integrate with Microsoft 365 (calendar, email, files) via MCP server

## Setup

### 1. Copy This Folder Structure

Copy the contents of this folder to your working directory (e.g., `~/Desktop/chief-of-staff/`).

### 2. Customize CLAUDE.md

Edit `CLAUDE.md` with:
- Your background and role
- How you work (energy rhythms, communication style)
- Key people you work with
- Your priorities
- Permissions (what the AI can/cannot do autonomously)
- Your writing style guide

### 3. Set Up Microsoft 365 Integration (Optional)

If you want calendar, email, and file access:

1. Copy the `office-365-mcp-server/` folder to your home directory
2. Follow the setup instructions in `office-365-mcp-server/SETUP.md`
3. Add the MCP server to your Claude Code settings

### 4. Start Using It

Open Claude Code in your chief-of-staff folder and start working. The AI will read CLAUDE.md on each session and maintain the other files as you work together.

## File Structure

```
chief-of-staff/
├── CLAUDE.md           # Core instructions (customize this!)
├── INBOX.md            # Quick capture for unprocessed items
├── PROJECTS.md         # Active projects with outcomes and next actions
├── WAITING_FOR.md      # Things blocked on others
├── DECISIONS.md        # Decisions with reasoning
├── CLIENTS.md          # Client/contact reference
├── IDEAS.md            # Ideas that need to marinate
└── work-orders/        # Specific briefs or work packages
```

## Key Principles

1. **Files ARE the memory** - Claude has no memory between sessions. Everything important must be written to files.

2. **Update as you go** - Don't batch updates. Update files immediately when relevant information comes up.

3. **CLAUDE.md is the instruction manual** - Keep it stable. Update rarely but keep it accurate.

4. **Other files are living documents** - They should be updated constantly as work happens.

## Daily Routines

**Morning check-in**: Say "morning check-in" to get a summary of your day, pending follow-ups, and suggested priorities.

**End of day**: Say "end of day" to reconcile what got done, capture loose threads, and prep for tomorrow.

## Credits

Built by AgentFlow (goagentflow.com)
