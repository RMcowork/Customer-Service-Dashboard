# practice.md

A running log of what's been built in this repo and why — most recent first. For current scope/decisions see [spec.md](spec.md); for repo conventions see [CLAUDE.md](CLAUDE.md).

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
