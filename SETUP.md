# Aura Setup Guide

## Step 1: Connect Aura MCP in Claude Team (one-time)

1. Open **Claude Team** → **Settings** → **Connectors**
2. Click **Add custom connector**
3. Enter:
   - Name: `Aura Brand Memory`
   - URL: `https://aura.autointent.ai/mcp`
4. Click **Add** → sign in with your company email
5. Approve the connection

## Step 2: Install Skills (each team member, one-time)

1. Download: [aura-brand-skills.zip](https://github.com/SproutronLabs/BrandSkills) → Code → Download ZIP
2. Open Claude Team → **Settings** → **Features** → **Custom Skills**
3. Click **Upload skill** → select the zip
4. Done — skills are active

## Step 3: Try It

Start a new conversation and ask:

**Campaign opportunities:**
> "What campaign opportunities do I have this week?"

**Deep dive a segment:**
> "Tell me more about the Win-Back Target segment"

**Push to Klaviyo:**
> "Preview Klaviyo sync for our persona segments"
> Then: "Yes, sync to Klaviyo"

**Brand memory:**
> "What do we know about Smoked Cheddar?"

**Quality check:**
> "Run a quality check on our brand memory"

## What's Available

**15 MCP tools:**
- Campaign opportunities (ranked with reasoning + products)
- Complete/dismiss lifecycle
- Klaviyo sync (preview → confirm → push with personalization properties)
- Brand memory (read/write/search/lint)
- BigQuery analytics (any SQL query)

**5 Skills:**
- brand-memory-lookup — read context before answering
- campaign-opportunities — Aura intelligence + Klaviyo execution
- learning-capture — write back durable patterns
- conversation-logger — log every Q&A
- data-quality-check — lint + validate claims

## Tips

- Say **"Use Aura..."** if Claude picks the wrong tool
- Klaviyo flows are **MANUAL mode** — nothing sends until you set to LIVE
- Brand memory compounds — the more you use it, the smarter it gets

## Web App

https://aura.autointent.ai

- Daily Flash (health score)
- Campaign opportunities (explainability + download)
- Personas (16 behavioral segments)
- Ask Aura (agent chat)

## Support

saranya@sproutron.com
