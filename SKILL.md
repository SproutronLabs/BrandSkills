---
name: aura-brand-intelligence
description: >
  Compounding brand intelligence for DTC brands. Activates sub-skills based on
  user intent: brand knowledge lookup before answering, write-back after data
  discovery, conversation logging, and data quality validation. Requires Aura
  Brand Memory MCP server connected.
license: Apache-2.0
metadata:
  author: sproutron-ai
  version: "1.0.0"
---

# Aura Brand Intelligence

You are connected to a brand's living knowledge base via MCP tools. Every conversation
should make the brand smarter. Follow these rules on every interaction:

## Routing

Based on user intent, activate the appropriate workflow:

### Brand questions (products, personas, campaigns, brand voice, reviews)
**Always search brand memory first.** See `skills/brand-memory-lookup/SKILL.md`.

### After any data analysis (BigQuery queries, analytics tools)
**Extract and log new insights.** See `skills/learning-capture/SKILL.md`.

### Every conversation turn
**Log the Q&A for context.** See `skills/conversation-logger/SKILL.md`.

### "Validate", "audit", "lint", "check the data", "is this still true"
**Cross-validate claims against live data.** See `skills/data-quality-check/SKILL.md`.

## Data Source Routing

Choose the right data source based on what the user needs:

| Signal in question | Use | Why |
|---|---|---|
| "last month", "trends", "compare", "segment", "persona" | `query_brand_data` (BigQuery) | Pre-computed, fast, historical |
| "right now", "today", "current", "live status" | Real-time API (Shopify/Meta/Klaviyo via Composio) | Fresh, not yet in BigQuery |
| "what do we know about", "brand voice", "reviews say" | `search_brand_memory` / `get_brand_memory` | Accumulated qualitative context |

When in doubt, check brand memory first, then query data.
