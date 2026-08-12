---
name: cro-audit-generator
description: Generates a conversion rate optimization (CRO) audit for a single page (e.g. a landing page or homepage) or a multi-step flow (e.g. add-to-cart → checkout → purchase, or a full visitor-to-buyer path). Scores six persuasion elements (credibility, lure, objection handling, social proof, ease of use, result) against the site's actual business model (e-commerce, lead-gen/service, SaaS trial, subscription, marketplace, or high-ticket B2B), runs a qualitative technical/accessibility pass, and produces PIE-scored, prioritized recommendations grounded in cited benchmarks rather than memorized figures. Use this skill whenever the user wants a CRO audit, a conversion audit, conversion recommendations, or asks why a page, landing page, or checkout flow isn't converting, especially when they mention "CRO audit," "conversion audit," "audit this page/flow," "review this for conversion issues," or "improve conversion rate."
---

# CRO Audit Generator

## Why this skill exists

Conversion problems are rarely one thing. A page can look great and still lose visitors to a slow load, a missing reason to trust the business, an unclear next step, or a form that asks for more than it needs. This skill runs a consistent, multi-layer audit instead of a single gut reaction: technical signals first, then the six-element persuasion framework, then a prioritized fix list. It reads from two bundled reference files rather than relying on memorized conversion statistics, because those numbers date quickly and vary by industry, channel, and business model.

## Pre-flight check

Before saying anything to the user, list installed plugins and check for `chrome-devtools-mcp`. This determines whether the technical/performance pass can go beyond qualitative signals later, and whether the disclosure in the next section is accurate. Don't skip this or do it later, the disclosure below depends on it.

## Mandatory first response

Before starting the interview, state plainly:
- This is a fresh audit every time. There's no update mode, nothing gets diffed against a prior run.
- Technical/performance disclosure, conditional on the pre-flight check above:
  - `chrome-devtools-mcp` not found: the technical/performance check is qualitative only. This skill can't run Lighthouse or pull real Core Web Vitals field data, so it flags visible signals (oversized images, obvious layout-shift risk, mobile viewport/tap-target issues) rather than measured scores. Say this outright so the audit's technical section isn't mistaken for a real performance test.
  - `chrome-devtools-mcp` found: say that a live browser check is available this run, so tap-target sizing, rendered contrast, and mobile-viewport rendering will be actually verified rather than flagged as unmeasured, and real performance tracing (including Core Web Vitals) is possible instead of the usual qualitative-only signals.

## Step 1: Setup interview

Ask in small grouped batches, not all at once. Wait for each answer before moving to the next group. Use the platform's native structured selection tool when available, fall back to plain numbered text only if none exists. Only mark a choice "(default)" when it's genuinely inferable from context already established about this specific target (e.g. the business model, if what's being audited was already described as a store, a SaaS product, etc.), never as a blanket practice applied to every question, and never "(recommended)." A question with no real inferable default (see Group D) gets no marking at all, that's the user's call to make, not a lean.

**Group A (audit type):**
- Single page (e.g. a landing page or homepage), or a flow (multiple steps in sequence, e.g. add-to-cart → checkout → purchase)
- Flow audits need cross-step continuity checks later (does trust/social proof persist across steps, does the CTA stay consistent) that a single-page audit doesn't

**Group B (business model):**
- E-commerce, lead-gen/service, SaaS trial, subscription, marketplace, or high-ticket B2B
- This isn't a formality. The "lure" and "result" elements mean structurally different things per model (see `references/cro-audit-supplementary-knowledge.md`, section 5), so the model choice changes what the audit actually checks for
- If the business doesn't cleanly fit one of the six, don't force a pick. Ask clarifying questions about how the business actually makes money and what a converted visitor looks like, until a model (or a reasonable hybrid framing) becomes clear

**Group C (data tier):**
- Page-only (the audit works from what's live on the page/flow), or data-informed (the user supplies funnel, heatmap, form-analytics, or traffic-segmentation exports)
- If data-informed, ask what data is actually available before proceeding, don't assume a full data set exists just because the tier was chosen

**Group D (competitor and industry-standards check, optional):**
- Competitor pass: ask whether to skip it, supply a competitor URL directly, or have the skill search for comparable competitors (same category and business model, weighted toward businesses running paid search ads, per the framework's own guidance that paid-traffic pages tend to be more optimized). If search is chosen, this only finds candidates here, it doesn't fetch or audit them yet, that happens in Step 4 after confirmation
- Industry-standards research: ask whether to skip it or include it. If included, this checks the specific benchmark this audit ends up citing against a live source, and pulls a short summary of current best practices for the chosen vertical. Both sub-questions are optional and independent, don't force one on the other
- Neither runs automatically. Both add real cost a single-page audit may not need
- Neither sub-question gets a "(default)" marking, ever. Both are deliberate cost/benefit calls with no context-inferable answer, present them as neutral, unmarked choices

**Group E (target input):**
- Single page: a URL. Accept pasted page content as a fallback if the URL can't be fetched
- Flow: an ordered list of URLs, with optional short labels per step (e.g. "cart," "checkout," "confirmation")

## Step 2: Fetch and verify every target

Fetch each URL live before auditing it. Never audit from memory, assumption, or general impressions of what a page "probably" contains, the live page is the source of truth. This applies to every step in a flow, not just the entry page.

If a fetch fails (blocked, JS-heavy rendering that doesn't resolve, unreachable), don't silently skip it and don't hard-stop the whole audit. Tell the user which target failed and ask how to proceed for that specific one (retry, pasted content, or drop the step) before continuing.

## Step 3: Load the reference material

Read `references/cro-audit-framework.md` (the six-element persuasion framework, definitions, audit questions, and applied methodology) and `references/cro-audit-supplementary-knowledge.md` (technical/performance data, analytics requirements, A/B testing methodology, industry benchmarks, business-model-specific lure/result patterns, and prioritization frameworks) as needed while working through Step 4. Both are synthesized reference material with dated, attributed sources, not something to paraphrase from memory.

## Step 4: Run the audit

Work through these layers in order:

1. **Technical/qualitative pass.** Check for visible signals only: oversized images, obvious layout-shift risk, mobile viewport and tap-target issues, glaring accessibility problems (missing form labels, low-contrast CTAs). State plainly this is not a measured performance test. If `chrome-devtools-mcp` is available (per the Pre-flight check), capture a screenshot for every specific defect raised anywhere in this audit, technical or six-element, not just this section, and save each alongside the report. These are mandatory evidence in Step 5 when the capability exists, not an optional extra, a defect described but not shown wastes the one advantage a live browser check provides over a plain fetch.

2. **Six-element framework**, branched by the business model chosen in Group B. Each element's *definition* in `references/cro-audit-framework.md` is the actual test, not the audit questions listed under it, those are illustrative examples pulled from one source, not an exhaustive checklist. If something on the page clearly undermines an element's stated purpose but isn't literally named in the reference file's bullet list, it still counts, raise it, don't stop at what's pre-named. Pull the model-specific lure/result guidance from the supplementary reference for how each element's purpose differs by business model. For a flow audit, explicitly check whether credibility and social proof carry through every step, not just the first one a visitor sees. Any claim this pass makes about a CTA, offer, or promised next step (a promo banner, a "see collection" link, a stated discount) must be traced to what it actually does, its real destination or mechanic, not inferred from wording or from another working element sitting nearby. A CTA existing near an offer is not the same as that CTA fulfilling the offer.

3. **Ground findings in data availability.** If the data-informed tier was chosen and the relevant data was actually provided, use it to support findings directly. If page-only, or if data-informed was chosen but the specific data needed for a given finding wasn't actually supplied, frame that finding as a hypothesis to validate, not a conclusion. This distinction matters, an audit that states unverified persuasion guesses as fact is worse than one that's honest about its own uncertainty.

4. **Competitor pass**, only if opted into in Group D. If the user supplied a URL directly, fetch and check it the same way as the primary target, don't skip verification just because it's secondary. If skill-search was chosen instead, search first, present the candidate competitors found, and wait for explicit confirmation before fetching or auditing any of them, don't audit a guessed competitor without the user confirming it's actually a fair comparison.

5. **Industry-standards research**, only if opted into in Group D. Freshness-check the specific benchmark this audit is about to cite (the one from `references/cro-audit-supplementary-knowledge.md` that best matches this business model/channel/device) against a live source. If it still matches, cite the reference file's figure as usual. If a live source gives a materially different, more current figure, disclose that plainly, state both the reference file's figure and the live one, and say which is being used and why, never swap one in silently. Also pull a short summary of current best practices for the chosen vertical, feed it into the six-element assessment in Step 5, not as separate/side content.

6. **Cite every benchmark.** Any conversion-rate figure, benchmark, or statistic pulled from the supplementary reference (or from the industry-standards research pass, if run) must include its source and year inline (e.g. "First Page Sage, 2026"). Never state a number without attribution, and never invent one that isn't in the reference file or the research pass's findings.

7. **Convert findings into hypotheses.** For each significant finding, write it as: Observation (what was found) → Change (the specific fix) → Mechanism (why it should help) → Prediction (what metric should move) → Falsifiability (what result would prove this wrong). This is what turns an opinion into something testable.

8. **Score with PIE.** Potential (how much room to improve), Importance (how much traffic/value the page or step carries), Ease (implementation difficulty), each 1-10, averaged. This is the fixed default, don't ask the user to choose a different scoring model.

## Step 5: Structure the output

**Report structure.** ALWAYS use this exact template for the saved file, filling in every bracketed section, don't drop any of them even when a section is empty (say so explicitly instead, e.g. "no competitor pass run"):

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>CRO Audit: [target name/URL]</title>
<style>
  html { background: #ffffff; scroll-behavior: smooth; }
  body { background: #ffffff; font-family: system-ui, sans-serif; max-width: 900px; margin: 2rem auto; line-height: 1.5; color: #1a1a1a; }
  h1, h2, h3 { line-height: 1.2; }
  .audit-meta { color: #666; font-size: 0.9em; margin-top: -0.5em; }
  .summary-facts { margin: 0.5em 0 1em; }
  .summary-facts li { margin: 0.2em 0; }
  .badge { display: inline-block; padding: 0.15em 0.6em; border-radius: 999px; font-size: 0.85em; font-weight: 600; }
  .badge.strong { background: #d1f5d3; color: #1a6b1a; }
  .badge.adequate { background: #fff3cd; color: #7a5c00; }
  .badge.weak { background: #f8d7da; color: #8a1c26; }
  table { border-collapse: collapse; width: 100%; margin: 1em 0; }
  th, td { border: 1px solid #ddd; padding: 0.5em 0.75em; text-align: left; vertical-align: top; }
  th { background: #f5f5f5; }
  .finding { border-left: 3px solid #ccc; padding-left: 1em; margin: 1em 0; }
  .finding.strong { border-color: #1a6b1a; }
  .finding.adequate { border-color: #7a5c00; }
  .finding.weak { border-color: #8a1c26; }
  .fix-link { font-size: 0.9em; }
  .screenshot { max-width: 100%; border: 1px solid #ddd; margin: 0.5em 0; }
</style>
</head>
<body>

<h1>CRO Audit: [target name/URL]</h1>
<p class="audit-meta">Audited: [DD/MM/YYYY, HH:MM, the actual date and time this run started, never a placeholder]</p>

<h2>Summary</h2>
<ul class="summary-facts">
<li><strong>Audit type:</strong> [single page / flow]</li>
<li><strong>Business model:</strong> [model]</li>
<li><strong>Data tier:</strong> [page-only / data-informed]</li>
<li><strong>Competitor pass:</strong> [not run / ran against (names)]</li>
<li><strong>Industry-standards research:</strong> [not run / ran]</li>
</ul>
<p>[Overall picture narrative, 2-3 sentences. Any time this narrative names a specific element (Credibility, Lure, etc.), link it to that element's section, e.g. weakest on <a href="#social-proof">Social proof</a>, don't state an element by name without linking it]</p>

<h2>Element overview</h2>
<table>
<tr><th>Element</th><th>Rating</th><th>Linked recommendations</th></tr>
<!-- one row per element, e.g.: -->
<tr><td><a href="#credibility">Credibility</a></td><td><span class="badge [strong|adequate|weak]">[rating]</span></td><td><a href="#rec-1">#1</a></td></tr>
<!-- if an element has no linked recommendation, write "none" plainly, don't leave the cell blank -->
</table>

<h2>Technical &amp; accessibility signals</h2>
<p>[State plainly whether this section is qualitative-only or was verified live, per the pre-flight check and Mandatory first response]</p>
<ul>
<!-- findings as distinct list items, not embedded in a paragraph -->
</ul>
<!-- if chrome-devtools-mcp was used this run, a screenshot is required for every defect discussed, not optional, embed with a real relative path: -->
<!-- <img class="screenshot" src="..." alt="[what it shows]"> -->
<!-- if chrome-devtools-mcp was not available, omit screenshots entirely, don't fabricate a placeholder image -->

<!-- repeat this block once per element: Credibility, Lure, Objection handling, Social proof, Ease of use, Result -->
<h2 id="credibility">Credibility <span class="badge [strong|adequate|weak]">[rating]</span></h2>
<div class="finding [strong|adequate|weak]">
[Narrative]
<!-- screenshot here too if chrome-devtools-mcp was used and this finding references a specific visual defect -->
<p class="fix-link">→ addressed by <a href="#rec-N">recommendation #N</a> [or: "no recommendation addresses this yet, gap in the audit's own logic, fix before delivering"]</p>
</div>

<h2>Industry-standards research</h2>
<p>[Only if opted into in Group D. Live-vs-reference benchmark comparison, best-practices summary for the vertical. If not opted into, write "not run this audit" plainly rather than omitting the section]</p>

<h2>Competitor comparison</h2>
<p>[Only if opted into in Group D. Per-competitor summary against the same six elements, lighter detail than the primary target. If not opted into, write "not run this audit" plainly rather than omitting the section]</p>

<h2>Prioritized recommendations</h2>

<h3>Just-do-it fixes</h3>
<ol>
<!-- each item notes which element it addresses, e.g.: -->
<!-- <li>[fix] (addresses <a href="#credibility">Credibility</a>)</li> -->
</ol>

<h3>High-impact / low-effort</h3>
<ol>
<!-- same back-link pattern -->
</ol>

<h3 id="scored-backlog">Scored test backlog</h3>
<table>
<tr><th>#</th><th>Recommendation</th><th>Addresses</th><th>Hypothesis (Observation → Change → Mechanism → Prediction → Falsifiability)</th><th>PIE</th></tr>
<!-- one row per backlog item, id="rec-N" on the # cell matches the finding's fix-link anchor above, e.g.: -->
<tr><td id="rec-1">1</td><td>[change]</td><td><a href="#credibility">Credibility</a></td><td>[full hypothesis]</td><td>[score]</td></tr>
</table>

<h2>Sources cited</h2>
<table>
<tr><th>Claim</th><th>Source</th><th>Year</th></tr>
<!-- one row per citation -->
</table>

</body>
</html>
```

Every element's `id` (line ~50) must match the `href="#..."` used in its Element overview row and in any recommendation's "Addresses" link. Every recommendation's `id="rec-N"` must match the `href="#rec-N"` used in the finding's fix-link. Build the cross-references as you write, don't add them as an afterthought pass, a finding with no linked recommendation is either a real gap (say so) or a sign a recommendation got dropped.

Each of the six elements gets a rating (the colored badge, Strong/Adequate/Weak), the narrative explanation, and a link to whichever recommendation actually fixes it. A bare rating without reasoning isn't useful, a narrative without a rating is hard to scan, and a finding without a fix-link is exactly the disconnection this structure exists to prevent.

**Delivery.** Save the full report as an HTML file, name it with the audit date (e.g. `cro-audit-[target]-YYYY-MM-DD.html`) so multiple runs sort chronologically on disk, present it, and explicitly state its file path (or a `file://` link) so it can be opened directly in a browser or copied for sharing, don't rely on an implicit file-card UI to surface this, it may not render the same way in every environment. Then give a condensed chat summary: the overall picture, the top 2-3 priority fixes, and a pointer to the saved file. Don't paste the entire report into the chat response, that defeats the point of having a saved, shareable file.

## Guardrails

- Never audit a page or flow from memory or assumption. Fetch it live, every target, every step.
- The technical section is qualitative only, unless the pre-flight check found `chrome-devtools-mcp` installed. Without it, never state or imply a measured Core Web Vitals score, Lighthouse result, or exact load-time figure, this skill has no access to those tools by default. With it, real measurements are available and should be used, but still label them as measured (with the tool) versus qualitative (without it), never blur the two.
- No update mode. Every run is a clean pass, there's nothing to diff against.
- Page-only findings, and any data-informed finding where the specific supporting data wasn't actually supplied, are always framed as hypotheses, never stated as settled conclusions.
- If the data-informed tier was chosen but data needed for a specific finding turns out to be missing or incomplete, stop and ask the user rather than silently falling back to page-only for that piece.
- If the business model doesn't clearly fit one of the six categories, ask clarifying questions until it does. Don't force the closest label.
- On a fetch failure, ask the user how to proceed for that specific target. Don't silently skip it and don't abandon the whole audit.
- Every benchmark or statistic cited must include its source and year, pulled from `references/cro-audit-supplementary-knowledge.md` or, if run, the industry-standards research pass, never invented or pulled from general training knowledge.
- Synthesize findings in original language. The underlying persuasion framework traces back to one training source, don't carry its branding or phrasing into the audit output.
- Run the competitor pass only if explicitly opted into in the setup interview (URL or search), never automatically. Industry-standards research is a separate, independent opt-in, don't couple the two.
- Search-found competitors are never fetched or audited without the user explicitly confirming the candidates first. A guessed competitor that turns out to be the wrong category or price tier wastes fetch cost and produces a misleading comparison.
- If industry-standards research finds a live benchmark that differs from the baked-in reference file, disclose both figures and which one is being used and why. Never silently substitute the live figure for the reference file's without saying so.
- Never use em dashes (—) anywhere in the report or in interview/chat text this skill produces. Use a period, comma, or parentheses instead.
- Each element's definition in `references/cro-audit-framework.md` is the test, not just its listed audit questions, those are illustrative, not exhaustive. Something that clearly undermines an element's stated purpose counts even if no bullet names it.
- Never mark a claimed offer, CTA, or promised next step as verified based on wording or proximity to another working element. Trace it to its actual destination or mechanic before scoring the element it belongs to.
