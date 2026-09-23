# Customer Service Dashboard

A dashboard for tracking customer inquiries — volume trends, SLA/response performance, and team workload — designed to work on mobile and desktop.

**Live prototype:** https://rmcowork.github.io/Customer-Service-Dashboard/

## Status

`index.html` is an interactive, self-contained HTML prototype (no build step) used to nail down layout, colors, and UX before writing the real app. Open it directly in a browser, or view it live via GitHub Pages (link above). It ships with generated sample data.

**Data sources (UI mocked, not yet wired to live backends):**
- CSV upload — actually parses the file client-side
- **Airtable — live**: paste a personal access token (`data.records:read`), base ID/URL and table in *Connect data → Airtable*. The token stays in your browser tab's memory and is never saved.
- Google Sheets, Helpdesk APIs (Zendesk / Intercom / HubSpot), generic REST/JSON — connect flow loads demo data as a stand-in until a real backend exists

**Views:** Overview (KPIs, volume trend, channel breakdown, workload), Inquiries (filterable list/table), Team (agent workload).

## Docs
- [spec.md](spec.md) — scope, data sources, views, and open decisions.
- [CLAUDE.md](CLAUDE.md) — context and conventions for Claude Code working in this repo.
- [practice.md](practice.md) — running log of what's been built and why.
