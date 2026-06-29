# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A deliverable set of **self-contained HTML report dashboards** — the "Swan · Reddit Intelligence Suite". Each `.html` file is a standalone, offline-capable dashboard presenting marketing/competitive-intelligence findings mined from Reddit (667 threads, 7,384 comments, 49 GTM tools) for **Swan**, a go-to-market / sales-data product competing against Apollo, Clay, ZoomInfo, n8n, and others.

There is **no build system, package manager, test suite, or git repo**. Files are opened directly in a browser (e.g. `start overview.html` on Windows). To "run" or preview, just open the file; for live reload while editing, serve the folder with any static server (`python -m http.server`).

## The reports

All files use a consistent `<topic>.html` lowercase snake_case naming convention.

- `overview.html` — hub page summarizing all seven reports (no charts; pure HTML/CSS).
- `competitor_intelligence.html` — share-of-voice across 49 tools, sentiment, co-mentions.
- `role_intelligence.html` — findings by GTM role (RevOps, Sales Ops, GTM Eng, Demand Gen, Sales). Chart-heavy.
- `subreddit_intelligence_map.html` — subreddit prioritization.
- `use_case_intelligence.html` — use-case / demand analysis. Chart-heavy.
- `audience_report.html` — audience psychology / personas.
- `top_posts_report.html` — top Reddit posts.
- `reddit_roadmap.html` — go-to-market roadmap (no charts).

Every file's header contains a shared **inline report menu** (`<nav class="rnav">`, a flex-wrap row of links, no JS) linking to all 8 pages, with the current page marked via `aria-current="page"`. Its CSS is appended to each file's `<style>` block and uses only tokens present in all files (radius is hardcoded; swan tint uses a `var(--swan-tint, var(--swan-t, #FBF1EB))` fallback chain). This is separate from each report's in-page section `<nav id="nav">` / `<nav id="mainnav">`, which links to anchors within that page.

## Architecture / conventions

Each file is a **single, fully inlined document**: inline `<style>`, inline `<script>`, inline data, and (where charts exist) an inlined copy of Chart.js — so the dashboards work with no network access. Editing a report means editing one file; there are no shared assets, imports, or partials.

**Charting** uses Chart.js 4.4.1 via `new Chart(...)` calls. Be aware of two delivery patterns — keep a file's existing pattern when editing:
- Most chart files **inline the full Chart.js 4.4.1 source** (search for `Chart.js v4.4.1`) for offline use.
- `top_posts_report.html` is the exception — it loads Chart.js from a **CDN** (`cdnjs.cloudflare.com`), so it needs network access.
- `overview.html` and `reddit_roadmap.html` have **no charts**.

**Data is hard-coded** inline (chart datasets as JS literals, metric numbers as HTML text). Updating figures means editing both the chart config objects and any matching hand-written HTML labels — there is no single data source feeding both, so keep them in sync.

**Shared design system** — every file re-declares the same token set via CSS custom properties in `:root` (copy-pasted, not shared). Match these when adding UI so reports stay visually consistent:
- Brand color "swan" `--swan: #C75B39` (terracotta) on a warm paper background (`--bg` ~`#F5F1EB`/`#F6F2EC`), with green/blue/amber/red/purple accent pairs (each a base + soft tint).
- Fonts: **Inter** (UI) and **JetBrains Mono** (numbers, via `.mono` with `font-variant-numeric: tabular-nums`), loaded from Google Fonts.
- Common tokens: `--card`, `--line`, `--ink`, `--muted`, `--faint`, `--shadow`, and a radius var (`--rad`/`--radius`/`--r`). Note the radius variable name differs between files.
- Sticky blurred `header > .bar` (max-width ~1160–1220px, centered) is the standard top-bar pattern.
