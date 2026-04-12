# Non-Technical Setup Guide

*You don't need to be technical to use this. You just need Claude and about 30 minutes.*

---

## What You Need Before Starting

- **A Claude subscription** (you're paying for Claude at claude.ai - any paid plan works)
- **A computer** (Mac or Windows)
- **Willingness to follow along** - Claude will do the heavy lifting

That's it. You don't need to know what a terminal is, what git is, or how to code. Claude will explain everything as you go.

---

## The Big Idea

You're going to use Claude (in your browser) to help you install Claude Code (a more powerful version that runs on your computer). Once Claude Code is running, it becomes your Chief of Staff - it reads your files, learns how you work, and helps you run your day.

Think of it like this: browser Claude is the receptionist who helps you set up. Claude Code is the Chief of Staff who stays.

---

## Step 1: Open Claude in Your Browser

Go to [claude.ai](https://claude.ai) and start a new conversation.

Paste this prompt:

```
I want to set up an AI Chief of Staff system on my computer. I'm not technical - I've never used a terminal before and I don't know what git is. I need you to walk me through everything step by step, in plain English, with screenshots-level detail.

My computer is a [Mac / Windows PC] (tell Claude which one).

Here's what I need installed:
1. Node.js (needed to run the tool)
2. Git (needed to download and sync files)
3. Claude Code (the AI tool that becomes my Chief of Staff)

For each one, please:
- Tell me exactly where to go and what to click
- Explain any choices I need to make
- Tell me what success looks like before moving to the next step
- If I need to type something into a terminal, tell me how to open the terminal first

Go one step at a time. Don't move to the next step until I confirm the current one worked.
```

Claude will walk you through installing the three things you need. This typically takes 10-15 minutes. Follow along at your own pace - there's no rush.

---

## Step 2: Download the Chief of Staff System

Once Claude confirms everything is installed, paste this:

```
Everything is installed. Now I need to download the AI Chief of Staff template. 
Please walk me through:
1. Opening my terminal
2. Running this command: git clone https://github.com/goagentflow/ai-chief-of-staff.git ~/Desktop/chief-of-staff
3. Then starting Claude Code in that folder

Go step by step. Tell me exactly what to type and what I should see.
```

After this, you'll have a folder called `chief-of-staff` on your Desktop with all the template files.

---

## Step 3: Start the Guided Setup

Claude will have helped you open Claude Code in the chief-of-staff folder. Now type:

```
Let's set up my AI Chief of Staff
```

From here, Claude Code takes over. It will ask you:
- Who you are and what you do
- How you write (it'll ask for writing samples - emails, posts, anything in your natural voice)
- What it's allowed to do without asking

This takes about 15 minutes. Just answer the questions naturally - it's a conversation, not a form.

---

## Step 4: Start Using It

Once setup is complete, type:

```
Morning check-in
```

Your Chief of Staff will walk you through your day. That's it - you're running.

---

## What's Next

Once you've been using the basics for a few days and it feels natural, you can add more capabilities. Each one has its own setup guide - and the same principle applies: if you get stuck, ask Claude to walk you through it.

| What | Why you'd want it | Guide |
|------|-------------------|-------|
| **Wiki memory** | So it remembers things between sessions | [Setup guide](wiki-memory-setup.md) |
| **Telegram** | So you can message it from your phone | [Setup guide](telegram-setup.md) |
| **Email alerts & meeting prep** | So it pings you when clients email and preps you before meetings | [Setup guide](scheduled-agents-setup.md) |
| **Content scanning** | So it monitors your industry sources and filters the noise | [Setup guide](content-scanning-setup.md) |

For each of these, you can ask Claude Code to help you set it up:

```
I want to set up [wiki memory / Telegram / scheduled agents / content scanning]. 
I'm not technical. Read the setup guide at setup/[guide-name].md and walk me 
through it step by step.
```

Claude Code can read the guide and translate each step for your skill level.

---

## If Something Goes Wrong

**"I can't find the terminal"**
- Mac: Press Cmd+Space, type "Terminal", hit Enter
- Windows: Press the Windows key, type "Command Prompt" or "PowerShell", hit Enter

**"A command gave me an error"**
- Copy the exact error message
- Paste it into Claude (browser or Claude Code) and say "I got this error, what do I do?"
- Claude is very good at diagnosing error messages

**"I'm confused and want to start over"**
- That's fine. Close everything. Go back to Step 1. Nothing you've done is permanent or harmful.

**"I want someone to just do this for me"**
- Completely understandable. [Get in touch with AgentFlow](mailto:hamish@goagentflow.com) and we can set the whole thing up for you. That's literally what we do.

---

## A Note From Hamish

I'm not a developer either. I'm a CEO who got tired of forgetting things and losing track of conversations. I built this system by telling Claude what I needed and letting it figure out the technical bits. You can do the same.

The first 30 minutes are the hardest part. After that, you're just having conversations with your Chief of Staff - no technical knowledge required.

If you build something interesting with this, I'd genuinely love to hear about it: hamish@goagentflow.com

[AgentFlow](https://goagentflow.com) - AI consulting and implementation.
