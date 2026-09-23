# practice.md

A running log of what's been built in this repo and why — most recent first. For current scope/decisions see [spec.md](spec.md); for repo conventions see [CLAUDE.md](CLAUDE.md).

## 2026-09-23 — Auto-refresh every minute
- Airtable sources now update every 60s in the background. Quiet by design (spinner in the Source bar only, no card pulse or toast), pauses while the tab is hidden, never overlaps loads, stops on 401/403 or 3 consecutive failures while keeping the last good data. On/off toggle with a live countdown, preference remembered. Disconnect stops it.
- Tested with a mocked API: countdown, natural 60s firing across three cycles, quiet mode, off/on, 401 stop and recovery, disconnect.

## 2026-09-23 — Refresh animation
- While pulling from Airtable (connect or Refresh), the Source bar shows a spinning icon, a sliding progress bar and a live "N records so far" count; KPI/chart cards pulse, then fade in when the new data lands. The Connect button shows a spinner too. Loads are held visible for at least 0.7s so fast responses don't just flash. Animations are disabled under `prefers-reduced-motion`. Tested with a mocked, slowed Airtable API.

## 2026-09-23 — Airtable connector
- Added a real Airtable connector (new "Airtable" tab in Connect data): token + base/URL + table + optional view, paginated fetch straight from api.airtable.com, auto field detection with optional manual mapping, friendly errors for 401/403/404/429/network, Refresh and Disconnect.
- Token handling: password field, held in memory only, cleared after connecting, never stored or committed (repo is public). A token pasted into chat was deliberately *not* entered into the page by Claude; the user pastes it themselves.
- Made real-data paths honest: replaced the demo-only CSV shortcuts (random status/channel/assignee, fake response times, fake overdue) with real-field mapping and "—" when data is missing; assignees now come from the data; added an "Other" channel.
- Security hardening found while doing this: inquiry text is customer-authored, so all data-derived text is now HTML-escaped (previously interpolated raw into `innerHTML`).
- Replaced the regex CSV splitter (dropped empty cells and shifted columns) with a proper quoted-field parser.
- Added a "Source" bar showing origin, record count, freshness and any defaulted fields.
- Tested against a mocked Airtable API (pagination, auth header, XSS payload, error codes) at desktop and mobile widths. Not yet run against a live Airtable base.

## 2026-09-17 — Docs pass + GitHub Pages
- Renamed `prototype.html` → `index.html` so GitHub Pages can serve it directly from the repo root.
- Added `CLAUDE.md` (repo conventions) and this file.
- Enabled GitHub Pages on `main` / root.

## 2026-09-17 — Global download button
- Added a "Download data" affordance in the sidebar (desktop) and as a top-bar icon (mobile), next to "Connect data" — exports the *entire* current dataset as CSV from any view, complementing the filtered export on the Inquiries page.

## 2026-09-17 — Artifact downloads capability
- The first CSV export used a plain `<a download>` blob link, which silently does nothing inside the Claude artifact viewer's sandbox (confirmed via a publish-time warning). Fixed by using `window.claude.use('downloads')` when available, with the plain browser download kept as the fallback for the standalone file / GitHub Pages.

## 2026-09-17 — CSV download (Inquiries view)
- Added a "Download CSV" button to the Inquiries view that exports whatever rows currently match the search/filters.

## 2026-09-17 — spec.md
- Wrote the first spec after being asked why it hadn't been written up front — scope, data sources, views, and open decisions, so those choices live in the repo instead of only in chat history.

## 2026-09-17 — Initial prototype
- Built the first interactive prototype (`prototype.html`): Overview / Inquiries / Team views, responsive (sidebar+topbar on desktop, bottom tab bar on mobile), light/dark themes using the `dataviz` skill's validated color palette, and a "Connect data" modal (CSV upload works for real; Google Sheets / Helpdesk API / REST are UI-complete but load demo data).
- Verified in-browser at desktop/mobile widths and both themes; fixed a real bug found during testing (search bar collapsing to ~50px on mobile due to a flex layout conflict).

## 2026-09-17 — Repo created
- Created the private GitHub repo `RMcowork/Customer-Service-Dashboard`.
