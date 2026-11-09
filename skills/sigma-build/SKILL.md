---
name: sigma-build
description: "Build a Sigma Computing workbook that matches an agreed HTML mockup, pixel for pixel. Use after sigma-html-benchmark or when asked to build a Sigma dashboard from a design."
---

# Sigma Build

Build the workbook the benchmark describes: nothing more, nothing less. The HTML benchmark is the quality bar; the build is finished when the published Sigma pages match it and the numbers tie out.

**No benchmark yet?** Stop and run the **sigma-html-benchmark** skill first. Building straight into Sigma without an agreed design is how workbooks end up rebuilt twice.

## House rules

1. **Title Case** for page names, element titles, tabs, menus, and buttons. Keep acronyms and client terms exactly as written.
2. **Never change the math or the terminology.** Implement calculations exactly as specified. If one looks wrong, flag it. Do not "fix" it silently.
3. **Tie out every headline KPI** against an independent warehouse query before calling the build done.
4. **No edit-mode screenshots for clients.** Anything shown to a client comes from the published view, never the edit-mode bar.
5. **Native Sigma only.** No CSS injection or workarounds the client's team cannot maintain. Match the benchmark with grid placement, element sizing, typography, colors, and spacing.

## Search the Sigma docs

Sigma ships changes every week. Before designing or building with any feature you are not certain of, check the official docs at [help.sigmacomputing.com](https://help.sigmacomputing.com) (the [changelog](https://help.sigmacomputing.com/changelog) shows what changed). Most doc pages have a Markdown version (add `.md` to the URL), and Sigma publishes a docs MCP server at `https://help.sigmacomputing.com/_mcp/server` that agents can connect to. Cite the doc page when a decision depends on it, and trust the docs over this skill if they disagree.

## Grill the user on gaps

The benchmark answers most questions; grill the user on the rest before building. Ask **one question at a time** (multiple-choice with 2–4 concrete options when your agent supports it), acknowledge each answer, and keep going until nothing is left to guess. Never ask what you can check yourself in the data or the benchmark.

Typical gaps:

- Which source table, when copies exist in more than one schema or database.
- Ambiguous metric definitions the data can't settle (gross vs net, which date column defines the period).
- Filter scope: page-only or workbook-wide.
- Who can edit input tables, and where written data should land.
- Where the workbook lives (folder, workspace) and who it is shared with.

Summarize the decisions before building.

## Choose the build path

- **A Sigma MCP connector is connected** (it can search sources, describe tables, run queries, and build/edit workbooks): build through it. See *Path A*.
- **No connector**: produce a build guide the user follows in the Sigma UI. See *Path B*.
- **API access, no connector**: Sigma can create a workbook from a JSON/YAML *code representation* via its REST API (public beta at the time of writing: check the docs). Useful for repeatable builds; not every feature is supported, and unsupported features are silently dropped.

Either way, do the data work first.

## 1. Data first

- Locate the exact source tables. Watch for duplicate copies of a table in different schemas or databases and confirm which one the client means.
- Describe the columns and profile the data: row count, date range, grain, distinct counts of key dimensions.
- For every metric in the benchmark, write the calculation and run it as a warehouse query. Record the expected values. These are the tie-out targets for QA.
- Resolve ambiguous measures before building (e.g. is revenue `price` or `price × quantity`? gross or net? which date column defines the period?).
- Decide where shared logic lives: one source element (or data model) per metric family that charts and KPIs reference as children, so numbers cannot drift apart.

## Path A: build through a Sigma MCP connector

1. Start the session the connector requires before any other call.
2. Read the connector's own instructions and SDK/reference resources before writing a build. Follow its conventions over anything here.
3. Build in passes, committing between them: pages and layout skeleton → data sources and calculations → elements and formatting → controls and interactions.
4. Screenshot each page after each pass and compare against the benchmark at the same width. Fix layout drift before moving on.

Lessons from real builds (connector behavior varies; check yours):

- **Containers** may not be screenshot-able as single elements: screenshot the whole page instead.
- **Popovers** may need a container inside a header as their anchor rather than attaching to the header directly.
- **Linked input tables:** build and commit the source table first, add the linked input table in a separate build step, then add seed rows in a further step.
- **Hidden pages** (e.g. a Data page) may not be directly queryable through the connector: verify their values through a visible element or by re-running the logic as warehouse SQL.
- **Helper or command libraries** referenced in docs may not be importable on every host: inline the logic if an import fails.
- Look up elements by ID and check their type after creation rather than assuming the build placed what you expected.

## Path B: click-by-click build guide

Produce a guide the user can follow without guessing:

- One section per page, in build order.
- For each element: element type, source, grid position (column start, width, height), title, every column/formula with exact Sigma formula syntax, number format, sort, and chart settings.
- Controls: type, the column it filters, which elements it targets, default value, and whether it is page- or workbook-scoped.
- Interactions: actions, drill-downs, input table permissions.
- A check after each section: "the page should now look like benchmark page N, with these numbers: …".

## 2. Match the benchmark: pixel perfect

The standard is **pixel perfect**, not "close." "Better" is not done.

How to check it:

1. Render the HTML benchmark and screenshot the published Sigma page at the **same viewport width** (e.g. 1512px).
2. Measure, don't eyeball: left and right edges of every card, column widths, gutters, card heights, input field widths, title baselines, and font sizes. Compare the numbers between the two renders.
3. List every difference as *page → element → benchmark value vs Sigma value* (e.g. "Costs card: right input column 412px vs 286px in benchmark").
4. Fix the largest differences first, re-screenshot, re-measure. Repeat until every element lines up with the benchmark, or the remaining difference is a documented Sigma limitation.

Then match the content:

- Same pages, same element order, same grid proportions (keep the benchmark's 24-column splits; sections designed as stacks are built as stack containers).
- Same titles, labels, number formats, and period labels.
- KPIs show their comparison and period. Charts use one accent color for the primary series.
- Cards sized to content: no fixed heights leaving empty space.
- Form inputs read top-to-bottom within each column, columns equal width.

Where Sigma cannot match the benchmark exactly, take the closest native option and list the difference in the handoff.

## 3. Hand off to QA

Run the **sigma-workbook-qa** skill before calling the build done. Then report:

- What was built (pages and key elements) and where it lives.
- Tie-out results: expected vs workbook value for each headline KPI.
- Anything built but not live-tested (e.g. buttons that write data, permission-dependent behavior, AI agents).
- Every place the build deviates from the benchmark, and any assumption made.

---

Part of [sigma-skills](https://github.com/linerss/sigma-skills) by [Greyfield Data](https://greyfielddata.com): Sigma Computing consultants.
