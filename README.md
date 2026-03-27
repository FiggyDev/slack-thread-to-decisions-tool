# ✅ Settled

**Your team makes decisions in Slack. Settled captures them.**

Paste a thread. Get a clean decision record — owner, rationale, action items — in seconds. Runs on your machine. No Slack account needed.

---

## The problem

Your team spent 40 messages debating a tech stack choice. Somewhere in the middle, a decision was made. Three days later nobody can find it, nobody agrees on what was decided, and the debate starts again.

Decisions buried in threads aren't decisions — they're liabilities.

---

## The solution

Paste the thread. Settled reads it with a local AI model and hands you back:

- **What was decided** — one clear sentence
- **Who owns it** — the person responsible
- **Why** — the rationale, if it was stated
- **What happens next** — action items with owners
- **Confidence** — how clearly the decision was actually made

No Slack API. No login. No data leaving your machine.

---

## Demo

Save this thread to a file and run one command:

```bash
python settled.py --file thread.txt
```

**thread.txt** — a real Slack thread:

```
Sarah:   ok we need to pick a deploy target before EOD — Railway or stay on Render?
James:   render has been fine but we keep hitting the 512mb limit
Marcus:  same, it crashed twice this sprint already
Sarah:   Railway gives us more headroom and the pricing is similar once we upgrade
James:   yeah i'm fine with Railway
Marcus:  same
Sarah:   ok Railway it is. @James can you migrate staging this week?
James:   yep, done by Thursday
Sarah:   great. prod migration follows after staging looks stable for a few days
Marcus:  should we document this somewhere?
Sarah:   yeah @Marcus add it to the decisions log in Notion
Marcus:  on it
```

**Output:**

```
# Decision Record

Thread summary: The team agreed to migrate from Render to Railway.

## Decision 1
**Migrate from Render to Railway as the deploy target**

- Owner:     James
- Rationale: Render's 512MB memory limit has caused two crashes this sprint;
             Railway provides more headroom at similar cost
- Confidence: 🟢 high
- Action items:
  - James: migrate staging environment by Thursday
  - Prod migration follows after staging is confirmed stable
  - Marcus: document decision in the Notion decisions log
```

12 messages. 3 people. 2 action items with owners. Done in under 5 seconds.

Prefer the web UI? `python settled.py --web` — paste and press Cmd+Enter.

---

## Get started

```bash
git clone https://github.com/FiggyDev/slack-thread-to-decisions-tool
cd slack-thread-to-decisions-tool
python settled.py --web
```

Opens at `http://localhost:5100`. Paste your thread, press **Cmd+Enter**.

No install step. No environment setup. No account.

> **First time?** You'll need Ollama running locally — [download here](https://ollama.com), then:
> ```bash
> ollama pull llama3.2:3b
> ```
> That's it. Settled routes to it automatically.

---

## Why local-first

Most AI tools send your data to external servers. Settled doesn't.

When you run it with Ollama, your Slack thread text stays on your machine. It never touches a cloud API. That matters when threads contain engineering decisions, business strategy, or anything confidential.

Cloud fallbacks (OpenAI, Anthropic) are available if you prefer, using your own keys.

---

## CLI usage

```bash
# Paste a thread directly
python settled.py "Alice: Let's go with Postgres. Bob: Agreed."

# From a file
python settled.py --file thread.txt

# From stdin
cat thread.txt | python settled.py

# Raw JSON output (great for piping into other tools)
python settled.py --file thread.txt --json

# Use a specific model
python settled.py --model gemma3:4b --file thread.txt
```

---

## LLM backends

Settled tries these in order — first available wins:

| Backend | Setup | Privacy |
|---|---|---|
| Ollama (local) | `ollama serve` running | ✅ Stays on your machine |
| OpenAI | `OPENAI_API_KEY` env var | Sent to OpenAI |
| Anthropic | `ANTHROPIC_API_KEY` env var | Sent to Anthropic |

For local use: `ollama pull llama3.2:3b` (fast) or `ollama pull gemma3:4b` (better quality).

Zero required dependencies for local Ollama. Pure Python stdlib.

---

## Free vs paid

### Free — this repo, forever

- ✅ Web UI (local)
- ✅ CLI with JSON output
- ✅ Local LLM via Ollama — fully private
- ✅ Cloud LLM fallbacks with your own keys
- ✅ Unlimited use, no account

### Settled Pro — coming soon

For teams who want this to live inside Slack itself:

| Feature | Description |
|---|---|
| `/settled` slash command | Extract decisions without leaving Slack |
| Decision log | Searchable history across all channels |
| Team workspace | Shared log your whole team can access |
| Monday digest | Weekly decisions summary sent to your channel |
| Reminders | Ping action item owners when things go stale |
| Export | Push decisions to Notion, Confluence, or Linear |

→ [Join the waitlist](https://github.com/FiggyDev/slack-thread-to-decisions-tool/issues/1)

---

## Requirements

Python 3.8+. No required packages for local use.

Optional for cloud:
```bash
pip install openai       # OpenAI backend
pip install anthropic    # Anthropic backend
```

---

## Contributing

PRs welcome. The goal is to keep this zero-dependency for local use — please don't add frameworks or build steps to the core path.

---

## License

MIT
