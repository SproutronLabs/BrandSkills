# Rebel Cheese — Email Template Playbook

Learned from 67 emails sent Jan–Apr 2026. Use this as the reference for generating new email content.

## Email Architecture

All Rebel Cheese emails follow a consistent structure:

```
[Header Image — full width, branded banner or product hero]
[Greeting — "Hi {{ first_name }}"]
[Body — 2-4 short paragraphs, conversational]
[Primary CTA — button or linked text]
[Product Grid — 2-4 products with images, titles, prices]
[Footer CTAs — SHOP ALL CHEESES | SHOP CURATED BOXES | SHOP BEST SELLERS]
[Unsubscribe + address footer]
```

### Layout Rules
- Max width: 600px
- Images are the primary content vehicle — most copy is in hero images, not HTML text
- Body text is short, personal, conversational — rarely more than 4 sentences
- Always opens with `Hi {{ first_name }}` (Klaviyo personalization)
- Always includes product recommendations grid using Klaviyo's `MayLikeBasedOnPurchaseHistory` feed
- Footer always has 3 shop links: All Cheeses, Curated Boxes, Best Sellers
- Klaviyo branding badge in footer

---

## 6 Template Categories

### 1. Founder's Note (Kirsten)

**Frequency:** Weekly (every Saturday)
**Tone:** Personal, storytelling, behind-the-scenes
**Structure:**
- Small header image (Kirsten's photo or factory shot)
- 3-5 paragraphs of personal story
- One link mid-text: "Read the full story" → blog
- Soft product mention in P.S. line
- No product grid — this is relationship, not selling
- Sign-off: "Co-founder, Rebel Cheese"

**Copy patterns:**
- Opens with a hook: "You could call us control freaks."
- Tells a specific story (factory build, Shark Tank prep, ingredient sourcing)
- Connects story to brand values (craft, quality, sustainability)
- P.S. line does the selling: "P.S. Our Mother's Day boxes are on sale..."

**When to use:** Relationship nurture. Best for Loyalists and Repeat Builders who already love the brand. Highest engagement per send.

**Example subject lines:**
- "The guy who saved our Brie"
- "We built our own factory. Here's why."

---

### 2. Product Drop / New Product

**Frequency:** 2-3x/month
**Tone:** Excited, appetizing, discovery
**Structure:**
- Full-width hero image (new product, beautifully styled)
- 1-2 sentences introducing the product
- Product details (flavor notes, ingredients, pairing suggestions)
- Primary CTA: "SHOP NOW" or "TRY IT FIRST"
- Product grid showing the new item + complementary products
- Often includes subscription upsell: "SUBSCRIBE & SAVE 10%"

**Copy patterns:**
- Short, appetizing descriptions
- Emphasizes what's NEW or LIMITED
- Pairing suggestions (wine, crackers, charcuterie)
- Urgency if limited: "Spring batch — while supplies last"

**When to use:** Reorder Ready, Repeat Builders, Loyalists. People who already buy and want to discover more.

**Example subject lines:**
- "April Fromagier is here"
- "Black Truffle Pecorino just dropped"
- "Swiss Fondue — last call"

---

### 3. Sale / Promotion

**Frequency:** 2-4x/month (clustered around events)
**Tone:** Urgent, value-focused, but still premium
**Structure:**
- Bold hero image with sale messaging baked into the image
- Minimal text — the image does the talking
- Multiple product images showing what's on sale
- Clear discount: percentage or "free" item
- Primary CTA: "SHOP THE SALE"
- Often sends 3-email sequence: announcement → reminder → last call

**Copy patterns:**
- Discount in subject line
- "Free" performs better than percentage off
- Always frames as premium/exclusive, never desperate
- Resend to non-openers with different subject line (PM resend pattern)

**When to use:** Win-Back Target (incentive to return), Curious Trier Dormant (incentive for first repeat). NOT for Loyalists (they don't need discounts).

**Example subject lines:**
- "Sitewide Sale + 2 free cheeses"
- "Free brie with your first order"
- "CA Sale — last chance"

---

### 4. Seasonal / Holiday

**Frequency:** Event-driven (Valentine's, Easter, Mother's Day)
**Tone:** Celebratory, gifting-focused, warm
**Structure:**
- Themed hero image (holiday styling, gift presentation)
- Short copy connecting cheese to the occasion
- Gift-specific CTAs: "SHOP MOTHER'S DAY BOXES"
- Gift set product grid (curated boxes, not individual cheeses)
- Often includes opt-out for the holiday: "Don't want Mother's Day emails?"

**Copy patterns:**
- Connects artisan cheese to celebration moments
- Emphasizes gifting: "Perfect for brunching, gathering, gifting"
- Starts 3-4 weeks before event, escalates urgency
- Holiday opt-out email sent first (respectful, builds trust)

**When to use:** Gifters, Seasonal Gifters. Also broad send to full list since holiday campaigns are time-bound.

**Example subject lines:**
- "Mother's Day boxes are here"
- "Easter last call"
- "Valentine's Box — 1 day left"

---

### 5. Education / Recipe

**Frequency:** 1-2x/month
**Tone:** Helpful, inspiring, community
**Structure:**
- Hero image (styled recipe photo or master class visual)
- Brief intro connecting to the content
- Recipe or event details
- CTA: "GET THE RECIPE" or "SIGN UP" (for master class)
- Product grid showing ingredients/featured cheeses

**Copy patterns:**
- Value-first — teaching, not selling
- Connects recipes to specific products (drives product discovery)
- Master class invitations build community and exclusivity
- Follow-up with recording for attendees

**When to use:** Curious Trier Engaged (value before asking for sale), Repeat Builders (deepen the relationship).

**Example subject lines:**
- "3 recipes with our Spring Box"
- "Spring Picnic Master Class — you're invited"
- "Awards Night Master Class recording"

---

### 6. Reorder / Win-Back (Aura Targeted)

**Frequency:** Triggered by persona entry (nightly sync)
**Tone:** Personal, gentle, acknowledges the gap
**Structure:**
- Warm header image (their favorite products)
- Personal greeting with acknowledgment: "It's been a while..."
- 1-2 sentences — why they should come back
- Subscription upsell: "SUBSCRIBE & SAVE 10%"
- Product grid from `MayLikeBasedOnPurchaseHistory` feed (personalized)
- No aggressive discounting for Reorder Ready

**Copy patterns (Reorder Ready):**
- "Whenever you're ready for your next round of Rebel Cheese, we've got you."
- Subscription suggestion as convenience, not discount
- Products shown are THEIR purchase history, not generic best sellers

**Copy patterns (Win-Back):**
- "We miss you" framing
- Feature THEIR previously purchased products
- 15% discount for Win-Back (not Reorder Ready)
- Urgency: "Welcome back offer expires in 7 days"

**When to use:** Reorder Ready (no discount, gentle), Win-Back Target (15% off, urgency), Loyalist Lapsing (VIP treatment, exclusive access).

**Example subject lines:**
- Reorder: "Time to restock?"
- Win-Back: "We miss you — 15% off your favorites"
- VIP: "Something exclusive for our best customers"

---

## Personalization Tags

All emails use these Klaviyo tags:

| Tag | Usage |
|-----|-------|
| `{{ person.first_name\|title\|default:'there' }}` | Greeting — always included |
| `{{ feeds.MayLikeBasedOnPurchaseHistory }}` | Product grid based on purchase history |
| `{{ item.title }}`, `{{ item.price }}`, `{{ item.image_full_url }}` | Product card details |
| `{{ item.url }}` | Product link |
| `{{ aura_persona }}` | Aura persona name (for conditional content) |
| `{{ aura_days_lapsed }}` | Days since last order |
| `{{ aura_ltv }}` | Lifetime value |
| `{{ aura_order_count }}` | Total orders |

---

## Product Featuring Rules

| Persona | Products to Show | Source |
|---------|-----------------|--------|
| Reorder Ready | Their previous purchases | `MayLikeBasedOnPurchaseHistory` |
| Win-Back Target | Their favorites + new products they missed | `MayLikeBasedOnPurchaseHistory` + latest drops |
| Loyalist Lapsing | Exclusive/new items + their staples | New products + purchase history |
| Curious Trier | Entry products (Fromagier box, best sellers) | `t_product_metrics` top entry_point_pct |
| Gifter | Gift sets, curated boxes | Curated boxes collection |
| General/Seasonal | Hero product (Cave-Aged Brie) + seasonal items | Seasonal collection |

---

## What Performs Best (from benchmark data)

| Metric | RB Average | Best Performing |
|--------|-----------|-----------------|
| Open rate | 55.3% | Founder's Notes (~60%+) |
| Click rate | 1.25% | Founder's Notes + Recipes |
| Conversion | 0.126% blended | **Aura targeted: 0.75%** (6x) |
| Revenue/recipient | $0.14 blended | **Aura targeted: $0.54** (3.9x) |

**Key insight:** Founder's Notes drive the highest engagement. Sale emails drive volume. Aura-targeted drives the highest conversion per send. The ideal mix combines all three based on the customer's relationship stage.

---

## Anti-Patterns (Don't Do)

- Don't send sale emails to Loyalists — they don't need discounts
- Don't send generic "we miss you" without featuring their actual products
- Don't send more than 2 emails/week to any customer
- Don't use aggressive urgency for Reorder Ready — they're already coming back
- Don't blast seasonal emails without offering holiday opt-out first
- Don't put important copy only in images (accessibility + deliverability)
- Don't discount more than 15% for Win-Back (they have brand affinity, start gentle)
