# Aura Brand Skills

Brand context layer skills for DTC brands. Gives Claude the brand context it needs to answer with precision — and writes new patterns back so context compounds over time.

## What it does

Aura is **not** brand intelligence — the intelligence is yours and Claude's. Aura is the **context layer** that makes both smarter:

- **Read-first:** Searches brand context (products, personas, decisions, customer voice) before answering
- **Write-back:** Extracts durable patterns from data analysis and persists them
- **Conversation logging:** Builds searchable Q&A history for future context
- **Quality gates:** Only durable patterns get saved (no metric snapshots, no PII, no duplicates)

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
