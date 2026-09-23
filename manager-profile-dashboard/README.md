# Hedge fund profile: 13F holdings vs Form ADV

What a manager says in its Form ADV brochure against what its 13F holds, with a replicating portfolio measured against ^SP500TR and a peer set. Built on FocusAlpha's `get_institution_profile`, `get_institutional_holdings`, `get_market_data` and `get_benchmark_prices`.

- **[index.html](index.html)** — the sample as published at [app.focusalpha.ai/manager-profile-dashboard](https://app.focusalpha.ai/manager-profile-dashboard/). Data frozen as of 2026-09-23; it never updates.
- **[live.html](live.html)** — the same page with no data baked in. It reads everything through the viewer's FocusAlpha connector. Publish it as a claude.ai artifact with `capabilities: { mcp: { servers: [{ server: "FocusAlpha", tools: ["search_institutions", "search_companies", "get_institution_profile", "get_institutional_holdings", "get_market_data", "get_benchmark_prices"] }] } }`.
- **Where to edit:** `PEERS` near the top of the script: the first entry is the default fund, the rest are its peers (CIK and CRD each). Use `search_institutions({ q, has_style: true })` to look them up. A 13F is not AUM, and the replication is not the fund's return; keep both labels.

For current data, connect FocusAlpha in your AI: [focusalpha.ai/start](https://focusalpha.ai/start). The **Build my own** button on the sample gives you the prompt that does all of this.
