# Wokelo MCP Server

**Dealmaking research for AI assistants - company intelligence, transaction data, and research deliverables via MCP.**

[Website](https://wokelo.ai/) · [Documentation](https://docs.wokelo.ai/getting-started-mcp)

---

**The problem with AI assistants for dealmaking research**

You are likely already using an AI assistant - Claude, ChatGPT, Copilot, or similar. But every one of them shares the same core limitations: training data with a cutoff date, no access to premium sources, a reliance on web search that surfaces the same SEO-optimized headlines everyone else sees, and the need for constant back-and-forth prompting just to get to a usable result.

**How Wokelo MCP supercharges your existing tools**

Wokelo MCP is built to solve every one of those limitations for dealmaking research - bundled premium subscriptions, a native company and transactions database, zero hallucination, a proprietary news algorithm to surface signals your competitors aren't seeing, and a single query that delivers a finished deliverable without you staying in the loop.

|  | Generic AI assistant | Wokelo MCP | Relevant for |
| --- | --- | --- | --- |
| Company data | Open web + training data with a cutoff date; misses emerging or less-covered players | 100M+ entity-resolved company database queried directly | Company discovery & enrichment |
| Premium sources | Inaccessible, requires separate subscriptions | 30+ pre-integrated premium sources, bundled | Company research |
| Transaction data | No native deal database | Exhaustive fundraises + M&A database; every acquirer/target entity-resolved | Deal & M&A research |
| Signal quality | Web search returns SEO-optimized headlines, the same results everyone else sees | Proprietary news algorithm surfaces hidden signals not biased by headlines | Company discovery, research, monitoring & alerts |
| Source integrity | Source credibility unpredictable; no deduplication or entity resolution | Thousands of verified global publishers; deduplicated, entity-resolved | Monitoring & alerts |
| Accuracy | 10-40% hallucination rate on structured queries; compounds with list length | Zero hallucination, every data point source-verified | End-to-end dealmaking research |
| Bulk enrichment | Manual re-prompting every 20-25 rows; credits exhausted across sequential calls | Full list enriched in a single MCP call | Company enrichment |
| Output format | Raw links or text requiring human synthesis; never client-ready | Structured briefing or finished PDF/PPT in your firm's template | Research deliverables |
| Methodology | Cannot codify or consistently apply firm-specific frameworks | Firm methodology codified once; applied consistently on every run | Research deliverables |
| Workflow | Requires active involvement throughout | Trigger and pick up the finished deliverable asynchronously | Research deliverables |

---

## Quickstart

Wokelo MCP is a hosted remote server. No local installation required. Connect using the server URL and authenticate once via browser.

**Server URL:**

```
https://mcp.wokelo.ai/mcp
```

**Transport:** Streamable HTTP
**Authentication:** OAuth 2.0 with Dynamic Client Registration (browser-based login on first connection)

### Claude Desktop (Recommended)

Claude Desktop connects to remote OAuth servers through its built-in Connectors UI:

1. Open **Claude Desktop**
2. Go to **Settings > Connectors**
3. Click **Add MCP Server**
4. Paste the server URL: `https://mcp.wokelo.ai/mcp`
5. Complete the OAuth login in your browser
6. All 25 Wokelo tools will appear automatically

> **Note:** Remote MCP servers with OAuth authentication are added through the Connectors UI, not through the `claude_desktop_config.json` file. This is by design in Claude Desktop.

### Claude Code

Run the following in your terminal:

```bash
claude mcp add --transport http wokelo https://mcp.wokelo.ai/mcp
```

Then open a Claude Code session and run `/mcp` to complete the OAuth authentication flow. Your browser will open for sign-in.

**Verify:** Run `/mcp` again — `wokelo` should appear as connected with its tool list.

### OpenAI Codex (CLI and IDE extension)

Codex shares MCP configuration between the CLI and the IDE extension via `~/.codex/config.toml`.

**Step 1 — Add the server** (either command or direct file edit):

```bash
codex mcp add wokelo --url https://mcp.wokelo.ai/mcp
```

Or add to `~/.codex/config.toml` directly:

```toml
[mcp_servers.wokelo]
url = "https://mcp.wokelo.ai/mcp"
```

**Step 2 — Complete OAuth sign-in:**

```bash
codex mcp login wokelo
```

This opens your browser for authentication. Codex stores the tokens locally and refreshes them automatically. The `codex mcp login` command works only for Streamable HTTP servers that advertise OAuth — which Wokelo does.

**Verify:** Start a Codex session (`codex`) and run `/mcp` in the TUI. Confirm `wokelo` lists its tools. The IDE extension reflects the same configuration automatically.

### Microsoft Copilot Studio

Copilot Studio has a built-in MCP onboarding wizard that handles OAuth 2.0 with Dynamic Client Registration (DCR) — which Wokelo supports — so no Swagger file or custom connector is required.

1. Open your agent in **Copilot Studio** → go to the **Tools** page
2. Select **Add a tool** → **New tool** → **Model Context Protocol**
3. In the onboarding wizard, fill in:
   - **Server name:** `Wokelo`
   - **Server description:** Dealmaking research — company intelligence, transactions, and research deliverables
   - **Server URL:** `https://mcp.wokelo.ai/mcp`
4. Under **Authentication**, select **OAuth 2.0**
5. For the OAuth type, select **Dynamic discovery**
6. Click **Create**, then **Next**
7. On the **Add tool** dialog, select **Create a new connection** → complete the browser sign-in
8. Click **Add to agent**

The agent orchestrator uses the server description to decide when to call Wokelo tools, so keep the description clear and specific.

> **Note:** Generative Orchestration must be enabled on the agent for MCP tools to be invoked. Copilot Studio dropped SSE support in August 2025; Streamable HTTP (what Wokelo uses) is the supported transport.

### Cursor

Cursor 1.0+ supports OAuth and Streamable HTTP natively:

1. Go to **Cursor > Settings > Cursor Settings > MCP**
2. Click **Add MCP Server**
3. Paste the server URL: `https://mcp.wokelo.ai/mcp`
4. Complete the OAuth login when prompted
5. Click the refresh icon in the MCP panel if the tool list doesn't appear immediately

### Perplexity (Pro / Enterprise)

Custom remote connectors are available to paid Perplexity users and support OAuth 2.0 with Dynamic Client Registration natively.

1. Open **Perplexity → Settings → Connectors**
2. Click **+ Custom connector** in the top-right, then select **Remote**
3. Fill in:
   - **Name:** Wokelo
   - **MCP Server URL:** `https://mcp.wokelo.ai/mcp`
   - **Transport:** Streamable HTTP
   - **Authentication:** OAuth 2.0 — leave Client ID and Client Secret blank (Wokelo supports DCR, so endpoints are auto-discovered via `/.well-known/oauth-protected-resource/mcp`)
4. Check the acknowledgement box and save
5. Complete the browser sign-in when prompted
6. Toggle the Wokelo connector on under **Sources** on the Perplexity home screen

> **Enterprise note:** Organization-wide sharing requires an admin to share the connector from the Permissions screen after creation. With OAuth, admins can either authenticate once on behalf of the organization or require each member to authenticate individually.

### Windsurf

1. Open **Windsurf Settings > MCP**
2. Add a new server with URL: `https://mcp.wokelo.ai/mcp`
3. Complete the OAuth login when prompted

### VS Code / GitHub Copilot

In VS Code 1.101+, open the Command Palette → **MCP: Open User Configuration** and add:

```json
{
  "servers": {
    "wokelo": {
      "type": "http",
      "url": "https://mcp.wokelo.ai/mcp"
    }
  }
}
```

Save the file. VS Code picks up the change automatically and prompts for OAuth sign-in on first tool use. Check status in the Chat view's **Configure Tools** menu.

> **Note on keys:** VS Code uses `servers` (plural). This is **different** from the `mcpServers` key used by Claude Desktop's legacy config and most other clients — don't confuse the two.

### Visual Studio 2022 (17.14+) / Visual Studio 2026

Visual Studio supports remote MCP servers with OAuth via an `mcp.json` file at the solution or user level:

```json
{
  "servers": {
    "wokelo": {
      "type": "http",
      "url": "https://mcp.wokelo.ai/mcp"
    }
  }
}
```

Use the **Manage Authentication** CodeLens action in the `mcp.json` file to trigger the OAuth browser flow.

### GitHub Copilot CLI

Inside an interactive Copilot CLI session:

```
/mcp add
```

Select **HTTP**, then provide:
- **URL:** `https://mcp.wokelo.ai/mcp`
- **Server ID:** `wokelo`
- **Tools:** `*`

Or edit `~/.copilot/mcp-config.json` directly:

```json
{
  "mcpServers": {
    "wokelo": {
      "type": "http",
      "url": "https://mcp.wokelo.ai/mcp"
    }
  }
}
```

**Verify:** `/mcp show wokelo`

### Google Antigravity

> **Note:** Antigravity's native handling of OAuth with remote MCP servers has been inconsistent in publicly reported cases. Try the native config first; if the OAuth flow fails or hangs, use the `mcp-remote` bridge fallback.

Open **Agent pane → `…` menu → MCP Servers → Manage MCP Servers → View raw config**.

**Option A — Native config (try first):**

```json
{
  "mcpServers": {
    "wokelo": {
      "serverUrl": "https://mcp.wokelo.ai/mcp"
    }
  }
}
```

Save, click **Refresh**, and complete OAuth when prompted.

**Option B — `mcp-remote` bridge (fallback, requires [Node.js](https://nodejs.org/)):**

```json
{
  "mcpServers": {
    "wokelo": {
      "command": "npx",
      "args": [
        "-y",
        "mcp-remote@latest",
        "https://mcp.wokelo.ai/mcp"
      ]
    }
  }
}
```

`mcp-remote` handles the OAuth flow in your system browser and proxies the connection to Antigravity as a local stdio server.

### Any Other MCP Client

Wokelo works with any MCP client that supports Streamable HTTP transport and OAuth 2.0. Point your client at:

```
https://mcp.wokelo.ai/mcp
```

**Full connection details:**

| Field | Value |
|---|---|
| URL | `https://mcp.wokelo.ai/mcp` |
| Transport | Streamable HTTP |
| Authentication | OAuth 2.0 with Dynamic Client Registration |
| Resource metadata | `https://mcp.wokelo.ai/.well-known/oauth-protected-resource/mcp` |

On first connection, your client opens a browser tab for authentication. After sign-in, tokens are stored by your client and refreshed automatically.

**If your client only supports stdio (not Streamable HTTP):** use `mcp-remote` as a bridge:

```json
{
  "mcpServers": {
    "wokelo": {
      "command": "npx",
      "args": ["-y", "mcp-remote@latest", "https://mcp.wokelo.ai/mcp"]
    }
  }
}
```

### Verifying the server before connecting a client

To confirm the server is healthy independently of any client, use the official MCP Inspector:

```bash
npx @modelcontextprotocol/inspector
```

In the Inspector UI, choose **Streamable HTTP**, enter `https://mcp.wokelo.ai/mcp`, click **Connect**, and sign in. If Inspector lists tools under the **Tools** tab, the server is working correctly — any compliant client should also work.

---

## Tools

Currently, Wokelo MCP exposes **25 tools** across two categories.

### Read-Only Tools (16)

| Tool | Description |
| --- | --- |
| `Search Company` | Search Wokelo's database of 100M+ companies by name, sector, geography, or other criteria. |
| `Get Single Company Enrichment` | Retrieve a comprehensive company profile including firmographics, financials, funding history, growth metrics, team data, and culture signals. |
| `Get Company News` | Fetch real-time company news using Wokelo's proprietary algorithm. Deduplicated, entity-resolved, sourced from thousands of verified global publishers. |
| `Get News Article Detail` | Retrieve the full content and details of a specific news article. |
| `Get Earnings Calls` | Fetch earnings call transcripts for a public company. |
| `List Earnings Calls` | List available earnings call transcripts for a company. |
| `Get Employee Reviews` | Pull employee review data (Glassdoor) including culture scores, leadership ratings, sentiment analysis, and trending themes. |
| `Get Product Reviews` | Pull software and product review data (G2) including customer satisfaction, feature perception, and NPS signals. |
| `Get SEC Filings` | Retrieve SEC filings (10-K, 10-Q, 8-K, etc.) for a public company. |
| `List Notebooks` | Retrieve previously generated research reports and notebooks. |
| `Download Notebook` | Download a completed notebook's structured data. |
| `List Workflows` | Retrieve the catalog of available workflow templates and their configurations. |
| `Get Request Status` | Check the status and progress of any async job (enrichment, screening, workflow). |
| `Get Request Result` | Retrieve the final result data from a completed async job. |
| `Poll Request to Completion` | Poll a request until completed or failed and return the final result. |
| `Wait for Request Result` | Submit a request and automatically poll until complete, then return the result. |

### Write Tools (9)

| Tool | Description |
| --- | --- |
| `Submit Company Enrichment` | Submit batch enrichment requests for companies covering firmographics, financials, funding, hiring, and alternative data across multiple entities in a single call. |
| `Submit Company Intelligence` | Request AI-generated deep intelligence for companies including narrative analysis, competitive positioning, risk assessment, and strategic insights. |
| `Submit Company Peers` | Discover peer and competitor companies for a given company. |
| `Submit Buyer Screening` | Identify potential acquirers for a target company based on strategic fit, deal history, and sector alignment. |
| `Submit Target Screening` | Identify potential acquisition targets for an acquirer based on strategic criteria, sector focus, and deal parameters. |
| `Submit Industry Enrichment` | Request sector-level market intelligence covering trends, market size, key players, growth drivers, and competitive dynamics. |
| `Submit Market Map` | Discover and map companies in a specific market or competitive landscape. |
| `Start Workflow` | Trigger a pre-configured research workflow built in Wokelo's no-code agent builder. Returns finished deliverables (PDF/PPT) asynchronously. |
| `Cancel Request` | Cancel a pending or in-progress async request. |

---

## Data Sources

Wokelo bundles 30+ premium data sources so you don't manage separate subscriptions:

| Firmographics | Hiring & job postings |
| --- | --- |
| Financials | Employee reviews |
| Regulatory filings | Software reviews |
| Transactions & deals | Patents & IP |
| Web & app traffic | Social media |
| News & media | Podcasts & interviews |
| Business reviews | Academic & scientific publications |

---

## Use Cases

### Company Enrichment - Core Datasets

> *"For Cleo (meetcleo.com) - do a data-backed analysis across firmographics, funding rounds and investors, recent news, UK financials, employee trends, products, website traffic."*

Your AI assistant calls `Get Single Company Enrichment`, pulls verified data across all relevant dimensions, and returns a structured briefing in one turn. Works for well-known and obscure companies alike - structured data is aggregated across sources that don't talk to each other (e.g. one funding round from PitchBook and one from a news article).

### Company Enrichment - Bulk

> *"Find the top LLM observability startups in the US who raised funding in the last 3 years. Enrich each with funding stage, amount raised, key investors, and key products. Output an Excel file."*

Uses `Search Company` to identify the universe, then `Submit Company Enrichment` to enrich every row in a single pass. No re-prompting per row. Results compound in bulk - LLMs struggle when dealing with multiple companies and multiple structured datasets simultaneously.

### Company Enrichment - Earnings and Filings

> *"Look at Snowflake's last 4 earnings calls. Extract these themes from each call in a grid view: AI commentary, product launches, management changes, financial outlook, risks. Add exact quote excerpts for each cell."*

Uses `Get Earnings Calls` and `List Earnings Calls` to pull structured transcripts from proprietary datasets (including S&P). Raw transcripts enable nuanced analysis for granular insights that get missed in open-ended Q&A without Wokelo.

### Company Enrichment - Alternative Datasets

> *"Look at Glassdoor reviews of Ensemble Health Partners. Prepare an analysis of gaps and improvement areas. Also prepare an interactive UI to view review summary and all individual reviews."*

Uses `Get Employee Reviews` and `Get Product Reviews` to pull all raw review data. Getting all raw data is typically difficult for an LLM, and without raw data, granular insights are patchy. Especially useful for portfolio teams and product teams tracking individual reviews.

### Company Discovery - Market Map

> *"I am looking at flooring companies in US - founded at least 10 years ago, with focus on manufacturers. I need a long list of companies with details and analytics of players."*

`Submit Market Map` generates comprehensive market maps in seconds. LLMs typically produce only 30-40 companies max without hallucinations. Wokelo's market map also enriches each company with structured data and supports filters for geography, employees, type, funding, and more.

### M&A Target Screening

> *"We're looking at acquisition targets in construction tech vertical SaaS. Find companies with $5-50M revenue, Series A-C, strong product reviews."*

`Submit Target Screening` identifies targets matching your criteria. Follow up with `Submit Company Intelligence` for deep dives on the shortlist.

### Buyer Identification

> *"Who are the most likely acquirers for a mid-market cybersecurity MSSP?"*

`Submit Buyer Screening` identifies potential acquirers based on strategic fit, deal history, and sector alignment.

### Hybrid - Buyer Screening + Portfolio Analysis

> *"For Warburg Pincus - prepare a dashboard of all M&A and investments. Create themes around portcos nearing their term. Select a sample portco nearing harvest period and find 30 top buyers with key metrics and rationale."*

This is an example of a hybrid query made possible by Wokelo's width of data and insights. Leverages curated algorithms like buyer screening + target screening alongside structured M&A and investment data not available via web search.

### News Analysis - On-Demand

> *"Find all news related to lawsuits and litigations related to Gen-AI. Look at news of OpenAI, Anthropic, Google, Microsoft, Cohere, Perplexity. Summarize in an insightful table."*

`Get Company News` provides real-time, deduplicated, time-series news with categorization and date filtering. Can be used for nuanced analysis of specific news types across multiple companies for cluster, peer set, or industry analysis. LLM web search is generally not exhaustive for periodic news.

### News Analysis - Scheduled Monitoring

> *"Every Monday morning, send me a digest of key developments across our portfolio companies from the prior week."*

Set up as a recurring scheduled task (via Claude Cowork or similar). Build your own news monitoring dashboard that runs on a cadence of your choosing - daily, hourly, weekly. `Get Company News` monitors thousands of publishers and returns deduplicated, entity-resolved intelligence on each run.

### Peer Sentiment Analysis

> *"We have a value creation review for Make.com next week. Pull employee and software review data for them and their three closest peers. Compare culture scores and leadership ratings. Flag themes."*

Uses `Submit Company Peers` to identify peers, then `Get Employee Reviews` and `Get Product Reviews` to pull alt-data from Glassdoor and G2 that a generic LLM cannot reliably access.

### Trigger Wokelo Reports

> *"Do a peer comparison workflow comparing OpenAI and Anthropic in Wokelo."*

`Start Workflow` triggers pre-built workflows without needing to visit the Wokelo platform. Can trigger frequently used workflows or custom workflows your org has access to. After completion, use the structured JSON output to create deliverables in your format of choice (3-page Word doc, 5-page PPT, etc.).

---

## Examples

See Wokelo MCP in action with real outputs:

| Use Case | Sample Query | Live Output |
| --- | --- | --- |
| Company enrichment (core) | Cleo data-backed analysis | [View output](https://claude.ai/share/2871f791-d8c8-46f9-9eb5-2b317786f2ee) |
| Company enrichment (headcount) | Mitra Chem headcount insights | [View output](https://claude.ai/share/06f1fdab-08b9-4bd4-a849-e33b3e0456fc) |
| Bulk enrichment | LLM observability startups table | [View output](https://claude.ai/share/92ebe241-ecbe-455d-8b70-36419beaea43) |
| Earnings analysis | Snowflake earnings calls grid | [View output](https://claude.ai/share/c724bb42-e762-4224-999e-b469cbb4a41b) |
| Alternative data | Ensemble Health Partners reviews | [View output](https://claude.ai/share/16616452-3eae-4c76-9ac1-9d0ed2ad623d) |
| Market map | US flooring manufacturers | [View output](https://claude.ai/share/aa4d5ae4-f434-4bcf-bab9-3a673283c9a0) |
| Hybrid buyer screening | Warburg Pincus portfolio + buyers | [View output](https://claude.ai/share/01a8323d-7974-491d-ae51-31de23456050) |
| News analysis | Gen-AI litigation tracker | [View output](https://claude.ai/share/f380d846-d0cf-4010-855d-32ffb3f9aea5) |
| Trigger workflow | OpenAI vs Anthropic peer comparison | [View output](https://claude.ai/share/8495dcec-34a0-40a7-b199-ee06f5c640bd) |

[Browse all sample outputs](https://claude.ai/project/019d8f9c-d573-76e0-83e4-a8a7ec7ed82d)

---

## How It Works

```
┌─────────────────┐     MCP (Streamable HTTP)     ┌──────────────────┐
│                  │ <──────────────────────────── │                  │
│  Your AI Client  │         OAuth 2.0             │  Wokelo Platform │
│  (Claude, etc.)  │ ────────────────────────────> │                  │
│                  │                                │  100M+ companies │
└─────────────────┘                                │  30+ data sources│
                                                   │  Transaction DB  │
  You ask a question                               │  News algorithm  │
  |                                                │  Workflow engine │
  Your AI calls Wokelo tools                       └──────────────────┘
  |                                                        |
  Wokelo returns verified,                                 |
  structured, source-cited data                  ──────────┘
  |
  Your AI synthesizes the answer
```

1. **Connect** - Add the server URL to your MCP client. Authenticate once via OAuth.
2. **Ask** - Query your AI assistant in natural language. It determines which Wokelo tools to call.
3. **Receive** - Wokelo returns verified, structured data. Your assistant synthesizes and presents it.
4. **Async support** - Long-running jobs (enrichment, workflows) run in the background. Use `Poll Request to Completion` or `Wait for Request Result` to pick up results when ready.

---

## Requirements

- An MCP client that supports **Streamable HTTP** transport and **OAuth 2.0**
- A Wokelo account - [sign up at wokelo.ai](https://wokelo.ai/)
- No local installation, packages, or API keys to manage

## Supported Clients

| Client | Setup Method | OAuth Handling | Status |
| --- | --- | --- | --- |
| Claude Desktop | Settings > Connectors > Add MCP Server | Native | ✅ Supported |
| Claude Code | `claude mcp add --transport http` | Native via `/mcp` | ✅ Supported |
| OpenAI Codex (CLI + IDE) | `codex mcp add --url` + `codex mcp login` | Native | ✅ Supported |
| Microsoft Copilot Studio | MCP onboarding wizard → OAuth 2.0 → Dynamic discovery | Native (DCR) | ✅ Supported |
| Cursor | Settings > MCP > Add MCP Server | Native | ✅ Supported |
| Perplexity (Pro/Enterprise) | Settings > Connectors > Custom connector > Remote | Native (DCR) | ✅ Supported |
| Windsurf | Settings > MCP | Native | ✅ Supported |
| VS Code / GitHub Copilot | `mcp.json` (`servers` key) | Native | ✅ Supported |
| Visual Studio 2022 (17.14+) / 2026 | `mcp.json` + Manage Authentication CodeLens | Native | ✅ Supported |
| GitHub Copilot CLI | `/mcp add` or `~/.copilot/mcp-config.json` | Native | ✅ Supported |
| Google Antigravity | `mcp_config.json` (native or `mcp-remote` fallback) | See notes | ⚠️ Works via fallback if native fails |

---

## Troubleshooting

| Symptom | Likely cause | Fix |
| --- | --- | --- |
| Browser never opens for sign-in | Client's OAuth callback port blocked | Check local firewall; some clients let you configure the callback port |
| 401 Unauthorized on every call after sign-in | Stale or corrupted token | Clear stored tokens in your client and reconnect — the client should re-register automatically via DCR |
| "Tools: (none)" or empty tool list | Client connected but didn't fetch tool list | Restart the client or click its refresh/reconnect action |
| Connection refused / DNS errors | Network or proxy issue | Confirm reachability: `curl -I https://mcp.wokelo.ai/mcp` |
| Antigravity OAuth hangs or errors | Known inconsistency in Antigravity's native remote OAuth | Switch to the `mcp-remote` bridge config (Option B in Antigravity section) |

To isolate whether an issue is client-side or server-side, test the server with MCP Inspector (see the verification section above). If Inspector connects and lists tools, the server is fine and the issue is with the client configuration.

---

## Security & Compliance

- **OAuth 2.0** - No API keys stored in config files. Browser-based authentication with token refresh.
- **SOC 2 Type II Compliant** - Independently audited security controls for data handling, availability, and confidentiality.
- **ISO 27001 Certified** - Information security management system certified to international standards.
- **No data leaves Wokelo** - Queries are processed on Wokelo's infrastructure. Responses are returned to your MCP client.

---

## Documentation

Full setup guides, API reference, and tutorials:

[**docs.wokelo.ai/getting-started-mcp**](https://docs.wokelo.ai/getting-started-mcp)

---

## Support

- **Documentation:** [docs.wokelo.ai/getting-started-mcp](https://docs.wokelo.ai/getting-started-mcp)
- **Email:** sales@wokelo.ai
- **Website:** [wokelo.ai](https://wokelo.ai/)

---

## License

Proprietary. Usage is subject to the [Wokelo Terms of Service](https://wokelo.ai/legal).

---

Wokelo AI - Intelligence infrastructure for dealmakers.
