---
name: learning-capture
description: >
  After answering a question that used data or analytics tools, extract durable
  patterns and insights and write them to brand memory via log_learning. Captures
  trends, comparisons, and decisions — not point-in-time snapshots. Activates
  automatically after data tool usage.
metadata:
  author: sproutron-ai
  version: "1.1.0"
---

# Learning Capture

## Purpose

After every response where you queried data and discovered something new, extract
durable insights and write them back to brand memory. This is what makes the brand
smarter over time — each conversation compounds knowledge for future conversations.

**Critical distinction:** Brand memory stores patterns, trends, and decisions.
Point-in-time metrics (revenue this week, customer count today) come fresh from
BigQuery every time — don't persist those.

## Prerequisites

- Aura Brand Memory MCP server connected (provides `log_learning`)
- At least one data/analytics tool was called in this conversation turn

## Workflow

1. **Check trigger:** Did you use any analytics tools in this turn?
   (e.g., `query_brand_data`, any Composio tool, any BigQuery tool)
   - If no tools used → skip, do not call log_learning
   - If only brand memory reads (search/get) → skip

2. **Evaluate:** Does your answer reveal a DURABLE pattern or insight?
   - Is this a trend, comparison, anomaly, or decision — not just a number?
   - Would this still be useful context in 30 days?
   - Is it about a specific entity (product, persona, campaign)?
   - Is it not already in brand memory?

3. **Format the learning:** Separate the pattern from the evidence.
   - **claim:** The durable insight (the pattern, trend, or decision)
   - **evidence:** The current metrics that support it (timestamped reference)

4. **Extract and log:** For each new insight (maximum 3 per turn):
   ```
   Call log_learning with:
     entity_type: product | persona | campaign | theme | brand | decision
     entity_name: "Smoked Cheddar"
     claim: "Smoked Cheddar is consistently the top repeat-purchase product, maintaining 38-42% repeat rate over 6+ months"
     evidence: "As of Apr 2026: 40.4% repeat rate, 50,449 orders, $1.15M revenue. Source: t_product_metrics"
     source_tool: "query_brand_data"
   ```

5. **Confirm:** Briefly tell the user: "Logged N insights to brand memory."

## Quality Gate

### The learning must be DURABLE (pattern, not snapshot)

Ask: "Will this insight still be true and useful in 30 days?"
- YES → log it (pattern, trend, structural insight, decision)
- NO → don't log it (today's number, this week's metric)

### All claims must also pass:

- **Evidenced:** Includes reference metrics with date for when the pattern was observed
- **New:** Not already present in brand memory
- **Attributable:** About a specific named entity
- **Not PII:** No customer emails, names, or addresses

### Good claims (durable patterns with reference metrics)

- **claim:** "Fromagier boxes are the primary acquisition vehicle — consistently 80%+ of first-time buyers enter through a Fromagier variant"
  **evidence:** "As of Apr 2026: The Fromagier 80.1% entry (824 buyers), Nov Fromagier 41%, Dec Fromagier 39.8%. Source: t_product_metrics"

- **claim:** "Smoked Cheddar is consistently the top repeat-purchase product across all time periods"
  **evidence:** "As of Apr 2026: 40.4% repeat rate, 50,449 orders, $1.15M total revenue. #1 by both revenue and order count. Source: t_product_metrics"

- **claim:** "Win-Back customers are typically 5x overdue relative to their personal reorder cadence"
  **evidence:** "As of Apr 2026: 4,418 customers, avg 72 days between orders but 366 days since last order (5.1x ratio). Source: t_persona_summary"

- **claim:** "Meta ad fatigue sets in when frequency exceeds 3.5 — ROAS drops significantly past this threshold"
  **evidence:** "Observed Apr 2026: campaigns above 3.5 frequency showed 40% lower ROAS. Source: query_ad_fatigue"

- **claim:** "Holiday season (Nov-Dec) drives 3x growth in Gifter and Seasonal Gifter segments"
  **evidence:** "Nov-Dec 2025: Gifter grew from ~2K to 6K, Seasonal Gifter from ~1.8K to 5.6K. Source: t_persona_summary historical"

- **claim:** "Team chose 15% discount for Win-Back campaigns because this segment has high LTV ($250) but needs urgency to reactivate"
  **evidence:** "Decision made Apr 2026 based on: avg LTV $249.87, avg 366 days dormant, 4,418 addressable customers"

### Bad claims (volatile snapshots — query fresh instead)

- "Smoked Cheddar has 40.4% repeat rate" → stale next data refresh
- "Win-Back segment has 4,418 customers" → changes daily
- "Revenue was $45K last week" → stale immediately
- "Meta ROAS is 2.1x this week" → changes tomorrow
- "Shipping is expensive" → no evidence, no pattern
- "john@example.com ordered 5 times" → PII

## Best Practices

- Maximum 3 learnings per turn — focus on the most impactful patterns
- Always include reference metrics with a date ("As of Apr 2026: ...") so future readers know when the pattern was observed
- Prefer insights that would help future campaign, product, or pricing decisions
- If your analysis reveals a pattern that contradicts an existing brand memory claim, log the updated pattern
- Decisions and strategies are the highest-value learnings — always log "team decided X because Y"
