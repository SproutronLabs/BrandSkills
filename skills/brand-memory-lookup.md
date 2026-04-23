---
name: brand-memory-lookup
description: >
  Search brand memory before answering ANY question about the brand, products,
  customers, personas, campaigns, or performance. Prevents hallucination by
  grounding answers in accumulated knowledge. Activate for any brand-related question.
metadata:
  author: sproutron-ai
  version: "1.0.0"
---

# Brand Memory Lookup

## Purpose

Always search brand memory before answering brand-related questions. Brand memory
contains accumulated knowledge compiled from reviews, surveys, support tickets,
brand docs, and previous data analyses. Using it prevents hallucination and
ensures consistency across conversations.

## Prerequisites

- Aura Brand Memory MCP server connected (provides `get_brand_memory`,
  `search_brand_memory`, `list_brand_memory`, `get_brand_context`)

## Workflow

1. **Extract topic** from the user's question. Identify the key entity or concept.

2. **Search brand memory:**
   - Call `search_brand_memory` with the most relevant keyword
   - If a specific entity type is identifiable (product, persona, campaign, etc.),
     also call `get_brand_memory` with `entity_type` and `entity_name`

3. **First turn context:** If this is the first question in the conversation,
   call `get_brand_context` to understand what the brand knows overall.

4. **Weave into answer:** Include relevant brand memory claims in your response.
   Cite the source article.

5. **No results:** If brand memory has nothing relevant, state:
   "Brand memory has no articles on [topic] yet."
   Then proceed with data tools if available.

## Data Source Routing

Choose the right tool based on what the user is asking:

- **"last month", "trends", "compare", "segment"** → BigQuery via `query_brand_data`
- **"right now", "today", "current", "live"** → Real-time API via Composio MCP
- **"what do we know", "brand voice", "reviews say"** → Brand Memory via `search_brand_memory`

## Output Format

Include a `Sources:` footer when brand memory claims are used:

```
Sources: brand_memory/product/smoked-cheddar (updated 2026-04-10)
```

## What Brand Memory Is For vs BigQuery

Brand memory stores QUALITATIVE context — things BigQuery cannot answer:
- Customer voice & sentiment (from reviews, surveys)
- Brand guidelines & voice
- Team decisions & rationale
- Structural patterns & strategic insights
- Campaign learnings & competitive intel

For CURRENT NUMBERS (revenue, counts, rates), always query BigQuery fresh.
Combine both: "Reviews praise Smoked Cheddar's flavor (brand memory) and it's currently #1 at 40.4% repeat (live data)"

## Staleness Rules

When citing brand memory claims, check the evidence timestamp:

- **< 7 days old:** "Brand memory shows..." (cite directly)
- **7-30 days old:** "As of [date], brand memory shows..." (note the date)
- **> 30 days old:** "Brand memory from [date] suggests... let me verify with fresh data."
  MUST cross-check with query_brand_data before citing as current.

RULE: Never present a brand memory claim as current fact if it's > 30 days old
without verifying against live data first.

## Best Practices

- Never fabricate brand-specific claims. If memory is empty, say so.
- Always cite the article title and last-updated date.
- Combine brand memory (qualitative) with data tools (quantitative) for the richest answers.
- If a claim in memory contradicts fresh data, trust the data and flag the discrepancy.
- For human review of recent learnings, suggest: "Want me to review what was recently logged to brand memory?"
