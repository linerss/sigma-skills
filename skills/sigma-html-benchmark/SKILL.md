---
name: sigma-html-benchmark
description: Design a Sigma Computing workbook or data app as an HTML mockup BEFORE building anything in Sigma. Use whenever someone asks to build, redesign, or plan a Sigma dashboard, data app, or report — grill the user one question at a time, then produce a Sigma-buildable HTML benchmark on Sigma's grid with real sample numbers, get it agreed, then hand it to the sigma-build skill.
---

# Sigma HTML Benchmark

**Always build the HTML first.** Iterating on layout, hierarchy, and copy in HTML takes minutes; iterating inside a Sigma workbook takes hours. The HTML benchmark becomes the visual quality bar the Sigma build is measured against — screen by screen, element by element.

The benchmark must be **Sigma-buildable**. A beautiful mockup Sigma cannot reproduce is worse than useless: it sets an expectation the build will miss. Only design what Sigma can do natively.

## House rules

These apply to every benchmark:

1. **Title Case** for page names, element titles, tabs, menus, and buttons. Keep acronyms and industry terms exactly as the client writes them (e.g. TCE, LDT, YoY, ARR).
2. **Never change the math or the terminology.** If the client has a calculation or a label, the benchmark shows it as-is. Flag concerns; do not silently "fix" them.
3. **Real numbers.** Use values pulled from the actual data (or a faithful sample of it), never lorem ipsum or invented round numbers. Fake numbers hide formatting and width problems.
4. **No CSS tricks Sigma cannot copy.** No custom fonts Sigma's theme can't use, no gradients, shadows, overlapping elements, or hover effects that Sigma elements don't have.

## Search the Sigma docs

Sigma ships changes every week. Before designing or building with any feature you are not certain of, check the official docs at [help.sigmacomputing.com](https://help.sigmacomputing.com) (the [changelog](https://help.sigmacomputing.com/changelog) shows what changed). Most doc pages have a Markdown version (add `.md` to the URL), and Sigma publishes a docs MCP server at `https://help.sigmacomputing.com/_mcp/server` that agents can connect to. Cite the doc page when a decision depends on it, and trust the docs over this skill if they disagree.

## Workflow

### 1. Grill the user

Before designing anything, interview the user until there is a shared understanding of what to build. Most bad dashboards come from a design that guessed.

- Ask **one question at a time**. Use a multiple-choice question tool if your agent has one, with 2–4 concrete options and room for a custom answer; otherwise ask in plain text.
- Acknowledge each answer in a sentence, then ask the next question. Resolve decisions in dependency order — reader and job first, then pages, then elements, then details.
- Never ask what you can find out yourself. Check the data, existing workbooks, and prior notes first; ask only what only the user can answer.
- Keep going until every branch is resolved, then summarize the decisions and confirm before designing.

Questions to cover:

- **Reader and decision:** who opens this, and what do they decide with it?
- **Pages:** one per distinct question or job (overview, explore, review/act). Which ones are needed?
- **Metrics:** for each headline number, the exact definition, the period, and the comparison (prior period, plan, target).
- **Filters:** what readers need to slice by, and whether filters apply to one page or the whole workbook.
- **Actions:** does anyone enter data, approve, or log decisions (input tables, buttons)?
- **Audience and access:** internal, client, or embedded? Any row-level security?
- **Style:** existing brand or workbook to match? Client terminology that must be kept as-is?

### 2. Understand the data

- Profile the source: row count, date range, grain, the measures and dimensions that matter, and how each headline metric is calculated. Query the warehouse (or ask for a sample extract) to get real values for every number that will appear.

### 3. Map to Sigma's building blocks

Design only with elements Sigma has — data elements (tables, pivots, charts), UI elements (text, buttons, images, dividers, embeds), control elements (filters and parameters), plus input tables and layout containers:

| Need | Sigma element |
|---|---|
| Headline number with comparison | KPI element |
| Trend over time | Line / area / bar chart |
| Ranking | Horizontal bar chart |
| Share of whole (≤5 parts) | Donut / pie |
| Rows to scan or look up | Table |
| Cross-tab | Pivot table |
| Where | Map (point / region) |
| Filter or parameter | Control (list, date range, text search, slider, switch) |
| User-entered data, plans, approvals | Input table |
| Grouped layout, cards, tabs | Container (grid or stack), tabbed container |
| Explanations, headers | Text element |
| Actions | Button (with actions), drawer/modal |

If a requested feature has no Sigma equivalent, say so in the benchmark notes and propose the closest native alternative.

### 4. Lay it out on Sigma's grid

- Lay pages out on a **24-column grid** — the grid Sigma's own build tooling uses. Size every element in whole columns (e.g. 16/8, 14/10, 12/12, 8/8/8) and keep shared top edges and heights inside a horizontal band. Check the workbook's canvas width setting (Full or a custom maximum) and design at that width.
- Where a section should reflow with content or screen size, design it as a **stack** (a container laid out like CSS flexbox: row or column, gap, fill / fit-content / fixed sizing) rather than fixed grid positions. Note in the build sheet which sections are stacks.
- Build the HTML with a CSS grid of 24 equal columns at a typical client screen width (1440–1512px) so proportions translate 1:1.
- Stack the page as bands, top to bottom: title → KPIs → main chart(s) → detail table. Controls go in a top toolbar band or a left rail column.
- Size cards to their content. Fixed heights that leave empty space inside a card are a defect.
- Inputs in forms read **top-to-bottom within a column**, with equal column widths.

### 5. Style within Sigma's theme

- One page background, one card background, one accent color for the primary series, neutral grays for context series.
- A restrained type scale: page title, element title, body, small label. No more.
- Numbers right-aligned with explicit formats (currency, %, thousands separators, decimals) and units in titles or labels. Dates grouped to the period the reader thinks in (month, quarter).
- KPIs always show a comparison (vs prior period, vs plan) and the period they cover.

### 6. Produce the benchmark

- One self-contained HTML file, one section (or tab) per Sigma page, at the target screen width.
- Under each page, a short **build sheet**: each element's Sigma type, grid position (column start, width, height in rows), data source, and formula or aggregation for every number shown.
- A **notes** section listing assumptions, open questions, and anything that cannot be built natively.

### 7. Review and agree

- Render the HTML and look at it at the target width. Fix clipping, crowding, misalignment, and empty space before showing anyone.
- Get the user's (or client's) agreement on the benchmark. Changes happen here, in HTML, not in Sigma.
- Hand the agreed benchmark and build sheet to the **sigma-build** skill.

## Done when

- Every page the reader needs is designed, with real numbers.
- Every element maps to a native Sigma element and a grid position.
- The rendered HTML has been inspected at the target width with no visible defects.
- The user has agreed to it.

---

Part of [sigma-skills](https://github.com/linerss/sigma-skills) by [Greyfield Data](https://greyfielddata.com) — Sigma Computing consultants.
