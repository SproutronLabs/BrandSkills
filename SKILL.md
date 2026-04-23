---
name: aura-brand-context
description: >
  Brand context layer for DTC brands. Gives Claude full context about the brand —
  customers, products, campaigns, brand voice — so every answer is grounded in
  the brand's accumulated knowledge and live data. Activates sub-skills based on
  user intent: read context first, query fresh data, write durable patterns
  back. Requires Aura Brand Memory MCP server connected.
license: Apache-2.0
metadata:
  author: sproutron-ai
  version: "1.1.0"
---

# Aura Brand Context Layer

You have access to a brand's living context via MCP tools. The intelligence is
yours and the user's — Aura provides the context that makes both smarter.
Every conversation enriches the brand context for the next one.

## Routing

Based on user intent, activate the appropriate workflow:

### Brand questions (products, personas, campaigns, brand voice, reviews)
**Always search brand memory first.** See `skills/brand-memory-lookup.md`.

### "What campaigns should I run?" / "What should I work on?" / any campaign topic
**Surface pre-analyzed opportunities.** See `skills/email-campaigns.md`.

### After any data analysis (BigQuery queries, analytics tools)
**Extract and log new insights.** See `skills/learning-capture.md`.

### Every conversation turn (MANDATORY — no exceptions)
**Call `log_conversation` after EVERY response with the user's exact question and your answer summary.** This is non-negotiable. See `skills/conversation-logger.md`.

### "Validate", "audit", "lint", "check the data", "is this still true"
**Cross-validate claims against live data.** See `skills/data-quality-check.md`.

## Data Source Routing

Choose the right data source based on what the user needs:

| Signal in question | Use | Why |
|---|---|---|
| "last month", "trends", "compare", "segment", "persona" | `query_brand_data` (BigQuery) | Pre-computed, fast, historical |
| "right now", "today", "current", "live status" | Real-time API (Shopify/Meta/Klaviyo via Composio) | Fresh, not yet in BigQuery |
| "what do we know about", "brand voice", "reviews say" | `search_brand_memory` / `get_brand_memory` | Accumulated qualitative context |

When in doubt, check brand memory first, then query data.
