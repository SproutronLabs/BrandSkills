# Aura Brand OS — Feature Guide

## What is Aura?

Aura is a brand context layer that connects to your data (Shopify, Meta Ads, Google Ads, Klaviyo) and gives you an AI marketing teammate that actually knows your business. It works in two places:

1. **Claude Team** — ask questions, get campaign recommendations, push to Klaviyo, all in chat. Requires two one-time setup steps: connect the Aura MCP server + install one skill.
2. **Web App** (aura.autointent.ai) — visual dashboards for daily health, campaigns, personas

---

## 1. Campaign Opportunities

**What it does:** Analyzes your 16 behavioral customer segments every day and surfaces the highest-impact campaign opportunities, ranked by revenue potential and urgency.

**What you see:**
- Top 3 "Focus this week" campaigns with full reasoning
- Why this campaign (persona-specific explanation, not generic)
- Signals from your store (cadence ratio, engagement score, LTV, AOV)
- Estimated revenue recovery (segment size x LTV x conversion rate)
- Action window (how long before the opportunity decays)
- Top products to feature (based on actual purchase data for that segment)
- Download CSV of the segment for manual use

**How to use in Claude:**
> "What campaign opportunities do I have this week?"
> "Tell me more about the Win-Back Target segment"
> "What products should I feature for Reorder Ready customers?"

**How to use in Web App:**
Campaign Studio page — click any opportunity to see the full detail panel.

**Why it matters:** Instead of guessing which customers to target, Aura tells you exactly who is at risk, why, and what to do — with the math behind it.

---

## 2. Behavioral Personas (16 Segments)

**What it does:** Every customer is classified into one of 16 behavioral personas based on 54 signals — order frequency, recency, LTV, cadence ratio, gift behavior, subscription status, engagement, and more. Segments refresh nightly.

**The 16 personas:**

| Persona | Who they are | Typical action |
|---------|-------------|----------------|
| The Loyalist | 4+ orders, buying on cadence | Delight & retain |
| The Loyalist - Lapsing | 4+ orders, going cold (180-365d) | VIP re-engagement |
| The Loyalist - Lost | 4+ orders, 365d+ silent | Last chance outreach |
| The Pantry Stocker | Highest LTV, bulk buyers | Replenishment reminders |
| The Pantry Stocker - Lapsing | Top VIP going quiet | Urgent personal outreach |
| The Reorder Ready | 2-3 orders, in their reorder window | Gentle reminder (no discount) |
| The Repeat Builder | 2-3 orders, building habit | Nurture content |
| The Repeat - Nudge Needed | 2-3 orders, cadence slipping | Timely nudge |
| The Repeat - Other | 2+ orders, no clear pattern | Monitor |
| The Win-Back Target | 2+ orders, 120-365d lapsed | 15% off + their favorite products |
| The Win-Back - Hard | 2+ orders, 365d+ lapsed | Hail mary offer |
| The Curious Trier - Engaged | 1 order, actively engaging | Second purchase nudge |
| The Curious Trier - Dormant | 1 order, low engagement | Reactivation campaign |
| The Gifter | Buys for others (gift notes, diff shipping) | Gift sets + "share with a friend" |
| The Seasonal Gifter | 50%+ orders in Nov/Dec | Calendar-triggered campaigns |
| The Browser | Email subscriber, never purchased | First purchase incentive |

**How to use in Claude:**
> "How many customers are in each persona?"
> "What's the average LTV of our Win-Back Targets?"
> "Show me the overlap between Gifters and Loyalists"

**How to use in Web App:**
Personas page — view all 16 segments with stats, download any segment as CSV.

---

## 3. Klaviyo Integration

**What it does:** Pushes persona segments directly to Klaviyo as lists with behavioral properties attached to each profile. This lets you build Klaviyo flows that use Aura's intelligence for personalization.

**What gets synced to each Klaviyo profile:**
- `aura_persona` — which of the 16 segments they belong to
- `aura_days_lapsed` — days since their last order
- `aura_ltv` — lifetime value
- `aura_order_count` — total orders
- `aura_avg_order_value` — average order value

**Currently synced personas:**
1. The Win-Back Target → List: "Aura — The Win-Back Target"
2. The Reorder Ready → List: "Aura — The Reorder Ready"
3. The Loyalist - Lapsing → List: "Aura — The Loyalist - Lapsing"

**How lists stay fresh:**
- Nightly refresh after persona materialization (4 AM CT)
- New customers entering a segment are added automatically
- Customers who purchase are removed automatically
- No manual list management needed

**Safety guardrails:**
- All flows are created in MANUAL mode — nothing sends until you review and set to LIVE
- Klaviyo sync requires explicit confirmation ("Yes, sync to Klaviyo")
- Preview shows exactly what will change before any write happens

**How to use in Claude:**
> "Preview Klaviyo sync for our persona segments"
> (Review the preview, then) "Yes, sync to Klaviyo"

**How to use in Klaviyo:**
After sync, go to Klaviyo → Lists → find "Aura — [persona]" → create a flow triggered by "Added to list". Use the `aura_*` properties for dynamic content (e.g., show different copy based on `aura_days_lapsed`).

---

## 4. Brand Memory

**What it does:** A persistent knowledge base that accumulates everything Aura learns about your brand — product insights, customer patterns, campaign results, brand voice guidelines. The more you use Aura, the smarter it gets.

**What's stored:**
- **Products** — what customers say in reviews, best sellers, co-purchase patterns
- **Personas** — behavioral insights, what messaging works for each segment
- **Campaigns** — what you've run, results, learnings
- **Brand** — voice guidelines, positioning, values
- **Themes** — seasonal trends, category insights

**How it compounds:**
1. You ask Aura a question
2. Aura reads brand memory for context before answering
3. After answering, Aura writes back any new qualitative insight it discovered
4. Next time someone asks a related question, that learning is already there

**Quality gates (automatic):**
- No duplicate claims (SHA256 dedup)
- No PII (emails blocked)
- Claims must be substantive (min 20 characters)
- Rate limited (max 10 learnings per session)
- Qualitative patterns only — no metric snapshots that go stale

**How to use in Claude:**
> "What do we know about Smoked Cheddar?"
> "What's our brand voice?"
> "Run a quality check on our brand memory"

---

## 5. BigQuery Analytics (Ask Anything)

**What it does:** Ask any quantitative question about your business in plain English. Aura translates it to SQL and runs it against your data warehouse — Shopify orders, Meta Ads, Google Ads, Klaviyo, and all pre-computed persona tables.

**Available data:**
- Orders, revenue, AOV, customer behavior (Shopify)
- Ad spend, ROAS, CTR, frequency, impressions (Meta + Google)
- Email engagement (Klaviyo)
- 16 persona segments with 54 signals per customer
- Product co-purchase graph
- Product performance metrics

**How to use in Claude:**
> "What's our revenue trend for the last 30 days?"
> "Which products are most commonly bought together?"
> "How many customers bought Smoked Cheddar and then came back for more?"
> "What's our Meta Ads ROAS this week vs last week?"
> "Show me customers who ordered 3+ times but stopped in the last 90 days"

**Safety:** Read-only queries only. Cannot modify any data.

---

## 6. Daily Flash (Web App)

**What it does:** Every morning, Aura generates a health score (30-95) and a briefing letter covering revenue, orders, customer retention, ad performance, and top recommendations.

**What you see:**
- Overall health score with trend
- Revenue and order metrics (vs previous period)
- Retention metrics (repeat rate, reorder interval)
- Ad performance (ROAS, spend efficiency)
- Top 3 prioritized recommendations with reasoning
- Dive Deep into any topic for detailed analysis

**How to use:**
Visit aura.autointent.ai → Daily Flash page. Refreshes automatically at 5 AM CT.

---

## 7. Conversation Logging & Audit Trail

**What it does:** Every interaction with Aura — tool calls, questions, analyses — is logged to BigQuery with timestamps, latency, and user attribution. This creates a complete audit trail of what was asked, what was answered, and what actions were taken.

**What's tracked:**
- Every MCP tool call (which tool, arguments, latency, success/failure)
- User email (who made the request)
- Chat conversations (question + answer summary)
- Dive Deep analyses (topic, tools used, latency)

**Why it matters:** Full observability into how the team is using Aura, what questions are being asked, and what's driving value.

---

## 8. Data Quality Check

**What it does:** Runs automated quality checks on brand memory to catch issues before they compound.

**What it checks:**
- Empty articles (0 claims)
- Duplicate claims within the same article
- Stale articles (not updated in 30+ days)
- Auto-fix option removes duplicates

**How to use in Claude:**
> "Run a quality check on our brand memory"
> "Lint brand memory and fix duplicates"

---

## Setup: What Gets Installed

**One MCP server** (Aura) — connects Claude to your data. 15 tools for reading brand memory, querying BigQuery, managing campaigns, and syncing to Klaviyo.

**6 skills** (uploaded individually via drag-and-drop) — teach Claude how to use those tools:
1. **Root router** — routes to the right sub-skill based on user intent
2. **Brand memory lookup** — read context before answering any brand question
3. **Campaign opportunities** — surface and act on the highest-impact segments
4. **Learning capture** — write back durable patterns after data analysis
5. **Conversation logger** — log every Q&A for audit trail
6. **Data quality check** — validate and lint brand memory

One-time setup per team member. See SETUP.md for instructions.

---

## How Everything Connects

```
Shopify + Meta + Google + Klaviyo
        ↓ (Airbyte sync)
    BigQuery Data Warehouse
        ↓ (nightly materialization, 4 AM CT)
    54 signals → 16 Personas
        ↓
   ┌────────────────────────────────┐
   │         Aura MCP Server        │
   │  (15 tools, OAuth-protected)   │
   └──────┬──────────────┬──────────┘
          ↓              ↓
   Claude Team      Web App
   (chat + skills)  (dashboards)
          ↓              ↓
       Klaviyo      Campaign Studio
    (auto-sync)     (visual decisions)
```

**The compounding loop:**
1. Data flows in from 4 sources nightly
2. Aura materializes 54 signals into 16 personas
3. Campaign opportunities are ranked by revenue potential
4. Team acts on recommendations (via Claude or web app)
5. Results flow back as new data → Aura learns → better recommendations

---

## Quick Reference: What to Ask

| Goal | Ask this |
|------|----------|
| What to work on | "What campaign opportunities do I have?" |
| Understand a segment | "Tell me about the Win-Back Target" |
| Push to Klaviyo | "Preview Klaviyo sync" → "Yes, sync" |
| Product insights | "What do we know about Smoked Cheddar?" |
| Custom analysis | "How many customers bought 3+ times this quarter?" |
| Ad performance | "What's our Meta ROAS this week?" |
| Quality check | "Run a quality check on brand memory" |
| Brand context | "What's our brand voice for email?" |

---

## Support

Questions or issues: saranya@sproutron.com
