# Aura Setup Guide for Rebel Cheese

Set up Aura in Claude Team in ~5 minutes. Two pieces:
1. **MCP server** — gives Claude access to your brand data
2. **Skills** — instructs Claude how to use the data

---

## Step 1: Connect Aura MCP Server

In Claude Team, add the Aura connector:

1. Open **Claude Team** → **Settings** → **Connectors**
2. Click **+ Add connector**
3. Choose **Custom connector** (MCP)
4. Enter:
   - **Name:** `Aura Brand Memory`
   - **URL:** `https://aura.autointent.ai/mcp`
5. Click **Add** → it will redirect to Aura sign-in
6. Sign in with your **rebelcheese.com** Google account
7. Approve the connection

You're done with the MCP setup.

### Verify it works

Start a new Claude conversation and ask:
> "What tools do you have from Aura Brand Memory?"

You should see 10 tools listed:
- `get_brand_memory`, `search_brand_memory`, `list_brand_memory`, `get_brand_context`
- `query_brand_data` (BigQuery analytics)
- `log_learning`, `log_conversation`, `lint_memory`
- `review_recent_learnings`, `remove_claim`

---

## Step 2: Install Aura Skills

Skills tell Claude how to use the MCP tools — when to read brand memory first, when to log learnings, etc.

### Option A: Upload as zip (per user — easiest)

1. Download the skills zip:
   - **GitHub:** https://github.com/SproutronLabs/BrandSkills → Code → Download ZIP
   - Or ask Saranya for the latest `aura-brand-skills.zip`
2. In Claude Team → **Settings** → **Features**
3. Find the **Custom Skills** section
4. Click **Upload skill**
5. Upload the zip file
6. Skills are now active for your account

Each team member needs to do this individually.

### Option B: Skip skills (use MCP only)

You can use Aura without the skills. Claude will still have access to the tools but won't follow the read-first / write-back patterns automatically. You can manually ask things like:
- "Search brand memory for [topic]"
- "Log this insight to brand memory"

---

## Step 3: Test the Setup

### Test 1: Brand memory read
Ask Claude:
> "What do we know about Smoked Cheddar?"

Expected: Claude calls `search_brand_memory`, returns existing brand memory articles + sources.

### Test 2: Data query + write-back
Ask Claude:
> "What's the repeat rate for our top 5 products? Log the key insights to brand memory."

Expected:
1. Claude calls `query_brand_data` (queries BigQuery)
2. Returns the data
3. Calls `log_learning` for durable patterns (e.g., "Smoked Cheddar consistently #1 by repeat rate")

### Test 3: Quality check
Ask Claude:
> "Run a quality check on our brand memory"

Expected: Claude calls `lint_memory`, returns a report of empty/duplicate/unquantified/stale articles.

### Test 4: Review what's been logged
Ask Claude:
> "Show me what was recently logged to brand memory"

Expected: Claude calls `review_recent_learnings`, lists conversation-sourced claims for human review.

---

## What Aura Knows Today

When you connect, Aura already has:

**Brand Memory (Firestore):** ~66 articles compiled from your reviews, surveys, and brand docs:
- Products: Smoked Cheddar, Tomato Herb Fromage, Pepper Jack, etc.
- Personas: 14 customer segments (Reorder Ready, Win-Back Target, etc.)
- Themes: shipping, gifting, dietary preferences

**Data Warehouse (BigQuery):** Pre-computed intelligence:
- 102K customers across 14 personas
- Product metrics (revenue, repeat rate, entry point %)
- Co-purchase graph (which products are bought together)
- 30-day rolling baselines for anomaly detection

**Real-time data sources (synced via Airbyte):**
- Shopify orders, products, customers
- Meta Ads insights
- Google Ads campaigns
- Klaviyo profiles, events, flows

---

## Common Questions to Try

### Performance
- "What was revenue last 30 days vs prior 30?"
- "Which Meta campaigns have the best ROAS?"
- "How are our Klaviyo flows performing?"

### Customers
- "Who are our top 10 customers by LTV?"
- "How many subscribers haven't bought yet?"
- "Show me the Win-Back segment breakdown"

### Products
- "What products are bought together with Smoked Cheddar?"
- "Which seasonal products spike in November?"
- "What's the entry point rate for the Fromagier?"

### Strategy
- "What do we know about our gifting customers?"
- "Build a campaign for Reorder Ready customers"
- "What patterns have we learned about Meta ad fatigue?"

---

## Troubleshooting

### "Server disconnected" error
- Check the URL is exactly: `https://aura.autointent.ai/mcp`
- Re-authenticate via Settings → Connectors → reconnect

### "Unauthorized" error
- Token may have expired — disconnect and reconnect the MCP integration

### Skills not loading
- Skills require Pro/Max/Team plan with code execution enabled
- Each user must upload the zip individually (no team-wide install yet)

### Wrong data
- Some sources sync daily (Airbyte) — for "right now" data, ask Claude to use the live API
- Brand memory claims show their last-updated date — verify with fresh data if older than 30 days

---

## Need Help?

Contact: saranya@sproutron.com
GitHub: https://github.com/SproutronLabs/BrandSkills
