# Example Output: CMO Design Headcount Reduction Plan

This file shows the Critical Thinking skill applied to `test-artifact.md`.

**Invocation used:**
> Use critical thinking to evaluate this business plan: [contents of test-artifact.md]

---

> Persona: Senior adviser whose reputation depends on catching bad ideas before they get approved (primary). CFO lens applied for financial findings (secondary).

---

### Findings

**[Blocking] Productivity multiplier is asserted with no source or baseline**

- **Original:** "We assume a 2.2x per-designer productivity multiplier for the retained team once tooling is adopted."
- **Issue:** The entire savings case rests on this number and there is no source for it. It is not measured against a current baseline (the plan never states current output per designer), not benchmarked against comparable implementations, and not validated by a pilot. If the real multiplier is 1.3x, the 13-person team produces less than the current 25 while the department claims savings.
- **Solution:** Needs information. Run a time-boxed pilot measuring actual finished output per designer with the tooling before any headcount action. State current baseline output first, or the multiplier has nothing to multiply.

**[Blocking] Conflict of interest is built into the document**

- **Original:** "Prepared by: VP Marketing / CMO" - proposing cuts to the department that reports to that same CMO, closing with "the marketing organization will demonstrate cost discipline ahead of the next budget cycle."
- **Issue:** The author benefits from demonstrating cost discipline before budget season and achieves it by cutting their own subordinates' roles. The capability loss is borne by the department; the political credit accrues to the author. Every optimistic assumption in the plan (2.2x multiplier, one-quarter adjustment period, negligible risk, proven tooling) points in the direction that benefits the author. None were independently verified.
- **Solution:** Needs decision. Require an independent review of the productivity and risk assumptions by someone without a stake in the budget-cycle outcome before the numbers go to committee.

**[Material] Eliminating all junior roles kills the talent pipeline**

- **Original:** "Junior | 0" / "Eliminated: 5 junior."
- **Issue:** Mid and senior designers are grown from juniors. In five years the retained mid-level designers will advance or leave, leaving no internal supply to replace them. The plan is silent on this, because the cost lands just outside the five-year model window and then compounds. The model shows savings and ignores the replacement cost it is deferring.
- **Solution:** Needs decision. Either retain a reduced junior cohort explicitly as pipeline, or accept and budget for all-external senior hiring in years four to six. The current plan does neither.

**[Material] Senior and mid-level time reallocation is unmodeled**

- **Original:** "AI tools replace roughly half of mid-level output, with the remaining mid and senior designers directing, prompting, and finishing AI-generated work."
- **Issue:** This relocates work rather than eliminating it, then does not count the relocation. Senior designers at $175k now spend hours prompting, reviewing, and correcting AI output. That is the most expensive labor in the department doing production QA. Their salaries are counted as retained but their capacity is now partially consumed by work juniors used to handle. The net capacity gain is overstated by an unmodeled amount.
- **Solution:** Model the senior and mid-level time reallocation explicitly. Subtract the hours retained staff spend supervising AI output from the productivity multiplier before claiming a net gain.

**[Material] Output is measured in volume only, with no quality or revenue link**

- **Original:** The plan measures replacement in terms of production tasks (asset resizing, template population, layout automation) with no quality dimension.
- **Issue:** Design exists to produce business outcomes, not asset count. If AI-assisted output is lower quality or inconsistent with brand standards, the cost saving is offset by weaker campaign performance. That link is never examined. The plan optimizes the cost of the department without reference to what the department is for.
- **Solution:** Needs information. Define the output quality bar and tie at least one revenue or performance metric to design output before approving a capability reduction.

**[Material] Risk section is a dismissal, not an assessment**

- **Original:** "The primary risk is a short adjustment period while the remaining team adapts to the new tooling. We expect this to resolve within one quarter. Tooling cost may rise, but is negligible."
- **Issue:** The section names one risk and waves it off. Omitted entirely: quality regression, attrition of retained staff now doing less satisfying work, loss of institutional knowledge from the 12 who leave, AI tool vendor lock-in, IP and licensing exposure on AI-generated assets, and the morale effect of a 48% cut on survivors. A risk section that finds almost no risk in halving a department is not credible.
- **Solution:** Rebuild the section with the omitted risks, each with a likelihood and a mitigation, or state explicitly that each was considered and judged immaterial with reasoning.

---

#### CFO Lens

**[Minor] Severance costs are missing and tooling cost is held flat**

- **Original:** "Net annual saving, year 1: $1,120,000" and the five-year projection table.
- **Issue:** Twelve redundancies in quarter one carry one-time costs (notice pay, statutory redundancy, accrued leave, possible legal exposure) that are not netted against year-one savings. Year one is overstated. Separately, enterprise AI tooling cost is held flat across five years in a category where prices are rising fastest. Both errors push in the same direction: savings look larger than they are.
- **Solution:** Add a one-time severance line to year one. Model tooling cost with a realistic escalator rather than flat.

---

### Verification

- **Do not trust without independent verification:** The 2.2x productivity multiplier, the one-quarter adjustment period claim, the assertion that tooling is "proven," and the "negligible" five-year tooling cost trajectory. Every one of these is asserted by a party with a direct interest in the conclusion and none carries a source, a benchmark, or a pilot result.
- **Missing context that would change this analysis:** Current baseline output per designer (without it the multiplier is meaningless), severance cost figures for the relevant jurisdiction, the actual enterprise tooling quote and renewal terms, a quality or revenue metric tied to design output, and the company's legal redundancy obligations. Any of these could shift the plan from net positive to net negative.

---

**Verdict: Do not proceed.** The savings case rests entirely on an unvalidated productivity multiplier. Until it is tested against a measured baseline, there is no number to approve. The two Blocking findings are not fixable by editing the document.

> "Evaluation complete."
