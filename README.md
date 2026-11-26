# Appraisily MCP (`com.appraisily/mcp`)

Public documentation, setup configs, and agent skill for Appraisily's hosted Model Context Protocol server:

- **Streamable HTTP endpoint:** `https://mcp.appraisily.com/mcp` (Authentication: None for public tools)
- **Discovery document:** `https://mcp.appraisily.com/.well-known/mcp.json`
- **Official MCP Registry:** `com.appraisily/mcp`
- **Docs & prompt examples:** https://appraisily.com/mcp
- **Agent skill:** [`skills/appraisily-mcp/SKILL.md`](./skills/appraisily-mcp/SKILL.md)

## What It Provides (Public, Read-Only, Zero API Key)

1. **Silver Hallmarks (`search_silver_hallmarks`, `get_silver_hallmark`)**
   - Maker mark texts, dates, origin, canonical `/marks/maker/<slug>` page links, catalog line-drawing WebP URLs, AI-assisted oxidized comparison views, and maker-level `soldToMelt` median/IQR ratios.
2. **Realized Auction Comparables (`search_auction_comparables`)**
   - Up to **6 realized auction lots per query** (`title`, realized `price.amount` + `currency`, `auctionHouse`, `date`, `sourceUrl`, and lot thumbnail `imageUrl`). Historical auction records reflect Appraisily's public catalog through August 2026.
3. **Artist Market Profiles (`search_artists`, `get_artist`)**
   - Public `/artist/<slug>/` summaries with lot counts, p25/median/p75 price bands, recent 12-month medians, top houses, and related artists.
4. **Appraisal Preparation & Sample Reports (`search`, `fetch`, `get_public_sample_report`, `create_appraisal_start_link`)**
   - Photo-preparation checklists, public insurance / tax / donation / estate sample reports, and safe `/start` intake links.

> **Important boundaries:** Public tools return historical auction signals and catalog context only — never authentication, legal/tax advice, or a signed appraisal. Oxidized mark images are AI-assisted visual comparison aids built from catalog drawings, not source photographs.

## Quick Setup

### Claude Code (CLI)
```bash
claude mcp add --transport http appraisily https://mcp.appraisily.com/mcp
```

Install the agent skill:
```bash
mkdir -p ~/.claude/skills/appraisily-mcp
curl -fsSL https://raw.githubusercontent.com/Appraisily/appraisily-mcp/main/skills/appraisily-mcp/SKILL.md -o ~/.claude/skills/appraisily-mcp/SKILL.md
```

### Claude Web / Desktop or ChatGPT (Custom Connector / Developer Mode)
Add a custom MCP connector pointing to:
- **URL:** `https://mcp.appraisily.com/mcp`
- **Authentication:** `None`

### Cursor (`.cursor/mcp.json`)
```json
{
  "mcpServers": {
    "appraisily": {
      "url": "https://mcp.appraisily.com/mcp"
    }
  }
}
```

### Antigravity (`mcp_config.json`)
```json
{
  "mcpServers": {
    "appraisily": {
      "serverUrl": "https://mcp.appraisily.com/mcp"
    }
  }
}
```

### Local Package (`npx`)
```bash
npx appraisily-mcp
```
For public lookups without local database credentials, connect directly to the hosted server `https://mcp.appraisily.com/mcp`.

## Example Prompts
- `Identify a Gorham silver hallmark, show the drawing and oxidized reconstruction URLs, and check sold-to-melt context.`
- `Search past auction results for a Tiffany desk lamp and list realized prices, houses, and sale dates.`
- `Look up Pablo Picasso on Appraisily and compare his recent 12-month median against his overall p25-to-p75 range.`
- `Use Appraisily to tell me what photos I need for an insurance appraisal and show a public sample report.`

## Links
- Setup & docs: https://appraisily.com/mcp
- Silver marks catalog: https://appraisily.com/marks
- Support: https://appraisily.com/contact?topic=mcp
- License: [MIT](./LICENSE)
