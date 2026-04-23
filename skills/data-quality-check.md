---
name: data-quality-check
description: >
  Cross-validate brand memory claims against live BigQuery data and run quality
  lint checks. Find stale, duplicate, or unquantified claims. Activate when user
  says "validate", "audit", "lint", "check the data", or "is this still true".
metadata:
  author: sproutron-ai
  version: "1.0.0"
---

# Data Quality Check

## Purpose

Ensure brand memory stays accurate over time. Cross-validates quantified claims
against live BigQuery data and runs deterministic lint checks to find quality
issues (empty articles, duplicates, stale entries).

## Prerequisites

- Aura Brand Memory MCP server connected (provides `lint_memory`, `list_brand_memory`,
  `query_brand_data`, `log_learning`)

## Workflow

### Step 1: Run Lint

Call `lint_memory` to get a quality report. This checks:
- Articles with 0 claims (empty)
- Duplicate claims within articles
- Claims without any numbers (unquantified)
- Articles not updated in 30+ days (stale)

If user wants auto-fix, call `lint_memory` with `fix: true` to remove duplicates.

### Step 2: Validate Claims Against Live Data

For articles flagged as stale or when user asks "is [X] still true":

1. Call `list_brand_memory` to get articles
2. For up to 5 articles with quantified claims, generate a validation SQL query.
   Example: If claim says "Smoked Cheddar: $1.2M revenue", generate:
   ```sql
   SELECT title, total_revenue FROM `sproutron-ai.RB_Analytics.t_product_metrics`
   WHERE title = 'Smoked Cheddar' LIMIT 1
   ```
3. Run each via `query_brand_data`
4. Compare: if the claim value differs from live value by > 20%, flag as stale

### Step 3: Correct Stale Claims

For each stale claim:
- Call `log_learning` with the corrected value from live data
- This overwrites the old claim (dedup handles it)

### Step 4: Report

Present a summary:
```
Brand Memory Quality Report
- Scanned: 45 articles
- Lint issues: 3 (1 empty, 2 unquantified)
- Claims validated: 12
- Stale claims corrected: 2
- All clear: 10
```

## Output Format

Markdown report with sections for lint issues and validation results.

## Best Practices

- Run this weekly or when data seems inconsistent
- Focus validation on high-impact entities (top products, largest personas)
- Always correct stale claims rather than just flagging them
- If an article is empty after lint, consider whether it should be deleted
