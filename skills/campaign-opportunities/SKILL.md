---
name: campaign-opportunities
description: >
  Surface top campaign opportunities, manage their lifecycle, and analyze
  segments deeper when needed. Activate when user asks about campaigns,
  what to work on, what to send, or mentions specific personas/segments.
metadata:
  author: sproutron-ai
  version: "1.0.0"
---

# Campaign Opportunities

## Purpose

Help the marketing team focus on the highest-impact campaign opportunities
each week. Surface pre-analyzed opportunities, let them act (complete/dismiss),
and fall back to BigQuery for deeper analysis when needed.

## Workflow

### 1. "What should I work on?" / "What campaigns should I run?"

Call `get_top_opportunities` FIRST. This returns pre-analyzed, ranked opportunities
with AI-generated reasoning. Do NOT re-analyze from scratch — the data is already
computed from the latest persona snapshot.

Present the top 3 with:
- Persona name + customer count
- Revenue at stake (segment_size × avg_ltv × conversion_rate)
- Urgency + action window ("< 14 days — recovery drops 50% after")
- One-line reasoning (WHY this segment, WHY now)

### 2. "Tell me more about [segment]"

Use `query_brand_data` to dig deeper into a specific segment:

```sql
-- Segment overview
SELECT persona, customer_count, avg_ltv, avg_aov, repeat_rate_pct,
  avg_days_since_last_order, avg_cadence_ratio
FROM `sproutron-ai.RB_Analytics.t_persona_summary`
WHERE persona = 'The Win-Back Target'

-- Top products in this segment
SELECT product_name, qty FROM `sproutron-ai.RB_Analytics.t_persona_top_products`
WHERE persona = 'The Win-Back Target' ORDER BY product_rank

-- Recency distribution within segment
SELECT
  CASE
    WHEN days_since_last_order BETWEEN 120 AND 180 THEN '120-180d (recoverable)'
    WHEN days_since_last_order BETWEEN 181 AND 270 THEN '181-270d (getting cold)'
    ELSE '271-365d (urgent)'
  END AS band,
  COUNT(*) AS customers
FROM `sproutron-ai.RB_Analytics.t_customer_personas`
WHERE persona = 'The Win-Back Target'
GROUP BY band ORDER BY band
```

### 3. "I already sent that" / "Mark Win-Back as done"

Call `complete_opportunity` with the persona name and any result notes.

Example: "Sent via Klaviyo Apr 22, 12% open rate, $3K attributed revenue"

### 4. "Skip the Burst Buyer" / "Not now for Gifters"

Call `dismiss_opportunity` with the persona name and a reason.

Always ask for a reason if the user doesn't provide one — it helps improve
future opportunity ranking.

### 5. "Download the segment"

Direct the user to the web app: "Download the segment from your Campaigns page
at aura.autointent.ai/agent/campaigns"

MCP cannot provide file downloads — the web app handles CSV exports.

## When to Use BigQuery Directly

If the pre-analyzed opportunities don't answer the question, fall back to
`query_brand_data` for custom analysis:

- "How many Win-Back customers bought Smoked Cheddar?" → custom SQL
- "What's the overlap between Gifters and Loyalists?" → custom SQL
- "Show me customers who unsubscribed but still purchase" → custom SQL

Available tables for direct queries:
- `t_customer_personas` — 1 row per customer with persona + metrics
- `t_customer_features` — 57 behavioral signals per customer
- `t_persona_summary` — aggregate stats per persona
- `t_copurchase_pairs` — product co-purchase graph
- `t_product_metrics` — product performance

## Rules

- Always call `get_top_opportunities` FIRST — don't re-analyze
- Present top 3 prominently, mention others exist
- Include revenue at stake and action window in every recommendation
- When completing/dismissing, log the reason for future learning
- Never expose raw customer emails in chat — use counts and summaries
