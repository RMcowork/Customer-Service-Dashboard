# Customer Service Dashboard — Spec

## Purpose
A dashboard for tracking customer inquiries, built for both mobile and desktop, with an elegant/neutral visual style and the ability to load data from multiple sources.

## Build approach
1. **Prototype first** (current stage) — an interactive, self-contained HTML file (`prototype.html`) with generated sample data, used to validate layout, color, and UX before writing real application code.
2. **Real code** (next stage, not started) — once the design is approved, rebuild as a proper app (tech stack TBD — React/Vite is the default recommendation) with real backend connectors replacing the demo-data stand-ins.

## Data sources
The dashboard should be able to load inquiry data from:
- **CSV / Excel upload** — user drags in an exported file; parsed client-side. *(Implemented for real in the prototype.)*
- **Google Sheets** — pull rows live from a shared sheet.
- **Helpdesk API** — Zendesk, Intercom, or HubSpot Service Hub.
- **Generic REST/JSON endpoint** — any URL returning a JSON array (or `{data: [...]}` / `{results: [...]}`) of inquiries.

> In the prototype, the last three connectors are UI-complete (platform/URL/token fields, a "Connect" action) but load bundled demo data instead of calling a real endpoint, since there's no backend yet.

## Core views
- **Overview** — KPI tiles (total inquiries, open & pending, avg first response time, SLA compliance %), inquiry volume trend (last 30 days), inquiries-by-channel breakdown, team workload summary.
- **Inquiries** — searchable, filterable (status / channel / priority) list of individual inquiries. Table on desktop, cards on mobile.
- **Team** — per-agent workload: open inquiries, average resolution time.

## Inquiry data model
Each inquiry currently carries: `id`, `subject`, `customer`, `channel` (Email/Chat/Phone/Social), `priority` (Urgent/High/Medium/Low), `status` (Open/Pending/Resolved/Closed), `assignee`, `createdAt`, `firstResponseMins`, `resolutionHours`, `overdue` (SLA breach flag).

## Design
- Responsive: sidebar + top bar navigation on desktop, bottom tab bar on mobile (breakpoint 700px).
- Light and dark themes (manual toggle + OS preference), built on the validated dataviz color palette (colorblind-safe categorical/status colors).
- No external UI framework — plain HTML/CSS/JS.

## Open decisions
- Final tech stack for the real-code stage.
- Real backend/auth design for the Google Sheets, helpdesk API, and REST connectors.
- Any design changes from reviewing the prototype.
