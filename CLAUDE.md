# CLAUDE.md

Context for Claude Code (or any assistant) working in this repo.

## What this repo is

A customer service dashboard for tracking customer inquiries (volume, SLA/response performance, team workload). Currently in the **prototype stage**: `index.html` is a single self-contained HTML/CSS/JS file with no build step and no dependencies, using generated sample data. See [spec.md](spec.md) for full scope and [practice.md](practice.md) for the history of what's been built.

## Conventions

- **Single file, no build step.** `index.html` is the entire app: styles in a `<style>` block, logic in one `<script>` block at the bottom, no imports, no bundler. Keep it that way until the "real code" stage in spec.md actually begins — don't introduce a framework or build tooling speculatively.
- **`index.html` is also the GitHub Pages entry point.** It's served directly from the `main` branch root. Don't rename it without updating the Pages source and every doc that links to it.
- **Colors come from the `dataviz` skill's validated palette** (light + dark, colorblind-safe categorical/status colors) — defined as CSS custom properties at the top of `index.html`. Don't hand-pick new colors; extend from that palette if a new one is genuinely needed.
- **Responsive breakpoint is 700px** — sidebar + top bar above it, bottom tab bar below it. Test both when changing layout.
- **Data connectors are mostly UI-only.** Only CSV/Excel upload actually parses a real file (client-side). Google Sheets / Helpdesk API / REST are UI-complete but simulate a connection and load bundled demo data — there is no backend yet. Don't silently make one of these "real" without flagging it, since it changes the trust/security surface (API tokens, endpoints).
- **Downloads use the artifact `downloads` capability when available**, falling back to a plain browser download otherwise (see the `downloadCSV` function in `index.html`). This matters because the file is viewed both as a plain HTML file/GitHub Pages site and as a Claude artifact, and plain `<a download>` links don't work inside the artifact sandbox.

## Workflow

- This project uses **prototype-first**: validate design/UX as a static HTML prototype before writing the real app. Don't jump ahead to a framework rewrite unless the user says the prototype is approved.
- When scope or decisions change, update [spec.md](spec.md) — it's the source of truth for what this dashboard is supposed to do, not just this file.
- Log meaningful changes in [practice.md](practice.md) as they're made, not retroactively in bulk.
- Test changes in an actual browser (desktop + mobile width, light + dark mode) before considering a UI change done — this file has caught real layout bugs (e.g. a topbar flex conflict) that were invisible from reading the code alone.
