---
name: campaign-opportunities
description: >
  Surface top campaign opportunities, manage lifecycle, push segments to
  Klaviyo, and coordinate between Aura (intelligence) and Klaviyo (execution).
  Activate when user asks about campaigns, what to work on, segments, or
  anything involving email/SMS targeting.
metadata:
  author: sproutron-ai
  version: "2.0.0"
---

# Campaign Opportunities

## Purpose

Help the marketing team focus on the highest-impact campaign opportunities.
Aura provides the intelligence (who to target, why, when). Klaviyo provides
the execution (lists, flows, sending). This skill coordinates both.

## Tool Routing

Two MCP servers, different roles:

- **Aura** — historical intelligence + segmentation:
  segments, personas, opportunities, brand memory, BigQuery analytics
  Use for: "who to target", "why", "what's the opportunity"

- **Klaviyo** — real-time metrics + execution:
  live flow/campaign performance, subscriber status, list management, sending
  Use for: "how is it performing", "push this list", "create a flow"

ALWAYS use Aura first for WHO and WHY (get_top_opportunities, query_brand_data).
Use Klaviyo for HOW IT'S DOING and EXECUTE (performance, lists, flows).

## Workflow

### 1. "What should I work on?" / "What campaigns should I run?"

Call `get_top_opportunities` from Aura FIRST. Do NOT use Klaviyo tools for this.
This returns pre-analyzed, ranked opportunities with reasoning.

Present the top 3 with:
- Persona name + customer count
- Revenue at stake + action window
- Reasoning (WHY this segment, WHY now)
- Products to feature
- Signals explained (cadence ratio, engagement)

### 2. "Tell me more about [segment]"

Use Aura `query_brand_data` to dig deeper:

```sql
SELECT persona, customer_count, avg_ltv, avg_aov, repeat_rate_pct,
  avg_days_since_last_order, avg_cadence_ratio
FROM `sproutron-ai.RB_Analytics.t_persona_summary`
WHERE persona = 'The Win-Back Target'

SELECT product_name, qty FROM `sproutron-ai.RB_Analytics.t_persona_top_products`
WHERE persona = 'The Win-Back Target' ORDER BY product_rank
```

### 3. "Push this to Klaviyo" / "Create a list for Win-Back"

This is where Aura and Klaviyo coordinate:

**Step 1:** Use Aura `query_brand_data` to get the segment emails:
```sql
SELECT email FROM `sproutron-ai.RB_Analytics.t_customer_personas`
WHERE persona = 'The Win-Back Target'
```

**Step 2:** Use Klaviyo tools to create/update the list:
- Create list named "Aura — [persona name]"
- Add the profiles from Step 1

**Step 3:** Ask user: "Want me to create a flow for this list?"
- If yes: use Klaviyo tools to create a flow triggered by "Added to list"
- Set flow to MANUAL mode — nothing sends until user reviews
- Tell user: "Flow created in MANUAL mode. Open Klaviyo → Flows to design emails and set to LIVE."

**Step 4:** Use Aura `complete_opportunity` to mark it done.

### 4. "Exclude recent buyers" / "Filter this segment"

Use Aura `query_brand_data` for custom filtering:
```sql
SELECT email FROM `sproutron-ai.RB_Analytics.t_customer_personas`
WHERE persona = 'The Win-Back Target'
  AND days_since_last_order > 14
```

Then push the filtered list to Klaviyo (Step 2-3 above).
Do NOT try to create CSV files — push directly to Klaviyo.

### 5. "Set up flows for all personas"

One-time setup. Use Aura + Klaviyo together:

For each actionable persona:
1. Aura `query_brand_data` → get emails for that persona
2. Klaviyo → create list "Aura — [persona]"
3. Klaviyo → add profiles to list
4. Klaviyo → create flow (trigger: added to list, MANUAL mode)

Recommended personas for flows:
- The Win-Back Target → Win-Back email sequence (3 emails)
- The Reorder Ready → Reorder reminder (2 emails)
- The Loyalist - Lapsing → VIP re-engagement (2 emails)
- The Repeat Builder → Habit-building nurture (3 emails)
- The Curious Trier - Engaged → Second purchase nudge (2 emails)
- The Pantry Stocker - Lapsing → VIP urgent outreach (1 email)

Tell user: "All flows created in MANUAL mode. Design email content in Klaviyo,
then set each flow to LIVE. Lists will be refreshed automatically."

### 6. "I already sent that" / "Mark as done"

Call Aura `complete_opportunity` with notes.
Example: "Sent via Klaviyo Apr 22, 12% open rate"

### 7. "Skip this" / "Not now"

Call Aura `dismiss_opportunity` with reason.
Always ask for a reason if not provided.

## When to Use BigQuery Directly

Fall back to Aura `query_brand_data` for custom analysis:

- "How many Win-Back customers bought Smoked Cheddar?" → custom SQL
- "What's the overlap between Gifters and Loyalists?" → custom SQL
- "Show me customers who ordered 3+ times but stopped" → custom SQL

Available tables:
- `t_customer_personas` — 1 row per customer with persona + email
- `t_customer_features` — 57 behavioral signals per customer
- `t_persona_summary` — aggregate stats per persona
- `t_copurchase_pairs` — product co-purchase graph
- `t_product_metrics` — product performance

## Rules

- ALWAYS call Aura `get_top_opportunities` first — don't start with Klaviyo
- Use Klaviyo tools only when user wants to ACT (push, create list, create flow)
- All flows created in MANUAL mode — never auto-send
- Never expose raw customer emails in chat — use counts and summaries
- After pushing to Klaviyo, always call Aura `complete_opportunity`
- When filtering segments, push to Klaviyo directly — don't create CSV files
- Present top 3 opportunities, mention others exist
- Include revenue at stake and action window in every recommendation
