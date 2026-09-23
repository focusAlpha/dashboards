# FocusAlpha Dashboards — stock screener, semiconductor news monitor, hedge fund 13F profile

Three working financial dashboards built on the [FocusAlpha](https://focusalpha.ai) data API,
published as open-source **samples**. Each is one self-contained HTML file. Open it in a browser
and every control, sort and drawer works, because the data is baked into the page.

**Live pages:** [focusalpha.ai/dashboards](https://focusalpha.ai/dashboards/) ·
**GitHub Pages mirror:** [focusalpha.github.io/dashboards](https://focusalpha.github.io/dashboards/)

| Sample | What it shows | FocusAlpha tools behind it | Files |
| --- | --- | --- | --- |
| **Global stock screener** | Screen listed companies across 82 markets on guidance changes, filed events, ownership and fundamentals | `screen_companies` | [sample](https://focusalpha.ai/screener-dashboard/) · [source](screener-dashboard/index.html) · [live source](screener-dashboard/live.html) |
| **Semiconductor news monitor** | News as labelled events (direction, impact, the names each story reaches) for 36 chip companies in the US, Taiwan, Korea, Japan and China | `get_company_events`, `get_company_news`, `get_market_data` | [sample](https://focusalpha.ai/monitor-dashboard/) · [source](monitor-dashboard/index.html) · [live source](monitor-dashboard/live.html) |
| **Hedge fund profile** | What a manager says in its Form ADV brochure against what its 13F holds, with a replicating portfolio and peers | `get_institution_profile`, `get_institutional_holdings`, `get_market_data`, `get_benchmark_prices` | [sample](https://focusalpha.ai/manager-profile-dashboard/) · [source](manager-profile-dashboard/index.html) · [live source](manager-profile-dashboard/live.html) |
| **Get in Claude** | The guide page: the prompt to paste first, sample questions, what the data covers, and the three dashboards above with their Build-my-own prompts | — | [page](https://focusalpha.ai/get-in-claude/) · [source](get-in-claude/index.html) |

## The samples are frozen: data as of 2026-09-23

Every number in the three `index.html` files is **as of 2026-09-23** (the news monitor's events as
of the evening of 2026-09-22, US Eastern). Nothing in those files calls the API, so nothing in them
will ever update. They exist to show what the data looks like and how a page can use it.

**For current data, connect FocusAlpha in your AI** ([focusalpha.ai/start](https://focusalpha.ai/start))
and build the live version below.

## Build your own live version

Each dashboard has two files:

- `index.html` — the sample, with a data snapshot baked in. Works anywhere, never updates.
- `live.html` — the **same page with no data in it**. Every number is read through the viewer's
  FocusAlpha connector (MCP) when the page opens. Small (60–115 KB), one file, no dependencies.

The **Build my own** button on every sample hands you a prompt for Claude. The prompt fetches
`live.html` from this repository, publishes it as an artifact with the FocusAlpha connector
declared, and then applies your conditions: your filters, your watchlist, your fund. You can do the
same by hand:

1. Connect FocusAlpha in your AI: [focusalpha.ai/start](https://focusalpha.ai/start).
2. Fetch the live source, for example
   `https://raw.githubusercontent.com/focusAlpha/dashboards/main/screener-dashboard/live.html`.
3. Publish it as a claude.ai artifact with the connector declared, e.g.
   `capabilities: { mcp: { servers: [{ server: "FocusAlpha", tools: ["screen_companies"] }] } }`.
   Without the declaration `window.claude.use('mcp')` resolves `null` and the page cannot fetch.
4. Edit the one block that is yours: `PRESETS[0]` (screener), `SNAP.companies` (monitor),
   `PEERS` (fund profile). Each `live.html` says so in a comment at the top.

Opened as a plain web page, without a connector, `live.html` shows only its empty state.

## How the page reaches its data

Every page touches its runtime through one call, `MCP.callTool(server, tool, input, opts)`
(the monitor also uses `MCP.watchTool` for the feeds). In `index.html` that call reads the
snapshot baked into the file; in `live.html` it goes through the viewer's connector:

```js
const mcp = await window.claude.use('mcp');            // null when no connector is available
const r   = await mcp.callTool('FocusAlpha', 'screen_companies', input, { cache: { staleTime: 120000 } });
const rows = r.payload.data;
```

Declare the connector under the name it carries in your tool list (`focusalpha` in Claude Code);
call it at runtime by its display name (`FocusAlpha`).

## Things the data will punish you for getting wrong

- **Units differ by column.** Margins, yields and growth are fractions (`0.15` = 15%);
  price returns are percents (`15` = 15%).
- **A guidance raise is not good news.** A raise means a number went up; good news
  means better for the company. A cost line raised is both a raise and bad news. The
  screener keeps them in separate columns on purpose.
- **A 13F is not AUM.** It is long US-listed equity only: no shorts, options, non-US
  listings or cash. A falling line can be a drawdown or a move into assets the form
  does not cover, and the filing cannot tell those apart.
- **Multi-listed companies return one row per symbol per date** from `get_market_data`.
  Filter by symbol before you compute a return, or you will average two exchanges.
- **News direction and impact are a model's reading of an article**, not reporting,
  not a measurement, and not a price forecast. Knock-on rows are inferences drawn from
  filed relationships.
- **Filings are not in the news stream.** 8-K, TDnet, DART and HKEX filings come
  through `get_company_disclosures`.

## Licence

MIT. Take the pages, keep the attribution notice. The data inside them is public filing and
market data served by FocusAlpha and is not investment advice.
