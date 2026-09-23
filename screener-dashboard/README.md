# Global stock screener across 82 markets

Screen listed companies on guidance changes, filed events, ownership and fundamentals. Built on FocusAlpha's `screen_companies`.

- **[index.html](index.html)** — the sample as published at [focusalpha.ai/screener-dashboard](https://focusalpha.ai/screener-dashboard/). Data frozen as of 2026-09-23; it never updates.
- **[live.html](live.html)** — the same page with no data baked in. It reads everything through the viewer's FocusAlpha connector. Publish it as a claude.ai artifact with `capabilities: { mcp: { servers: [{ server: "FocusAlpha", tools: ["screen_companies"] }] } }`.
- **Where to edit:** `PRESETS[0]` near the top of the script is the default screen; its keys are the `screen_companies` parameters. Call `screen_companies({ list_facets: true })` first for the exact spelling of countries, sectors, exchanges and event families.

For current data, connect FocusAlpha in your AI: [focusalpha.ai/start](https://focusalpha.ai/start). The **Build my own** button on the sample gives you the prompt that does all of this.
