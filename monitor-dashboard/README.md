# Semiconductor news monitor, US and Asia

News as labelled events (direction, impact, the names each story reaches) for 36 chip companies in the US, Taiwan, Korea, Japan and China. Built on FocusAlpha's `get_company_events`, `get_company_news` and `get_market_data`.

- **[index.html](index.html)** — the sample as published at [focusalpha.ai/monitor-dashboard](https://focusalpha.ai/monitor-dashboard/). Data frozen as of 2026-09-23; it never updates.
- **[live.html](live.html)** — the same page with no data baked in. It reads everything through the viewer's FocusAlpha connector. Publish it as a claude.ai artifact with `capabilities: { mcp: { servers: [{ server: "FocusAlpha", tools: ["get_company_events", "get_company_news", "get_market_data"] }] } }`.
- **Where to edit:** `SNAP.companies` at the top of the script is the watchlist, one line per company (key, name, region, symbol, currency). `REGIONS` lists the region tabs. Direction and impact are FocusAlpha's model reading an article, not reporting; the page says so.

For current data, connect FocusAlpha in your AI: [focusalpha.ai/start](https://focusalpha.ai/start). The **Build my own** button on the sample gives you the prompt that does all of this.
