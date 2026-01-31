# OpenClaw Zero to Hero: Build Your First Autonomous AI Agent

*[A practical guide from first install to autonomous automation]*

---

**💎 If this guide helps you, consider supporting my work:**  
https://ko-fi.com/richardsloman

---

## What is OpenClaw?

OpenClaw is an open-source autonomous personal AI assistant that doesn't just chat — it **does things**. Run commands, browse the web, manage files, send messages, check emails, schedule tasks. It's the bridge between LLMs and your actual digital life.

Unlike ChatGPT in a browser, OpenClaw:
- Has persistent memory across sessions
- Runs scheduled tasks and reminders
- Interacts with your actual tools (file system, shell, browser)
- Can be proactive — checking your inbox, calendar, weather
- Runs locally or on your own server

---

## Installation

### Prerequisites
- Node.js 22+
- Linux, macOS, or WSL2 on Windows
- (Optional) Telegram, Discord, Signal, or other messaging apps

### Quick Install

```bash
npm install -g openclaw
openclaw init
```

Follow the prompts to set up:
1. **LLM Provider** (OpenRouter, OpenAI, Anthropic, etc.) — get an API key
2. **Channel** (Telegram, Discord, etc.) — for messaging
3. **Workspace** — where OpenClaw stores its memory and config

### Start the Gateway

```bash
openclaw gateway start
```

That's it. You now have an AI assistant running in your terminal or your messaging app.

---

## Your First Conversation

Say hello:

```bash
openclaw chat
```

Or message the bot you connected (Telegram, Discord, etc.).

Try:
```
What time is it?
List files in the current directory
Create a file called hello.txt with the text "Hello, OpenClaw!"
```

---

## Understanding OpenClaw's Brain

OpenClaw's workspace is its home. Here's the structure:

```
.openclaw/workspace/
├── AGENTS.md       # Who OpenClaw is (SOUL.md), who you are (USER.md)
├── MEMORY.md       # Long-term curated memory
├── memory/
│   └── 2026-01-31.md  # Daily raw logs
├── skills/         # Custom skills you install or create
└── config/         # Gateway and session config
```

**Edit `AGENTS.md`** to teach OpenClaw your preferences:
- How to address you
- Your timezone
- Projects you're working on
- Communication style preferences

**Edit `MEMORY.md`** for things it should always remember (passwords to services, server details, recurring tasks).

---

## Building Automation Skills

Skills are OpenClaw's plugins. They add new capabilities.

### Install a Skill from ClawdHub

```bash
clawdhub search weather
clawdhub install weather
```

### Create Your Own Skill

Skills have a simple structure:

```
my-skill/
├── SKILL.md         # Documentation and instructions
├── package.json     # NPM package info
└── lib/
    └── tool.js      # Your tool implementation
```

**Example: A tool to fetch a random joke**

Create `skills/random-joke/SKILL.md`:
```markdown
# Random Joke Skill

Fetches a random joke from the official-joke-api API.

Usage:
``Tell me a joke``
```

Create `skills/random-joke/package.json`:
```json
{
  "name": "random-joke-skill",
  "version": "1.0.0",
  "description": "Fetches random jokes",
  "main": "lib/tool.js"
}
```

Create `skills/random-joke/lib/tool.js`:
```javascript
export default {
  async fetchJoke() {
    const response = await fetch('https://official-joke-api.appspot.com/random_joke');
    const data = await response.json();
    return `${data.setup} ... ${data.punchline}`;
  }
};
```

Now OpenClaw can tell jokes! 🎭

---

## Scheduled Tasks & Cron Jobs

OpenClaw doesn't wait for you to ask. It can run tasks on a schedule.

### Add a Cron Job

```bash
openclaw cron add --schedule "0 9 * * *" --payload '{"kind":"systemEvent","text":"Good morning! Check your email and calendar."}'
```

This sends a daily 9 AM reminder.

### More Examples

**Every hour, check a website:**
```bash
openclaw cron add --schedule "0 * * * *" --payload '{"kind":"systemEvent","text":"Check example.com for updates"}'
```

**Every Monday at 10 AM:**
```bash
openclaw cron add --schedule "0 10 * * 1" --payload '{"kind":"systemEvent","text":"Weekly standup preparation"}'
```

### List Cron Jobs
```bash
openclaw cron list
```

### Remove a Job
```bash
openclaw cron remove <jobId>
```

---

## Proactive Behavior: The Heartbeat System

OpenClaw can check things periodically without being asked.

**Edit `HEARTBEAT.md` in your workspace:**

```markdown
# Heartbeat Checklist

Check these every 30 minutes:
- Email inbox for urgent messages
- Calendar for events in next 2 hours
- Weather if rain is forecast
- GitHub notifications
```

Now OpenClaw will proactively tell you about important things.

**Example response:**
```
📧 You have 2 urgent emails
📅 Meeting with design team in 45 minutes
🌧️ Rain expected at 4 PM — bring an umbrella
```

---

## Real-World Automation Examples

### 1. Automated Daily Briefing

**Cron job (8 AM daily):**
```bash
openclaw cron add \
  --schedule "0 8 * * *" \
  --payload '{"kind":"systemEvent","text":"Create my daily briefing: weather, calendar, top 3 priorities, email summary"}'
```

OpenClaw will:
- Fetch weather for your location
- Check your calendar
- Pull priorities from your task system
- Summarize urgent emails
- Present it all in a clean format

### 2. Smart File Organization

Tell OpenClaw:
```
Watch ~/Downloads and automatically organize files into:
- ~/Documents/PDFs (for .pdf files)
- ~/Images/Screenshots (for screenshot files)
- ~/Documents/Invoices (for files with "invoice" in name)
```

OpenClaw can set up a script and run it periodically.

### 3. Automated Research

```
Subscribe to tech news. Every morning, summarize the top 5 AI/ML stories.
```

OpenClaw will:
- Fetch RSS feeds
- Read articles
- Summarize key points
- Deliver a concise briefing

### 4. Social Media Management

```
Post a summary of my GitHub activity to Twitter every Friday at 5 PM.
```

OpenClaw will:
- Fetch your GitHub commits/PRs
- Format a tweet
- Post it (with your permission)

---

## Advanced: Multi-Agent Systems

OpenClaw can spawn sub-agents for specialized tasks.

**In your session:**
```
Spawn a sub-agent to research the best Node.js logging libraries.
```

OpenClaw will:
- Create an isolated session
- Give the sub-agent the task
- Let it work independently
- Report back when done

This is perfect for:
- Parallel research
- Isolating risky operations
- Running long tasks in background

---

## Security Best Practices

1. **Never expose API keys** — Use environment variables
2. **Limit permissions** — Only give OpenClaw access to what it needs
3. **Review auto-actions** — Don't let it delete files without confirmation
4. **Memory isolation** — MEMORY.md is main-session only (keeps private data secure)

---

## Troubleshooting

### Gateway won't start
```bash
openclaw gateway status
# Check logs for errors
```

### Cron job not firing
```bash
openclaw cron status
openclaw cron list
```

### Memory not persisting
Check workspace permissions and disk space.

---

## Community & Resources

- **GitHub:** https://github.com/openclaw/openclaw
- **Docs:** https://docs.openclaw.ai
- **Discord:** https://discord.com/invite/clawd
- **Skills:** https://clawdhub.com

---

## Going Deeper

- **Skill Development:** Read `/skills/skill-creator/SKILL.md` in your OpenClaw install
- **Memory System:** Understand how daily files feed into MEMORY.md
- **Proactive Agent Pattern:** Check `/skills/proactive-agent/SKILL.md`

---

## What's Next?

You now have an AI assistant that:
- ✅ Remembers everything
- ✅ Automates tasks
- ✅ Works proactively
- ✅ Learns your preferences
- ✅ Can be extended with skills

What will you build?

---

**Liked this guide?**  
☕ Buy me a coffee: https://ko-fi.com/richardsloman

🐛 Found a bug or want to contribute?  
Open an issue or PR on the OpenClaw repo!

---

*Written with love by OpenClaw users, for OpenClaw users.*
