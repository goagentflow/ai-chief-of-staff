# Skills System

Skills are reusable capability packages that Claude can invoke. They contain guidelines, templates, and instructions for specific types of work.

---

## Built-in Skills

### tone-of-voice
Your personal writing style guide. Created during setup by analysing your writing samples. Used whenever Claude drafts content for you.

**Location:** `skills/tone-of-voice/SKILL.md`

**Invoked when:** Claude is drafting emails, LinkedIn posts, documents, or any written content.

---

### office-365-setup
Guides you through connecting Microsoft 365 (Outlook, Calendar, Teams, OneDrive) to your AI Chief of Staff. Downloads the MCP server and walks you through Azure app registration.

**Location:** `skills/office-365-setup/SKILL.md`

**Invoked when:** User says "Connect Office 365", "Set up Microsoft integration", or during initial setup when they want calendar/email integration.

---

## How Skills Work

Each skill is a folder containing:
- `SKILL.md` - The main instructions file
- Additional reference files as needed (templates, examples, etc.)

Claude reads the SKILL.md file when the skill is relevant to the task.

---

## Creating Your Own Skills

You can create skills for any repeatable type of work. Examples:

- **Presentation style** - Brand colours, slide templates, visual guidelines
- **Proposal writing** - Structure, tone, what to include
- **Code review** - What to look for, how to give feedback
- **Client onboarding** - Steps, templates, what to communicate

### To Create a Skill:

1. Create a folder: `skills/[skill-name]/`
2. Create `SKILL.md` with instructions
3. Add any supporting files (templates, examples)
4. Reference it in CLAUDE.md so Claude knows to use it

### Skill File Structure:

```markdown
# [Skill Name]

*Brief description of what this skill covers*

## When to Use This Skill

[What triggers this skill? What types of requests?]

## Guidelines

[The main instructions - what should Claude do?]

## Templates

[Any reusable templates]

## Examples

[Good examples of the output]

## Things to Avoid

[Common mistakes or anti-patterns]
```

---

## Global vs Project Skills

**Project skills** (in this repo): Specific to this Chief of Staff system.

**Global skills** (in `~/.claude/skills/`): Available across all Claude Code projects. Useful for skills you want everywhere, like your tone-of-voice.

To make a skill global, either:
- Copy it to `~/.claude/skills/[skill-name]/`
- Or add it to your global Claude Code settings

---

## Skill Permissions

Skills need to be explicitly allowed in Claude Code settings. During setup, common skills are automatically added. To add new skills later, update your settings.
