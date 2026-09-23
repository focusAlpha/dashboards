# FocusAlpha dashboards — three samples

Three working dashboards built on the [FocusAlpha](https://focusalpha.ai) financial
data API, published here as **samples**. Open any of them in a browser and it works
as-is — every control, sort and drawer — because the data is baked into the page.

| Sample | What it shows | Built on |
| --- | --- | --- |
| [screener-dashboard](screener-dashboard/index.html) | A global stock screener: guidance moves, filed events, ownership and fundamentals across 82 markets | `screen_companies` |
| [monitor-dashboard](monitor-dashboard/index.html) | A semiconductor news monitor: labelled events with direction, impact and the names each story reaches, across the US, Taiwan, Korea, Japan and China | `get_company_events` |
| [manager-profile-dashboard](manager-profile-dashboard/index.html) | A hedge-fund profile: what the firm says in its Form ADV brochure against what its 13F holds, with a replicating portfolio and peers | `get_institution_profile`, `get_institutional_holdings`, `get_market_data`, `get_benchmark_prices` |

## The data in these pages is a frozen sample

**Every number here is as of 2026-09-23** (the news board's events as of 2026-09-22
evening, US Eastern). Nothing in these files calls the API, so nothing in them will
ever update. They exist to show what the data looks like and how a page can use it.

**For current data, connect FocusAlpha in your AI** and let the page fetch live:
[focusalpha.ai/start](https://focusalpha.ai/start). Each sample has a **Build my own**
button that hands you a prompt to do exactly that in Claude — your universe, your
filters, your account.

## How a live version differs from these samples

Each page reaches its data through one call, `MCP.callTool(server, tool, input, opts)`.
In these samples that call reads the snapshot baked into the file. In a live page it
goes through the viewer's own FocusAlpha connector:

```js
const mcp = await window.claude.use('mcp');            // null when no connector is available
const r   = await mcp.callTool('FocusAlpha', 'screen_companies', input, { cache: { staleTime: 120000 } });
const rows = r.payload.data;
```

and the artifact is published with the connector declared:

```
capabilities: { mcp: { servers: [{ server: "focusalpha", tools: ["screen_companies"] }] } }
```

Declare the connector under the name it carries in your tool list (`focusalpha`); call it
at runtime by its display name (`FocusAlpha`).

## Things the data will punish you for getting wrong

- **Units differ by column.** Margins, yields and growth are fractions (`0.15` = 15%);
  price returns are percents (`15` = 15%).
- **A guidance raise is not good news.** A raise means a number went up; good news
  means better for the company. A cost line raised is both a raise and bad news. The
  screener keeps them in separate columns on purpose.
- **A 13F is not AUM.** It is long US-listed equity only — no shorts, options, non-US
  listings or cash. A falling line can be a drawdown or a move into assets the form
  does not cover, and the filing cannot tell those apart.
- **Multi-listed companies return one row per symbol per date** from `get_market_data`.
  Filter by symbol before you compute a return, or you will average two exchanges.
- **News direction and impact are a model's reading of an article** — not reporting,
  not a measurement, and not a price forecast. Knock-on rows are inferences drawn from
  filed relationships.
- **Filings are not in the news stream.** 8-K, TDnet, DART and HKEX filings come
  through `get_company_disclosures`.

## Licence

MIT — take the pages, keep the attribution notice. The data inside them is public
filing and market data served by FocusAlpha and is not investment advice.
