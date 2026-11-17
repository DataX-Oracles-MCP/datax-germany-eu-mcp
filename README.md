# DataX Germany & EU Truth Engine — MCP Server (`stdio`)

Zero-dependency **Model Context Protocol (MCP)** `stdio` bridge (`Python 3.9+` standard library only — no `pip install` required) connecting AI agents (Claude Desktop, Cursor, OpenClaw, custom MCP clients) to live official **Germany & EU** registries with non-custodial pay-per-call settlement in **USDC** (Base, Polygon, Solana) and **MiCA EURC** (Base, Solana).

- **Live Endpoint:** `https://datax-de.duckdns.org`
- **Machine Discovery:** [`/llms.txt`](https://datax-de.duckdns.org/llms.txt) · [`/mcp.json`](https://datax-de.duckdns.org/mcp.json) · [`/.well-known/agent-card.json`](https://datax-de.duckdns.org/.well-known/agent-card.json) · [`/openapi.json`](https://datax-de.duckdns.org/openapi.json) · [`/.well-known/ai-crawl-pricing.json`](https://datax-de.duckdns.org/.well-known/ai-crawl-pricing.json)
- **Agent Payment Protocol & Wallet Guide:** See [`SKILL.md`](./SKILL.md)

---

## Official Data Sources Indexed

- **German Public Procurement (`oeffentlichevergabe.de`):** Daily federal/state/municipal tenders (`PC`) and contract awards (`PA`) with tender values, CPV codes, single-bidder offer counts (`Ofertas: N`), and corporate winners (`DE\d{9}`, `GmbH`, `AG`, `SE`, `UG`, `KG`). Natural-person contacts and PII are stripped per GDPR.
- **BaFin Financial Supervision (`bafin.de` RSS):** Enforcement measures (`Maßnahmen`), fines, and supervisory notices (`Sec MV`).
- **German Federal Commercial & Procurement Law (`gesetze-im-internet.de`):** Full official XML texts of `GmbHG`, `AktG`, `HGB`, `InsO`, and `GWB` (`Sec LC`).
- **EU Registries:** **EU TED v3** procurement (`DEU`, `Sec TE`), **EU SEDIA** grants (`Sec EU`), and **EU Consolidated Financial Sanctions List (`FSF`, `Sec SF`)**.

---

## MCP Tools Exposed (7 Tools)

| Tool | Price (`USDC` / `EURC`) | Purpose |
| :--- | :---: | :--- |
| `get_pricing` | **Free** | Fetch supported networks, contracts, treasuries, and tier prices. |
| `get_catalog` | **Free** | Check live freshness (`data_version`) and record counts by section. |
| `search_tenders` | **`0.10`** | FTS5 BM25 + BI filter search across Germany & EU registries; returns opaque `item_ref` (`dx_...`). |
| `find_distressed_auctions` | **`0.10`** | Real-estate auction endpoint (populated on Spain node; empty on Germany-only DB). |
| `find_funded_companies` | **`0.10`** | Screens companies winning public contract awards in Germany. |
| `check_company_risk` | **`0.30`** | Corporate KYB cross-check by `cif` (`DE...`/`HRB...`/`LEI`) or `company_name` across BaFin, EU FSF & awards. |
| `get_tender_detail` | **`0.50`** | Unlocks full verbatim record, winner corporate IDs, CPVs & official URLs from any `item_ref`. |

---

## Quick Setup (Claude Desktop / Cursor)

```json
{
  "mcpServers": {
    "datax-germany-eu": {
      "command": "python3",
      "args": ["/absolute/path/to/mcp_bridge.py"],
      "env": {
        "DATAX_BASE_URL": "https://datax-de.duckdns.org",
        "DATAX_TOKEN": "<unspent-utxo-token-from-issue-token>"
      }
    }
  }
}
```
