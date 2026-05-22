# Enrichment Log — claude-code-for-students

Run date: 2026-05-22

## Per-chapter results

| Chapter | Tables rendered | SVGs generated | D3 HTML generated | PNG generated | Wayback Machine |
|---|---|---|---|---|---|
| 00-frontmatter.md | 0 | 0 | 0 | 0 | n/a |
| 00-introduction-cautious-builder.md | 0 | 1 | 1 | 1 | kept (skipped per instruction; SIDEBAR converted to inline blockquote) |
| 01-homework-quiz-gap.md | 1 | 1 | 1 | 1 | kept (skipped per instruction) |
| 02-division-of-labor.md | 1 | 1 | 1 | 1 | kept (skipped per instruction) |
| 03-teacher-student-ai-gap.md | 0 | 1 | 1 | 1 | kept (skipped per instruction) |
| 04-conducting-not-prompting.md | 0 | 1 | 1 | 1 | kept (skipped per instruction) |
| 05-five-supervisory-capacities.md | 1 | 1 | 1 | 1 | kept (skipped per instruction) |
| 06-gru-tool.md | 0 | 1 | 1 | 1 | kept (skipped per instruction) |
| 07-software-design-document.md | 1 | 1 | 1 | 1 | kept (skipped per instruction) |
| 08-prompts-as-specifications.md | 1 | 0 | 0 | 0 | kept (skipped per instruction) |
| 09-handoff-conditions-dangerous-middle.md | 1 | 0 | 0 | 0 | kept (skipped per instruction) |
| 10-brutalist-creative-builds.md | 1 | 1 | 1 | 1 | kept (skipped per instruction) |
| 11-planning-first-build.md | 1 | 1 | 1 | 1 | kept (skipped per instruction) |
| 12-running-the-build.md | 0 | 1 | 1 | 1 | kept (skipped per instruction) |
| 13-verification.md | 0 | 1 | 1 | 1 | kept (skipped per instruction) |
| 14-first-full-build.md | 0 | 1 | 1 | 1 | kept (skipped per instruction) |
| 97-fundamental-themes.md | 0 | 0 | 0 | 0 | n/a |
| 98-appendix-walker-and-zelda.md | 0 | 0 | 0 | 0 | n/a |
| 99-back-matter.md | 0 | 0 | 0 | 0 | n/a |

## Summary

- **Total chapters processed:** 16 (15 with content; 4 had only frontmatter/appendix/back-matter and were not enriched)
- **Total tables rendered:** 8
- **Total SVG + PNG pairs generated:** 13
- **Total D3 HTML files generated:** 13
- **Total Wayback Machine subjects replaced:** 0 (per directive: existing Wayback sections were skipped; no new ones added)
- **Total Prompts sections appended:** 13 (one per chapter with a figure)

## Pass execution

- **Pass 1 — Tables:** 8 TABLE comments replaced with rendered markdown tables. Every cell carries real content inferred from chapter context. No `[insert]` placeholders. No headings added above tables.
- **Pass 2 — Figures:** 13 DIAGRAM comments resolved across three sub-batches:
  - 2A — Ch 00, 01, 02, 03, 04 (5 figures + 1 SIDEBAR citation converted inline)
  - 2B — Ch 05, 06, 07, 10 (4 figures)
  - 2C — Ch 11, 12, 13, 14 (4 figures)
  - Each figure: static SVG in `images/`, D3 v7 HTML in `d3/`, markdown link in chapter, structurally-written prompt appended to chapter's `## Prompts` section.
- **Pass 3 — Wayback Machine:** Skipped per directive ("Only add a Wayback machine if it does not exist, otherwise skip it"). All 15 existing Wayback Machine sections left untouched.
- **Pass 4 — PNG conversion:** `node SCRIPTS/svg-to-png.mjs` converted all 13 SVGs to 300 dpi PNG. Converted: 13, Skipped: 0.

## Style conformance

All SVGs:
- `viewBox` only, no width/height
- `#FFFFFF` canvas, `#F5EFE8` plot/callout fills
- 9-token color palette (white / fill / ink / mark / red / slate / ochre / secondary / border) — no hex values outside the palette
- `'EB Garamond', Georgia, 'Times New Roman', serif` for titles and body
- `'IBM Plex Mono', 'Courier New', monospace` for ALL CAPS identifiers only, `letter-spacing="0.08em"`
- One red highlight per figure (or none)
- Single shared `<defs>` arrowhead marker
- `role="img"`, `<title>`, `<desc>` on every SVG
- No shadows, no rounded corners (`rx="0"`), no gradients

All D3 HTML files:
- D3 v7.9.0 CDN exactly: `https://cdnjs.cloudflare.com/ajax/libs/d3/7.9.0/d3.min.js`
- `var(--color-*)` CSS custom properties — no hardcoded hex in the JS
- Light-mode and dark-mode (`prefers-color-scheme: dark`) variable sets
- Event handlers in `(event, d)` order (v7 contract)
- `.join()` not `.enter().append()`
- ResizeObserver redraw pattern
- `prefers-reduced-motion: reduce` suppresses transitions
- `role="img"`, `aria-labelledby`, `<title>`, `<desc>` on every chart SVG
- Slate-background tooltip, white text

## Deviations from spec

- Ch 13 SVG uses `viewBox` height 540 to accommodate the three-pass column plus two side resolution columns (within spec's "60px increments" allowance).
- A few other SVGs use 480 height for the same reason. All multiples of the 60px increment.
- No other deviations from the SVG Style Guide or the D3 v7 contract.
