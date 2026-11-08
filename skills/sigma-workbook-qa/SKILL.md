---
name: sigma-workbook-qa
description: "Grill the user, then review, polish, and sign off Sigma Computing workbooks and data apps before they reach a client or exec. Use when asked to QA, audit, clean up, fix, polish, or \"make client-ready\" a Sigma workbook, dashboard, page, or embedded view: render the real pages, name defects, fix the smallest thing, verify the data, and re-render before calling it done."
---

# Sigma Workbook QA

A Sigma workbook is not done because it saved, published, or the query ran. It is done when every page a reader will open renders cleanly, the numbers tie out to the warehouse, the controls do what their labels say, and the published version has been inspected the way the client will see it.

Use this skill for any Sigma workbook, dashboard, data app, or embed that is about to be shared, published, or handed off, and for "something looks off" reviews of existing workbooks.

## Core rules

1. **Render before you judge.** Element definitions, formulas, and SQL readback are not enough. Look at the actual page (screenshot, browser, or the Sigma connector's screenshot tool if available). The pixels decide whether it is client-ready.
2. **Name defects before you fix them.** Write a short defect list first. Fix against the list, not by feel.
3. **Make the smallest change that restores coherence.** Preserve the workbook's existing style, page structure, and naming unless the user asks for a redesign.
4. **Verify against the source, not against yourself.** A KPI is right when it ties to an independent query on the warehouse table, not when it matches another element in the same workbook.
5. **Re-render after every fix.** A defect is fixed only when a fresh render shows it gone.

## House rules

1. **Title Case** for page names, element titles, tabs, menus, and buttons. Keep acronyms and client terms exactly as written.
2. **Never change the math or the terminology** while fixing. If a calculation looks wrong, flag it and ask. Do not "fix" it silently.
3. **Tie out every headline KPI** against an independent warehouse query.
4. **No edit-mode screenshots for clients.** Review and capture the published view.

## Search the Sigma docs

Sigma ships changes every week. Before designing or building with any feature you are not certain of, check the official docs at [help.sigmacomputing.com](https://help.sigmacomputing.com) (the [changelog](https://help.sigmacomputing.com/changelog) shows what changed). Most doc pages have a Markdown version (add `.md` to the URL), and Sigma publishes a docs MCP server at `https://help.sigmacomputing.com/_mcp/server` that agents can connect to. Cite the doc page when a decision depends on it, and trust the docs over this skill if they disagree.

## Workflow

### 0. Grill the user

Before reviewing, interview the user so the review checks what matters. Ask **one question at a time** (multiple-choice with 2–4 concrete options when your agent supports it), acknowledge each answer, and stop once nothing is left to guess. Never ask what you can see in the workbook yourself.

- Is there an HTML benchmark or design to compare against?
- Who will read it: internal, client, embedded users?
- Which numbers matter most, and what should they tie out to?
- Review only, or fix what you find?
- Anything that must not change (calculations, labels, layout)?

### 1. Inventory

- List pages, and for each page its elements (tables, pivots, charts, KPIs, controls, text, input tables, containers).
- Note the data sources: warehouse tables, data models, custom SQL, CSV uploads, and which elements are children of which.
- Note who reads it: internal analysts, client execs, embedded/guest users. Embedded and guest viewers see less, so check what they can and cannot open.
- Check whether you are looking at the draft or the published version. Review the published version before sign-off.

### 2. Visual pass: every page, top to bottom

Capture the first viewport, then scroll. Lower sections are where defects hide. If there is an HTML benchmark, the bar is **pixel perfect**: render it and the published Sigma page at the same viewport width, measure card edges, column widths, gutters, heights, and font sizes in both, and log every difference as *element → benchmark vs Sigma*. A page passes only when the measurements match or the gap is a documented Sigma limitation.

Common Sigma defects to look for:

- Titles, page names, tabs, and buttons not in Title Case.
- Element titles that are default names (`Table 1`, `Viz 3`, raw column names) or stale after a rename.
- KPIs with no comparison, period label, or unit: a bare number nobody can judge.
- Raw column names in headers and axes (`TOTAL_REV_USD`, `created_at_utc`) instead of business labels.
- Inconsistent number formats across elements showing the same metric (currency vs plain, 0 vs 2 decimals, % vs ratio).
- Dates as raw timestamps where the reader needs Month / Quarter / Week.
- Charts with crowded or truncated axis labels, too many series, default rainbow colors fighting the page palette, or one series painted multiple colors.
- Pie or donut charts with more than ~5 slices.
- Elements that do not align to a shared grid; uneven gutters; one tall element leaving dead whitespace beside it.
- Visible helper columns, scratch tables, or QA elements on reader-facing pages.
- Controls with unclear labels, no default, or defaults that hide most of the data.
- Empty states: elements that show "No data" under a common filter combination.
- Text elements with stale dates, old client names, or TODOs.

Keep the defect list short and concrete: *page → element → what is wrong*.

### 3. Data pass

- **Tie-out:** for every headline KPI, run an independent query against the warehouse (or the data model) for the same period and filters. Record both numbers. Differences need an explanation, not a rounding excuse.
- **Grain:** confirm joins do not fan out. Totals that are suspiciously round multiples of the true value usually mean a duplicated join.
- **Filters and controls:** change each control through its main values and confirm every element it targets responds, and elements it should not touch do not. Check controls scoped to one page vs the whole workbook.
- **Date logic:** confirm "current period," "last period," and relative date controls land on the dates the label claims, and how time zones are handled.
- **Nulls and blanks:** check how nulls render in KPIs, groupings, and charts. "(null)" as a category label is a defect.
- **Lineage:** child elements should derive from a sensible parent. Duplicate source queries for the same metric drift apart over time, so consolidate where it is safe.

### 4. Interaction pass

- Drill-downs, links, and actions go where their labels say.
- Input tables accept the entries they are meant to and reject or flag bad ones; written values show up where the reader expects them.
- Tabs and containers show the right content in each state.
- If embedded, test as the embed/guest user, not as the builder. Row-level security and user attributes must filter what they claim to.

### 5. Stress test (for workbooks fed by changing data)

- Widen filters to "all" and narrow them to a single small value.
- Check long category names, new categories, and a period with no data.
- Confirm charts, pivots, and KPIs still read well at both extremes.
- Undo any temporary test data or settings afterward and verify they are gone.

### 6. Fix and re-verify

- Fix the named defects in order of reader impact: wrong numbers first, then broken controls, then visual polish.
- Re-render each changed page. Mark a defect fixed only after the fresh render proves it.
- Re-run the tie-out queries for any metric whose element, formula, or source changed.

## Style defaults (when the workbook has no established style)

- One clear title per page, with a subtitle that names the period and scope.
- KPIs at the top, each with a comparison (vs prior period, vs plan) and a unit.
- Trend charts as lines; group comparisons as columns; long category labels as horizontal bars.
- One accent color for the primary series; neutral grays for context series.
- Business labels everywhere a reader looks. Raw field names stay in the data model.
- Text and categorical values left-aligned; numbers right-aligned with explicit formats.
- Put methodology notes, data caveats, and refresh details on a dedicated Notes or About page, not on the main dashboard.

## Client-ready checklist

Do not call the workbook ready until all of these pass on the **published** version:

- [ ] If there is a benchmark, every page matches it pixel perfect at the same viewport width (or the gap is a documented Sigma limitation).
- [ ] Every reader-facing page renders with no errors, spinners, or empty elements under default controls.
- [ ] Every headline KPI ties out to an independent warehouse query.
- [ ] Every control changes the elements it should and only those.
- [ ] No default element names, raw column names, or stale text on reader-facing pages.
- [ ] Number and date formats are consistent for the same metric across the workbook.
- [ ] Helper, scratch, and QA elements are off reader-facing pages.
- [ ] Embedded/guest view checked as that user, including row-level security.
- [ ] Temporary test data and settings are removed.
- [ ] Client-facing screenshots come from the published view, with no edit-mode bar.

## Do not say ready if

- You have not looked at a fresh render of every reader-facing page.
- A headline number has not been tied out to the source.
- Any control was not exercised.
- You only reviewed the draft and not the published version.
- The embed was only checked as the builder.

## Reporting back

Report what matters: what was fixed, what was verified (including tie-out numbers for headline KPIs), and any caveat that affects how the workbook should be read. Skip tool chatter and scratch work.

---

Part of [sigma-skills](https://github.com/linerss/sigma-skills) by [Greyfield Data](https://greyfielddata.com): Sigma Computing consultants.
