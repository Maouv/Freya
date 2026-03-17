# TPA — Termux Personal Agent

A self-hosted AI agent that lives in Discord. Talks like a person, runs terminal commands, reads files, browses the web, and consults other AI models when needed.

## What it does

- **Responds in Discord** — send a message, get a response. That's the interface.
- **Runs bash commands** on the VPS with human-in-the-loop confirmation (Y/N before every execution)
- **Reads & writes files** within a sandboxed workspace directory
- **Fetches web pages** and summarizes the content
- **Consults other AI models** via `<ASK_DEEPSEEK>`, `<ASK_QWEN>`, `<ASK_CLAUDE>` tags — the agent decides when to delegate
- **Remembers context** — daily chat logs + semantic search via embeddings so it knows what was discussed earlier

## Stack

| Layer | Tech |
|---|---|
| Primary LLM | Groq (Kimi K2) |
| Fallback chain | Gemini 2.5 Flash → Qwen 3.5 122B → DeepSeek V3.2 |
| Embeddings | Gemini Embedding 001 |
| Interface | Discord.js v14 |
| Runtime | Node.js, self-hosted on VPS |

## How it works

```
User message (Discord)
  → Semantic search over chat history
  → System prompt (persona + rules + tools)
  → Primary LLM (Groq) with auto-fallback chain
  → Tag detection: RUN_BASH / READ_FILE / FETCH_URL / ASK_*
  → Human confirmation if action is destructive
  → Response back to Discord
```

Daily logs are auto-summarized into long-term memory every night.

## Setup

```bash
git clone https://github.com/Maouv/TPA.git
cd TPA
npm install
cp .env.example .env  # fill in your API keys
node src/discord.js
```

Required env vars: `DISCORD_BOT_TOKEN`, `DISCORD_CHANNEL_ID`, `GEMINI_API_KEY`, `GROQ_API_KEY`, `NVIDIA_API_KEY_DEEPSEEK`, `NVIDIA_API_KEY_QWEN`, `ANTHROPIC_API_KEY`

