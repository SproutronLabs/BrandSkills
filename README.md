# Aura Brand Skills

Compounding brand intelligence skills for DTC brands. Every conversation makes the brand smarter.

## What it does

- **Read-first:** Always searches brand memory before answering brand questions
- **Write-back:** Extracts quantified insights from data analysis and logs them to brand memory
- **Conversation logging:** Builds searchable Q&A history for context
- **Quality gates:** Lint checks, dedup, PII rejection, stale claim detection

## Skills

| Skill | Trigger | What it does |
|-------|---------|-------------|
| `brand-memory-lookup` | Any brand/product/persona question | Searches brand memory before answering |
| `learning-capture` | After data tool usage | Extracts and logs new insights |
| `conversation-logger` | Every response | Logs Q&A silently |
| `data-quality-check` | "validate" / "audit" / "lint" | Cross-validates claims vs live data |

## Requirements

- **Aura Brand Memory MCP server** — provides read/write tools for brand memory + BigQuery
- Claude Pro, Max, or Team plan (for skills support)

## Install

### Claude.ai (Team/Pro)
1. Download this repo as a zip
2. Go to Settings > Features
3. Upload the zip as a custom skill

### Claude Code
```bash
# Clone into your project skills directory
git clone https://github.com/sproutron-ai/aura-brand-skills .claude/skills/aura-brand-skills
```

## The Compounding Loop

```
User asks question
  -> brand-memory-lookup: search memory for context
  -> data tools: query BigQuery for fresh numbers
  -> Claude responds with grounded answer
  -> learning-capture: extract insights, write to memory
  -> conversation-logger: log Q&A for history
  -> Next question: memory is richer, answers are better
```

## License

Apache 2.0
