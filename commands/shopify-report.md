# /shopify-report — ESSCO Sales Intelligence Brief

Trigger: "shopify report" / "what's selling" / "give me a demand snapshot"
Purpose: Pull live ESSCO sales data via Shopify MCP connector, map to
audience tribes, and generate content seeds for MARVIS — automatically.

Store: esscoair-craft.myshopify.com
Connector: Shopify MCP (connected — use run-analytics-query tool)
Caveat: Shopify = RETAIL ONLY (~half the business). Wholesale POs from
Sporty's/Pilot Mall/Aircraft Spruce bypass Shopify. Label all output RETAIL-ONLY.

## Steps

1. **Pull top-20 SKUs** by net_sales, trailing 365 days:
   ```
   FROM sales SHOW gross_sales, net_sales, orders
   GROUP BY product_title ORDER BY net_sales DESC LIMIT 20
   SINCE -365d UNTIL today
   ```

2. **Pull recent velocity** (last 30 days) for comparison:
   ```
   FROM sales SHOW gross_sales, net_sales, orders
   GROUP BY product_title ORDER BY net_sales DESC LIMIT 20
   SINCE -30d UNTIL today
   ```

3. **Map each SKU to an audience tribe:**
   - Cessna 172/182/210 → Student pilots + GA owners
   - Piper Cherokee/Warrior/Arrow → GA owners (largest retail segment)
   - Beechcraft Bonanza/Baron → High-end owner/restorer
   - Military (T-6, T-38, etc.) → Collector/restorer/military
   - Avsoft products → Flight school / corporate
   - Generic/multi-aircraft → Broad GA

4. **Generate content seeds** for MARVIS:
   For each of the top 5 tribes:
   - Best-selling aircraft in that tribe
   - One story angle (pride of ownership / era / sound / restoration moment)
   - One save-bait content idea (type-specific knowledge = archive moat)
   - One share-bait idea (relatable tribe content)

5. **Print the report:**
```
ESSCO SALES INTELLIGENCE — [date] (RETAIL-ONLY — excludes wholesale)

TOP 5 TRIBES BY RETAIL REVENUE (trailing 12mo)
1. [tribe] — $[net] / [N] orders | top SKU: [product]
2. [tribe] — $[net] / [N] orders | top SKU: [product]
3. [tribe] — $[net] / [N] orders | top SKU: [product]
4. [tribe] — $[net] / [N] orders | top SKU: [product]
5. [tribe] — $[net] / [N] orders | top SKU: [product]

30-DAY VELOCITY MOVERS (up vs 12mo avg)
  [product] — [N] orders this month vs [N] avg | signal: [why]

CONTENT SEEDS FOR MARVIS
Tribe 1 [name]:
  Story angle: [one sentence]
  Save-bait: [one sentence]
  Share-bait: [one sentence]
[repeat for top 3 tribes]

KOSH BLITZ ALIGNMENT
  Week [N] tribe ([tribe]) — top SKU: [product] — [N] orders trailing 12mo
  This tribe is [hot/warm/cool] in the data.

WHOLESALE BLIND SPOT
  This data covers retail only. Dale's gut estimate for wholesale top movers:
  [ask Dale if not already known and bank in memory]
```

6. **Flag anything surprising** — a SKU spiking or dropping unexpectedly.
   One sentence. Don't over-analyze.

## Hard Rules
- RETAIL-ONLY label on every output. Don't imply this is the full picture.
- The best-seller list is an AUDIENCE MAP, not a product-promo list.
  Entertain the operator. The manual is the soft proof.
- Never use this data to suggest "promoting best-sellers" — the play is
  homage content for that tribe, not ads for the SKU.
- Shopify connector pulls live data — no stale training data answers.
  If the connector fails: flag and ask Dale to run a manual pull.
