# Component Spec Generator - A Claude Skill

A Claude skill that documents an existing Figma component, reading its actual variants, states, and properties directly from Figma, filling in the rest through a short interview, previewing the whole thing as a styled HTML document, then writing it into Figma as a documentation frame beside the component, through Figma MCP.

---

## The Problem It Solves

A component without documentation is a component every new designer or developer has to reverse-engineer from scratch: which variant to use, what each state actually looks like, whether that boolean prop is safe to toggle, what the accessibility behavior is supposed to be. Writing that documentation by hand means manually listing every variant and property, checking they still match what's actually in Figma, and doing it all again the next time the component changes.

This skill reads the component directly instead of relying on someone's memory of it, drafts the parts a design tool can't know on its own, and keeps the result in a format both a person and a future AI skill can read.

---

## What It Does

On invocation, the skill asks whether this is a new spec or an update to an existing one, then:

- Reads the component's actual variant properties, boolean/text/instance-swap properties, anatomy, and nested atom components straight from Figma
- Drafts a suggested definition, when-to-use, and when-not-to-use based on that real structure, so you're editing a starting point instead of writing from a blank page
- Asks for the parts Figma genuinely can't supply: usage do's and don'ts, and accessibility notes, explicitly flagging whether the accessibility information is verified against a real implementation or just the design intent
- Builds a single, styled HTML preview covering all ten sections for approval before anything touches Figma
- Once approved, writes a documentation frame into Figma beside the component, styled to match the preview but with every color and text style bound to the target file's actual design system, not hardcoded
- Sets the component's native `description` and `documentationLinks` fields too, so the documentation is discoverable directly from the component itself, not just from its position on the canvas

**Update mode** works the same way but takes two links, the component and the existing documentation frame, diffs the frame's stored values against the component's current state, and only rewrites what actually changed, marking each changed value with an `UPDATED` badge in both the preview and the final frame. Everything unchanged on the existing frame is left untouched.

**Multiple components in one pass:** the skill can take on a batch of components in a single run rather than one at a time. If you're using it for the first time, or on a design system you haven't run it against before, it's worth testing on one or two components first to see how it handles your specific file's structure and token setup before handing it a large batch, that's a cheap way to catch a mismatch early rather than after twenty components have already been built.

---

## Requirements

- A Claude.ai account (Free, Pro, Max, Team, or Enterprise), or Claude Code
- Figma, with either the official Figma MCP or the Figma Console MCP / Desktop Bridge plugin connected. The skill asks which one to use, it does not assume
- The Knowledge Work Figma plugin installed, it bundles the Figma-side skills this skill depends on for reading and writing nodes
- Code execution enabled, if running in Claude.ai (Settings > Capabilities > Code execution and file creation)
- For the Figma frame to visually match the HTML preview, the target file needs its own text styles and color variables already set up (a prior Type Scale Generator and/or Color Scale Generator run, or an equivalent existing system). Without those, the skill will stop and ask how to proceed rather than hardcode a fallback

---

## Installation

1. Download `SKILL.md` from this repository
2. In Claude.ai: go to **Customize > Skills > Add Skill**, paste the contents of `SKILL.md`, save
3. In Claude Code: place the `component-spec-generator` folder in your skills directory, or install as a plugin

---

## Usage

Invoke the skill with a request like:

> Document this Button component in Figma.

Or for an update:

> Update the documentation for this component, here's the component link and the existing spec frame link.

The skill will ask upfront whether this is new documentation or an update, confirm your Figma integration, read the component, and show you a full preview before writing anything to Figma.

**A known Figma MCP limitation:** the Figma MCP has a recurring issue reading nodes on a page that isn't the currently open one in Figma, even when given a direct node link. This is a known limitation of the Figma MCP itself. The skill asks you to confirm the target page name and that it's currently open before reading, which avoids the failure rather than debugging it after a read comes back empty.

---

## Configuration Options

| Setting | Options |
|---|---|
| Mode | New documentation, or update an existing frame |
| Scope | Whole component set, or a single representative variant |
| Related atom components | Auto-detected from the layer structure, or specified directly |
| Accessibility notes | Marked as verified (checked against a real implementation) or unverified (design intent only) |
| Batch size | One component, or several in a single pass |

---

## A Note on AI Output

This skill can get things wrong. It's worth checking its work at two separate points, not one.

First at the preview stage, before anything reaches Figma, since a wrong variant list, a misread property, or an inaccurate drafted definition is cheapest to catch there. The drafted definition, when-to-use, and when-not-to-use fields in particular are a starting point, not a finished answer, they're built from the component's structure and general reasoning about what that structure implies, not from anything guaranteed to be correct. Second, after it builds in Figma: open the frame itself and look, don't rely on the summary the skill gives you when it finishes. A run can report success and still have bound the wrong token, missed an `UPDATED` badge, or left a variant out.

Worth knowing specifically about accessibility notes: the skill asks whether what you're telling it has actually been verified against a real implementation or is just the intended design behavior, and carries that distinction into the output. An "unverified" label just means it hasn't been checked yet, so treat that field as a prompt to go verify it, not as settled fact.

---

## License

CC0 - public domain. Covered by this repository's root `LICENSE` file, which applies to everything inside it. Use, adapt, and redistribute freely, no attribution required.
