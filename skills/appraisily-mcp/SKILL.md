---
name: appraisily-mcp
description: Use Appraisily's MCP connector to check silver hallmarks (catalog drawings, oxidized reconstructions, sold-to-melt context) and past auction prices (up to 6 sold lots), look up public artist pages, run appraisal-preparation workflows, public sample-report lookup, tracked Appraisily start links, and credential-gated Pro Screener case-packet/comparable-evidence support. Use when a user wants to connect Appraisily to ChatGPT, an MCP client, a local agent, or a professional Pro Screener workflow.
---

# Appraisily MCP

Use the hosted remote MCP server first:

```text
https://mcp.appraisily.com/mcp
```

For public users, keep the workflow to appraisal preparation:

- explain which appraisal path may fit
- list photos and object details to gather
- show public sample-report links
- create tracked Appraisily start links
- search past auction results with `search_auction_comparables`, returning at most 6 lots per query as market signals
- look up public artist pages with `search_artists` and open one `/artist` page with `get_artist`
- search silver hallmarks with `search_silver_hallmarks` and open one `/marks` maker page with `get_silver_hallmark`; hosted oxidized images are reconstructions, not authentication
- do not claim private customer-order, uploaded-media, payment, or raw database access
- do not present auction results, artist pages, or hallmarks as a signed appraisal or final value conclusion

For professional users, use Pro Screener tools only when the MCP session is authenticated with an Appraisily professional credential:

- prepare case packets
- flag missing facts, missing photos, review risks, and next appraiser questions
- search comparable auction evidence through Appraisily Auction Data Service
- create tracked Pro Screener workspace links
- review existing authorized Pro Screener session status/results

Maintain the professional boundary:

- comparable sales are research evidence for review
- Appraisily MCP does not issue a signed appraisal inside ChatGPT
- do not present a final value conclusion, legal advice, tax advice, or appraiser certification without human professional review

## Ready-made workflows

Each workflow stays inside the public boundary: catalog and market context, never a value conclusion or authentication. When the user needs a value for their own item, offer the signed appraisal link the tools return.

### Silver collectors

1. `search_silver_hallmarks` with the mark text or symbols the user reads off the piece (for example `lion anchor G`, `STERLING`, a maker name).
2. `get_silver_hallmark` on the best match to show the maker page: mark variants, dates, the catalog line drawing, and the oxidized reconstruction.
3. Ask the user to compare their mark with the drawing first, then with the oxidized view (the reconstruction shows how a worn, tarnished mark tends to look). Say which details differ.
4. If sold-to-melt context is present, report it as how that maker's pieces sold relative to melt value, not as a price for the user's piece.
5. State the limit: a matching mark narrows the maker; it does not authenticate the object, date it exactly, or value it.

Prompts:

- "My spoon has a lion, an anchor and a G. Which maker is it? Show the drawing and the oxidized view."
- "Compare the Gorham mark variants on Appraisily and tell me which dates fit a mark with a date letter."
- "Is there sold-to-melt context for this maker, and what does it mean for a teapot?"

### Estate and vintage resellers

1. For each candidate piece, run `search_auction_comparables` with a short, specific description (maker, form, material). Report at most 6 sold lots with price, house, and date.
2. For silver, run `search_silver_hallmarks` on the mark first; use the maker it returns to make the auction search specific.
3. Triage in one line per piece: sold lots found or not, the price spread, the closest match, and what would need checking in hand (marks, weight, condition, signatures).
4. Flag pieces where the sold lots disagree or the match is weak, instead of averaging them.

Prompts:

- "I have a Gorham sterling pitcher, a Tiffany Favrile vase and a signed Picasso lithograph. Give me a quick sold-lot check for each."
- "Search sold auction results for Georg Jensen silver bowls and show the strongest matches."

### Appraisers (authenticated Pro Screener)

1. `prepare_pro_screener_case_packet` with the object facts to list missing photos, missing facts, review flags, and next questions for the client.
2. `search_pro_screener_comparables` for comparable-sales evidence; keep the source link and sale date for each lot.
3. `start_pro_screener_run` for a private, white-label Lotbook PDF from the signed-in account; poll `get_pro_screener_status`, then `get_pro_screener_pdf`.
4. Present the comparables as research evidence for the appraiser's own analysis. The appraiser decides the value and signs the report.

Prompts:

- "Prepare a Pro Screener case packet for a 19th-century oil on canvas, 24 x 30 in, signed lower right."
- "Find comparable auction sales for a Roseville Pinecone vase and list sale dates and sources."

## Setup

Use the public onboarding page when a user needs setup instructions:

```text
https://appraisily.com/mcp
```

Machine-readable discovery: `https://mcp.appraisily.com/.well-known/mcp.json`.
