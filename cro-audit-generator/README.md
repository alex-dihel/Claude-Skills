# CRO Audit Generator - A Claude Skill

A Claude Skill that runs a conversion rate optimization (CRO) audit on a single page or a multi-step flow, scores six persuasion elements against the site's actual business model, and produces a prioritized, PIE-scored HTML report, all built for Claude Code.

---

## The Problem It Solves

A CRO review usually comes down to one person's gut reaction to a page. This skill runs a consistent framework instead: credibility, lure, objection handling, social proof, ease of use, and result, checked against cited benchmarks rather than memorized figures, with every finding traced to something actually verifiable on the page, not inferred from wording or proximity.

It's a surface-level review, not an analytics platform. It reads what's live on the page (and, optionally, real technical signals via a browser tool), it doesn't have access to your actual visitor data unless you hand it to it.

---

## What It Does

On invocation, the skill:

- Checks whether the `chrome-devtools-mcp` plugin is installed, this determines whether the technical pass is qualitative-only or backed by real, measured Core Web Vitals and live-rendered checks
- Runs a setup interview: single page or multi-step flow, business model (e-commerce, lead-gen, SaaS trial, subscription, marketplace, or high-ticket B2B), page-only or data-informed (you supply funnel/heatmap/analytics exports), and two independent opt-ins, a competitor pass and an industry-standards research pass
- Fetches and verifies every target live, never from memory or assumption
- Assesses each of the six framework elements against its actual definition, not just a fixed checklist, and traces every claimed CTA or offer to its real destination before scoring it
- Produces a single HTML report: a summary, an element-by-element breakdown with a rating and a link to whichever recommendation fixes it, a just-do-it/high-impact/scored-backlog recommendation list, and a sources-cited table
- Screenshots every defect it raises when `chrome-devtools-mcp` is available

It does not pull data from Google Analytics, Microsoft Clarity, or any other analytics platform. If you want the audit grounded in real visitor behavior instead of page-only inference, you supply that data yourself when asked in the setup interview.

---

## Requirements

- Claude Code. This skill was built and tested in Claude Code, it hasn't been tested in Claude.ai
- Recommended: the `chrome-devtools-mcp` plugin, for real Core Web Vitals, live-rendered checks, and screenshots. Without it, the technical pass falls back to qualitative-only signals and the audit still runs, just with less verified technical detail

---

## Installation

**1. Install the skill.** Download [`cro-audit-generator.skill`](./cro-audit-generator.skill), then in Claude (web or desktop app), go to **Customize > Skills > Add > Upload a skill** and upload the file.

Alternatively, for Claude Code without the app: unzip `cro-audit-generator.skill` and place the `cro-audit-generator/` folder (containing `SKILL.md` and `references/`) into your Claude Code skills directory:

```
~/.claude/skills/cro-audit-generator/        # personal, available in every project
.claude/skills/cro-audit-generator/          # project-scoped, travels with the repo
```

Make sure `SKILL.md` sits directly inside that folder, not nested another level deeper, that's the most common install mistake. Start a new Claude Code session, then run `/skills` (or `claude skills list`) to confirm it loaded.

**2. Install `chrome-devtools-mcp` (recommended, not required):**

```
claude plugin install chrome-devtools-mcp@claude-plugins-official
```

This can be done before your first audit or at any point later, the skill checks for it fresh at the start of every run.

---

## Usage

The skill triggers two ways:

**Automatically**, when your request matches its description, for example:

> Run a CRO audit on our checkout flow.

> Why isn't this landing page converting?

**Directly**, by naming it, useful if the automatic match doesn't fire or you want to be explicit:

> Use the cro-audit-generator skill on [website/page url]'s homepage.

Either way, the skill runs its own setup interview in small batches, waits for your answers, verifies the live target before auditing it, and saves the final report as an HTML file with the audit date in the filename, so multiple runs sort chronologically.

---

## Configuration Options

| Setting | Options |
|---|---|
| Audit type | Single page, or a multi-step flow |
| Business model | E-commerce, lead-gen/service, SaaS trial, subscription, marketplace, or high-ticket B2B |
| Data tier | Page-only, or data-informed (you supply funnel/heatmap/form-analytics exports) |
| Competitor pass | Skip, supply a URL directly, or have the skill search for and confirm comparable competitors |
| Industry-standards research | Skip, or freshness-check the cited benchmark against a live source plus a best-practices summary for the vertical |
| Technical depth | Qualitative-only by default, real measured Core Web Vitals and live-rendered checks if `chrome-devtools-mcp` is installed |

---

## A Note on AI Output

This skill can get things wrong. Check its work same as any of these: open the saved HTML report and read the element-by-element findings against the actual page yourself before acting on the recommendations.

Specific limitations worth knowing:

- **This is a surface-level UX and technical review, not an analytics platform.** It assesses what's visible on the page, plus qualitative or (with `chrome-devtools-mcp`) real technical signals. It has no connector to Google Analytics, Microsoft Clarity, or any other analytics tool. Findings only reflect real visitor behavior if you supply that data yourself in the data-informed tier, page-only findings are inference from the page's content and structure, not confirmed behavior.
- **The technical section is qualitative only without `chrome-devtools-mcp`.** No Lighthouse, no Core Web Vitals field data, no live-rendered check, the report says so plainly when this is the case.
- **Competitor and industry-standards passes are opt-in and add real cost.** Neither runs unless you say so in the setup interview.

---

## License

CC0 - public domain. Covered by this repository's root `LICENSE` file, which applies to everything inside it. Use, adapt, and redistribute freely, no attribution required.
