# ETF screener: fees, NAV, flows and holdings

Screen 5,607 US-listed ETFs (one row per share class) by classification, expense ratio, NAV and premium, 20-day fund flows and measured holdings. Built on FocusAlpha's `screen_etfs`.

- **[index.html](index.html)** — the sample as published at [app.focusalpha.ai/etf-screener-dashboard](https://app.focusalpha.ai/etf-screener-dashboard/). The whole active universe is in the page, with every number **as of 2026-09-03** (the newest valuation date in the dataset when it was frozen on 2026-09-23); it never updates.
- **[live.html](live.html)** — the same page with no data baked in. It reads everything through the viewer's FocusAlpha connector. Publish it as a claude.ai artifact with `capabilities: { mcp: { servers: [{ server: "FocusAlpha", tools: ["screen_etfs"] }] } }`.
- **Where to edit:** `PRESETS[0]` near the top of the script is the default screen; its keys are the `screen_etfs` parameters (`min_aum`, `max_expense_ratio`, `segment`, `tier`, `taxonomy_prefix`, `min_flow_20d`, `max_premium`, `sort`, …). Call `screen_etfs({ list_facets: true })` first for the exact spelling of every tier, asset class, category, segment and exchange; `get_etf_taxonomy` lists the L4 codes. Both are free.

Things this dataset will punish you for getting wrong: the wrapper tier (what kind of thing a fund is) and the four-level classification (what its holdings show) are two different questions; AUM is the class-level figure and a pool-level net-assets number is never a fund's size; a fund with no fee row is not a zero-fee fund (unit investment trusts such as SPY and GLD file none); expense ratios are decimals (`0.002` = 20 bp) and holdings shares are fractions.

For current data, connect FocusAlpha in your AI: [focusalpha.ai/start](https://focusalpha.ai/start). The **Build my own** button on the sample gives you the prompt that does all of this.
