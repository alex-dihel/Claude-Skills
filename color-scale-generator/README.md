# Color Scale Generator - A Claude Skill

A Claude skill that builds a complete color token system for a design system, sourced from Tailwind CSS's official palettes, previewed in a single detailed HTML document, then built into real Figma variables, text/icon contrast-tested against WCAG 2.1 AA, through Figma MCP.

---

## The Problem It Solves

Color tokens are tedious to set up correctly by hand. Every role needs a family, a starting intensity, a dark mode counterpart, and enough contrast to actually pass WCAG, checked against every background it's likely to sit on. Get any of that wrong and it doesn't show up until someone finds a button they can't read.

This skill runs that setup end to end, sources every value from Tailwind rather than memory, and shows you the full result, contrast ratios included, before anything gets built.

---

## What It Does

On invocation, the skill:

- Checks its own baked-in Tailwind color data against Tailwind's live docs at the start of every run, and offers to update itself if anything's changed or new
- Runs a setup interview: 2-tier or 3-tier structure, brand roles (primary, secondary, optional tertiary) mapped to Tailwind families, five always-on system roles (error, warning, success, information, neutral) picked from sensible family shortlists, a starting intensity per role, and which Figma integration to use
- Computes text and icon tokens tested against real WCAG 2.1 AA thresholds (4.5:1 text, 3:1 large text/icons), stepping through the palette toward a passing value on failure rather than shipping something illegible
- Builds a single combined HTML preview: raw palette swatches, an illustrative opacity reference, surface samples, and a full detail table showing every token's exact path, what it's bound to, and its contrast ratio, all before Figma is touched
- Once approved, builds the actual Figma variables (`Brand`, `Alias`, and `Mapped` for 3-tier) and a specimen frame for visual QA

It can build fresh in a new file, or add to an existing one (a file with a type scale already in it, for instance), and it never touches anything that isn't a color token unless explicitly told to.

---

## Requirements

- A Claude.ai account (Free, Pro, Max, Team, or Enterprise), or Claude Code
- Figma, with either the official Figma MCP or the Figma Console MCP / Desktop Bridge plugin connected. The skill asks which one to use, it does not assume
- Code execution enabled, if running in Claude.ai (Settings > Capabilities > Code execution and file creation)

---

## Installation

1. Download `SKILL.md` from this repository
2. In Claude.ai: go to **Customize > Skills > Add Skill**, paste the contents of `SKILL.md`, save
3. In Claude Code: place the `color-scale-generator` folder in your skills directory, or install as a plugin

---

## Usage

Invoke the skill with a request like:

> Set up a color system for this project, sourced from Tailwind.

Or more simply:

> Build color tokens for this design system.

The skill runs the setup interview in small batches, checks for an existing token structure in the target file, shows you a full preview with contrast ratios before building anything, and only writes to Figma after you approve it.

**Requests that will get redirected:**

> "Change our existing brand color" / "Update the error color across the whole system"

This skill builds new color token sets. It is not built for editing individual values in an existing, already-built system.

---

## Configuration Options

| Setting | Options |
|---|---|
| Structure | 2-tier (Brand + Alias, usage tokens bound directly to Brand) or 3-tier (Brand + Alias + Mapped, Alias as a 6-step semantic scale) |
| Brand roles | Primary, secondary, optional tertiary, any Tailwind family |
| System roles | Error, warning, success, information, neutral, always included, picked from a constrained shortlist per role, or a named custom family |
| Starting intensity | Default, lighter, darker, or a specific step, set independently per role |
| Figma integration | Official Figma MCP or Figma Console MCP / Desktop Bridge, asked upfront |

---

## A Note on AI Output

This skill can get things wrong. It's worth checking its work at two separate points, not one.

First at the preview stage, before anything reaches Figma, that's the cheap moment to catch a wrong family, a step that doesn't read as intended, or a token bound to the wrong thing. The detail table in the preview shows exactly what every token is connected to, use it. Second, after it builds in Figma: open the Variables panel and the styles themselves and look. Don't rely on the summary the skill gives you when it finishes. A run can report success and still have bound something incorrectly. Verify both times.

Worth knowing specifically where contrast is actually enforced: text and icon tokens get a hard pass/fail check. Borders don't get the same enforcement, since WCAG only requires it of a border when that border is the sole way a component is identified, which depends on how a token gets used later, not something knowable at generation time. The skill shows border contrast as reference information rather than gating on it, that's a deliberate, documented choice, not an oversight.

---

## License

CC0 - public domain. Covered by this repository's root `LICENSE` file, which applies to everything inside it. Use, adapt, and redistribute freely, no attribution required.
