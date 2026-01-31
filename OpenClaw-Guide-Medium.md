# How I Built an AI Assistant That Actually Does Things (And How You Can Too)

ChatGPT is great, but it lives in a browser. It can't read your files, run commands, or remember what you said yesterday. It can't wake you up at 7 AM with a weather briefing.

Enter OpenClaw — the open-source AI assistant that bridges the gap between LLMs and your actual digital life.

---

## Why OpenClaw?

Most AI assistants are passive. You ask, they answer.

OpenClaw is **active**:

- It has persistent memory across sessions
- It can run shell commands, browse the web, manage files
- It schedules tasks and sends you reminders
- It's proactive — checking your inbox, calendar, and weather automatically
- It runs locally, so your data stays yours

It's like having a junior developer who never sleeps.

---

## Getting Started in 5 Minutes

### 1. Install

```bash
npm install -g openclaw
openclaw init
```

Follow the setup wizard. You'll need:
- An LLM API key (OpenRouter, OpenAI, Anthropic, etc.)
- A messaging app (Telegram, Discord, Signal, or just the CLI)

### 2. Start

```bash
openclaw gateway start
```

That's it. You now have an AI assistant running.

---

## Teaching OpenClaw About You

The magic of OpenClaw is in its **workspace** — a folder where it stores everything about you.

Edit `AGENTS.md` to tell OpenClaw who you are:

```markdown
- Name: Sarah
- Timezone: London (UTC+0)
- Projects: Working on a SaaS product, learning Rust
- Style: Concise, use bullet points
```

Now every response will be personalized to you.

---

## Building Automation: From Chatbot to Butler

### Example 1: Automated Daily Briefing

Add a cron job for 8 AM every day:

```bash
openclaw cron add \
  --schedule "0 8 * * *" \
  --payload '{"kind":"systemEvent","text":"Create my daily briefing: weather, calendar, top 3 priorities, email summary"}'
```

Every morning, OpenClaw wakes up, checks the weather, reviews your calendar, summarizes emails, and hands you a briefing before you've had your coffee.

### Example 2: Proactive Notifications

Edit `HEARTBEAT.md`:

```markdown
Every 30 minutes, check:
- Urgent emails
- Calendar events in next 2 hours
- Weather if rain is expected
```

OpenClaw now actively watches these things and alerts you only when it matters.

### Example 3: Smart File Organization

Tell OpenClaw:
```
Watch ~/Downloads and organize files automatically into folders by type and date.
```

It'll set up a script and run it periodically. No more messy downloads folder.

---

## Extending OpenClaw with Skills

Skills are plugins. Want OpenClaw to fetch jokes? Control smart home devices? Monitor servers? Build a skill.

A skill is just a folder with:
- `SKILL.md` (documentation)
- `package.json` (npm metadata)
- `lib/tool.js` (your code)

Here's a simple skill that fetches the current weather:

```javascript
export default {
  async getWeather(location) {
    const apiKey = process.env.WEATHER_API_KEY;
    const response = await fetch(`https://api.weatherapi.com/v1/current.json?key=${apiKey}&q=${location}`);
    const data = await response.json();
    return `${data.current.temp_c}°C in ${data.location.name}, ${data.current.condition.text}`;
  }
};
```

Now OpenClaw can answer "What's the weather in Tokyo?" using live data.

---

## The Proactive Pattern: Heartbeats

Most AI assistants wait for you to speak. OpenClaw can reach out first.

The **heartbeat system** lets it periodically check things and notify you when something needs attention:

- "You have 3 urgent emails"
- "Meeting with design team in 45 minutes"
- "Rain expected at 4 PM — bring an umbrella"

It's like having a personal assistant who nudges you before you even ask.

---

## Real-World Use Cases

### For Developers
- Automated daily standups (summarize commits, PRs, issues)
- Dependency update monitoring
- CI/CD failure alerts
- Server health checks

### For Content Creators
- Social media scheduling
- RSS feed summarization
- Trend monitoring
- Automated research briefings

### For Small Business
- Customer inquiry monitoring
- Invoice reminders
- Daily financial summaries
- Competitor tracking

### For Personal Life
- Daily briefings (weather, calendar, priorities)
- Grocery list management (add items via voice)
- Travel planning (research + itinerary)
- Health tracking reminders

---

## Security Your Data

OpenClaw runs locally. Your files, messages, and memory never leave your machine unless you explicitly connect it to external services.

Key practices:
- Never hardcode API keys — use environment variables
- Review auto-actions — don't let it delete files without confirmation
- Memory isolation — sensitive data in MEMORY.md only loads in main sessions

---

## Where to Go From Here

- **Explore skills:** Browse [ClawdHub](https://clawdhub.com) for pre-built skills
- **Build your own:** Read the skill creator documentation in your OpenClaw install
- **Join the community:** [Discord](https://discord.com/invite/clawd), [GitHub](https://github.com/openclaw/openclaw)
- **Go deep:** Check the proactive agent skill for advanced patterns

---

## Why This Matters

We're entering an era where AI isn't just a chatbot — it's a teammate. OpenClaw shows what's possible when you give AI memory, tools, and the ability to act autonomously.

The best part? It's open source. You can inspect the code, contribute skills, and shape how this technology evolves.

---

*Enjoyed this article? [Buy me a coffee](https://ko-fi.com/richardsloman) to help keep my OpenClaw instance running!*

---

*P.S. If you build something cool with OpenClaw, share it. I'd love to see what you create.*
