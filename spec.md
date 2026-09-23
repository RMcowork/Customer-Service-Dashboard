# Customer Service Dashboard — Spec

## Purpose
A dashboard for tracking customer inquiries, built for both mobile and desktop, with an elegant/neutral visual style and the ability to load data from multiple sources.

## Build approach
1. **Prototype first** (current stage) — an interactive, self-contained HTML file (`index.html`) with generated sample data, used to validate layout, color, and UX before writing real application code. Deployed via GitHub Pages for easy viewing/sharing.
2. **Real code** (next stage, not started) — once the design is approved, rebuild as a proper app (tech stack TBD — React/Vite is the default recommendation) with real backend connectors replacing the demo-data stand-ins.

## Deployment
`index.html` is served directly by GitHub Pages from the `main` branch root — no build step. Pushing to `main` updates the live site (may take a minute to propagate).

## Data sources
The dashboard should be able to load inquiry data from:
- **CSV / Excel upload** — user drags in an exported file; parsed client-side. *(Implemented for real in the prototype.)*
- **Airtable** — *(Implemented for real.)* Reads a table via the Airtable REST API directly from the browser. The user supplies a personal access token (`data.records:read`), base ID (or a pasted Airtable URL), table name/ID, and optional view. Pages through all records (cap 5,000). Fields are auto-detected by name (subject/title, customer/name, channel, status, priority, assignee, created date, plus optional first-response/resolution/overdue fields) with an optional manual field mapping. The token is held in memory only — never saved, never committed — and is cleared from the form after connecting; a Refresh button re-uses it for the session and Disconnect drops it. Base ID, table, view and mapping (not the token) are remembered in localStorage. Linked-record fields come back as record IDs, so a lookup field is needed for names. Airtable calls work on the GitHub Pages site or a local file; the Claude artifact preview blocks outbound requests.
- **Google Sheets** — pull rows live from a shared sheet.
- **Helpdesk API** — Zendesk, Intercom, or HubSpot Service Hub.
- **Generic REST/JSON endpoint** — any URL returning a JSON array (or `{data: [...]}` / `{results: [...]}`) of inquiries.

### Field normalization (CSV and Airtable)
Real data is never padded with made-up values. Status text is mapped to Open/Pending/Resolved/Closed (e.g. "In progress" → Pending, "Done" → Resolved), priority to Urgent/High/Medium/Low, channel to Email/Chat/Phone/Social/Other; anything unrecognized falls back to Open / Medium / Other. First-response and resolution times come only from real fields (explicit numbers, or timestamps relative to the created date) and show "—" when absent. "Overdue" uses an explicit field if present, otherwise "open for more than 4 days". Assignees are taken from the data, not a fixed list. A "Source" bar under the top bar shows where the data came from, the record count, freshness, and any fields that fell back to defaults.

> In the prototype, the Google Sheets, Helpdesk API and REST connectors are UI-complete (platform/URL/token fields, a "Connect" action) but load bundled demo data instead of calling a real endpoint, since there's no backend yet.

See [practice.md](practice.md) for a running log of what's been built, and [CLAUDE.md](CLAUDE.md) for repo conventions.

## Loading and refresh behavior
- While an Airtable pull is running (first connect or Refresh), the Source bar shows a spinning icon, a sliding progress bar and a live "N records so far" count; the KPI/chart cards pulse, and the Connect button shows a spinner.
- When the data lands, the cards fade in. A load is held visible for at least 0.7s so fast responses don't just flash.
- All of this is disabled under `prefers-reduced-motion`.
- **Auto-refresh:** a connected Airtable source re-pulls every 60 seconds. Background updates are quiet (small spinner and "Updating…" in the Source bar only: no card pulse, no toast). The timer pauses while the browser tab is hidden and catches up immediately when it becomes visible, never overlaps a load in progress, and resets after any manual Refresh. A toggle in the Source bar shows "Auto-refresh on · next in Ns" and can turn it off (preference saved in localStorage). If the token is rejected (401/403) or three updates in a row fail, auto-refresh stops, the last good data stays on screen, and the toggle becomes "stopped · retry". Only live sources (Airtable) auto-refresh; CSV and demo data don't. Disconnect stops the timer.
- Source bar actions for Airtable: **Refresh** (re-pulls using the in-memory token) and **Disconnect** (drops the token and returns to demo data).

## Security and privacy
- The repo and GitHub Pages site are public: no secrets in code, commits, logs or storage. The Airtable token is entered by the viewer, kept only in memory, and cleared from the form after connecting.
- Only non-secret Airtable settings (base ID, table, view, field mapping) are saved to localStorage.
- Inquiry text is customer-authored, so every data-derived string is HTML-escaped before rendering.
- Network calls go only to the chosen data source (currently `api.airtable.com`); no analytics or third-party requests. With auto-refresh on, that is roughly one pull per minute per open tab (Airtable allows 5 requests/second per base, and a 5,000-record table needs at most 50 requests per pull).

## Core views
- **Overview** — KPI tiles (total inquiries, open & pending, avg first response time, SLA compliance %), inquiry volume trend (last 30 days), inquiries-by-channel breakdown, team workload summary.
- **Inquiries** — searchable, filterable (status / channel / priority) list of individual inquiries. Table on desktop, cards on mobile. **Download CSV** exports the currently filtered rows.
- **Team** — per-agent workload: open inquiries, average resolution time.

## Data export
- The Inquiries view has a "Download CSV" button that exports whatever rows match the current filters/search (not necessarily the full dataset).
- A global "Download data" button (sidebar on desktop, icon in the top bar on mobile, next to "Connect data") exports the entire current dataset from any view.
- Both generate the `.csv` client-side (no server round-trip) and use the artifact viewer's `downloads` capability when running as a Claude artifact, falling back to a plain browser download otherwise.

## Inquiry data model
Each inquiry currently carries: `id`, `subject`, `customer`, `channel` (Email/Chat/Phone/Social/Other), `priority` (Urgent/High/Medium/Low), `status` (Open/Pending/Resolved/Closed), `assignee`, `createdAt`, `firstResponseMins`, `resolutionHours`, `overdue` (SLA breach flag). Response and resolution times are `null` when the source has no such data.

## Design
- Responsive: sidebar + top bar navigation on desktop, bottom tab bar on mobile (breakpoint 700px).
- Light and dark themes (manual toggle + OS preference), built on the validated dataviz color palette (colorblind-safe categorical/status colors).
- No external UI framework — plain HTML/CSS/JS.
- Motion is functional only (loading and refresh feedback) and respects reduced-motion settings.

## Connector status
| Source | Status |
|---|---|
| CSV upload | Real (client-side parse) |
| Airtable | Real (browser-direct, token in memory) |
| Google Sheets | UI only, loads demo data |
| Helpdesk API (Zendesk/Intercom/HubSpot) | UI only, loads demo data |
| REST / JSON | UI only, loads demo data |

## Open decisions
- Final tech stack for the real-code stage.
- Real backend/auth design for the Google Sheets, helpdesk API, and REST connectors.
- Airtable is browser-direct today, so anyone using the page must paste their own token. A small proxy/backend would let a shared deployment hold the token server-side; not built.
- Airtable connector has only been tested against a mocked API; needs a run against a live base.
- Any design changes from reviewing the prototype.
