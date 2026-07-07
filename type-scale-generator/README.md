# Type Scale Generator - A Claude Skill

A Claude skill that builds a complete typography type scale for a new design system, from setup through a previewable HTML mockup to real Figma variables, text styles, and a specimen frame, built through Figma MCP.

---

## The Problem It Solves

Setting up type for a new design system is one of the more tedious parts of getting one off the ground. Picking a scale, sizing every heading and body level, converting everything into consistent variables, wiring those into text styles, then doing it all again per breakpoint. None of that requires taste. It requires patience and consistency, which is exactly where manual setup tends to go wrong.

This skill runs that setup end to end and does it the same way every time.

---

## What It Does

On invocation, the skill:

- Runs a short setup interview: base size, typeface for heading and body (Google Fonts only), heading levels, body sizes, responsive modes, weight variants, and scale ratio
- Computes every fontsize and lineheight value, snapped to a shared numeric token scale rather than left as independent numbers
- Builds a real, rendered HTML preview for approval before anything touches Figma
- Once approved, builds the actual Figma variables (`Brand` and `Responsive` collections) and text styles, bound to those variables
- Finishes with a specimen frame in Figma showing every style applied, across every responsive mode, for visual QA

It does not update or extend an existing type scale. It is for new type scale creation only, and it says so before it starts.

---

## Requirements

- A Claude.ai account (Free, Pro, Max, Team, or Enterprise), or Claude Code
- Figma, with either the official Figma MCP or the Figma Console MCP / Desktop Bridge plugin connected. The skill asks which one to use, it does not assume
- Code execution enabled, if running in Claude.ai (Settings > Capabilities > Code execution and file creation)

---

## Installation

1. Download `SKILL.md` from this repository
2. In Claude.ai: go to **Customize > Skills > Add Skill**, paste the contents of `SKILL.md`, save
3. In Claude Code: place the `type-scale-generator` folder in your skills directory, or install as a plugin

---

## Usage

Invoke the skill with a request like:

> Set up a type scale for a new project. [Typeface] for headings, [typeface] for body, base [size]px.

Or more simply:

> Build me a type scale for a new design system.

The skill will run the setup interview in small batches, not all at once, wait for your answers, show you a preview, and only build in Figma after you approve it.

**Requests that will get redirected:**

> "Update our existing type scale" / "Add a new heading level to what we already have"

This skill is for new type scales only. It will tell you to look elsewhere for updates to an existing one.

---

## Configuration Options

| Setting | Options |
|---|---|
| Typefaces | Any Google Font, one for heading, one for body |
| Heading levels | H1-H6 by default, optional Hero level above H1 |
| Body sizes | sm/md/lg by default |
| Responsive modes | Desktop + Mobile by default, optional Tablet as a third mode |
| Scale ratio | Named modular ratios (Minor Second through Golden Ratio), or best-practice auto |
| Weights | Heading and body weights set independently, additional variants (e.g. a bold body) supported |

---

## A Note on AI Output

This skill can get things wrong. It's worth checking its work at two separate points, not one.

First at the preview stage, before anything reaches Figma, since that's the cheap moment to catch a wrong ratio or a size that looks off. Second, after it builds in Figma: open the Variables panel and the styles themselves and look. Don't rely on the summary the skill gives you when it finishes. A run can report success and still have missed a level, mismatched a responsive mode, or bound the wrong variable. Verify both times.

---

## License

CC0 - public domain. Covered by this repository's root `LICENSE` file, which applies to everything inside it. Use, adapt, and redistribute freely, no attribution required.
