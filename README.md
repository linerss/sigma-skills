# Sigma Skills

Agent skills for building client-ready [Sigma Computing](https://www.sigmacomputing.com) workbooks and data apps — the process we use on client work at [Greyfield Data](https://greyfielddata.com).

They work with Claude, Cursor, or any agent that reads `SKILL.md` files, on any Sigma org.

| Skill | What it does |
|---|---|
| [`sigma-html-benchmark`](skills/sigma-html-benchmark/SKILL.md) | Grills you on the reader, metrics, and pages, then designs the workbook as a Sigma-buildable HTML mockup with real numbers — **before** anything is built in Sigma. |
| [`sigma-build`](skills/sigma-build/SKILL.md) | Builds the workbook to match the agreed benchmark. Uses a Sigma MCP connector when one is connected, otherwise writes click-by-click steps for the Sigma UI. |
| [`sigma-workbook-qa`](skills/sigma-workbook-qa/SKILL.md) | Renders every page, names defects, ties every headline KPI to the warehouse, exercises every control, and signs off on the published version. |

## Why HTML first

Iterating on layout and copy in HTML takes minutes. Iterating inside a Sigma workbook takes hours. The benchmark is designed only with native Sigma elements on Sigma's 24-column grid, so the build can match it — and QA has a clear bar to measure against.

## House rules built in

- Title Case for pages, titles, tabs, and menus — acronyms and client terms kept as-is.
- Never change the client's math or terminology; flag concerns instead.
- Every headline KPI tied out against an independent warehouse query.
- Client screenshots come from the published view, never edit mode.
- The Sigma build must match the HTML benchmark pixel perfect, measured at the same screen width.
- The agent interviews you one question at a time instead of guessing.

## Install

```bash
npx skills add linerss/sigma-skills
```

Or copy a skill folder into your agent's skills directory (for Claude Code: `~/.claude/skills/<skill-name>/`).

## Use

- "Design a sales performance dashboard in Sigma for our regional managers."
- "Build the workbook from this benchmark."
- "QA this Sigma workbook before it goes to the client."

## About

Greyfield Data builds Sigma Computing dashboards, data apps, and embedded analytics, and migrates teams from Tableau and Power BI. Need help with a Sigma project? [greyfielddata.com](https://greyfielddata.com)

## License

MIT
