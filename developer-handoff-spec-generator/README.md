# Developer Handoff Spec Generator - A Claude Skill

A Claude skill that documents an existing Figma component, component set, or composed frame for developers, sizing, states, behavior, accessibility, responsive rules, edge cases, and composition, read from real Figma data and written to Figma as a documentation frame beside the target.

---

## The Problem It Solves

Developer handoff docs are tedious to write and easy to get wrong by omission: a disabled state with no stated trigger, an icon-only button with no accessible name, a padding value nobody bound to a token. None of that requires design judgment, it requires actually checking. This skill checks first: Figma properties, Code Connect bindings, existing component docs, before asking anything, and falls back to disclosed industry-standard defaults (WAI-ARIA, WHATWG HTML, HTTP status semantics) only when nothing else answers the question.

---

## What It Does

On invocation, the skill:

- Detects whether the target is a Component/Component Set or a composed frame (a panel built from already-existing component instances, plus simple non-componentized elements like a plain link)
- Runs a sourcing check against every field before asking about it: Figma property, Code Connect binding, an existing atom-level doc, or existing codebase/docs. Only what's genuinely unresolved becomes an interview question
- Gathers developer-specific conventions as a real interview step, falling back to disclosed industry-standard defaults when preferences haven't been established
- Applies a role-conditional accessibility schema (button, link, heading, image/icon, input, landmark) based on what each part actually is, not a generic checklist
- Builds a standalone HTML preview in a fixed, neutral visual preset, independent of the target file's own brand colors, for approval before anything touches Figma
- Writes a documentation frame into Figma matching that preview exactly, verified with a dedicated compare-and-correct pass against the saved preview, not just a self-reported summary
- Supports first-time documentation and updating an existing frame, diffing stored values against the target's current state and marking only what changed

It's scoped to a single documentable unit. Multi-screen flows, cross-page navigation, and file-wide navigation structure are out of scope.

---

## Requirements

- A Claude.ai account (Free, Pro, Max, Team, or Enterprise), or Claude Code, or Claude Cowork
- Figma, with either the official Figma MCP or the Figma Console MCP / Desktop Bridge plugin connected. The skill asks which one to use, it does not assume
- In Claude.ai: Figma connected via Settings > Connectors. In Claude Code or Cowork: the Knowledge Work Figma plugin installed
- Code execution enabled, if running in Claude.ai (Settings > Capabilities > Code execution and file creation)

---

## Installation

This skill depends on its `references/` and `assets/` files at runtime, not just `SKILL.md`, pasting `SKILL.md` alone will leave it missing files it needs while running.

**For Claude.ai:**
1. Open [`developer-handoff-spec-generator.skill`](./developer-handoff-spec-generator.skill) in this folder on GitHub, then click **Download raw file**
2. Go to **Customize > Skills > Add Skill**, and upload the downloaded `.skill` file

**For Claude Code:**
1. Clone this repository (or download the `developer-handoff-spec-generator` folder, containing `SKILL.md`, `references/`, and `assets/`)
2. Place the `developer-handoff-spec-generator` folder in your skills directory, or install as a plugin

---

## Usage

Invoke the skill with a request like:

> Document this Figma component for developer handoff: [link]

Or for an update:

> Update the developer handoff doc for this component against its current state: [link]

The skill runs the setup interview in small batches, tells you which fields it already resolved from real Figma data before asking anything else, shows you a full preview, and only writes to Figma after you approve it.

**Requests that will get redirected:**

> "Document this whole onboarding flow" / "Write usability guidance for this component"

This skill covers one component, component set, or composed frame at a time, implementation detail for developers. Multi-screen flows are out of scope. Usability guidance (when to use it, do's and don'ts) is Component Spec Generator's job, not this skill's.

---

## Configuration Options

| Setting | Options |
|---|---|
| Mode | New documentation, or update an existing frame against the target's current state |
| Figma integration | Official Figma MCP or Figma Console MCP / Desktop Bridge, asked upfront |
| Target type | Component/Component Set, or a composed frame of existing instances plus simple non-componentized elements |
| Developer requirements | Already-gathered preferences, a structured optional interview, or disclosed industry-standard defaults |
| Accessibility content | Role-conditional per part (button, link, heading, image/icon, input, landmark), not a fixed checklist |

---

## A Note on AI Output

This skill can get things wrong. It's worth checking its work at two separate points, not one.

First at the preview stage, before anything reaches Figma, that's the cheap moment to catch a wrong field, a missing part, or a defaulted value that should have been a real answer. Second, after it builds in Figma: open the frame and look, specifically checking that binding-status columns and disclosure lines rendered as real badges and icons, not plain text, and that the wording actually matches what you approved in the preview. The skill runs its own compare-and-correct pass against the saved preview before reporting done, but verifying independently is still worth doing, a run can report success and still have missed something.

Figma's own metadata and page-listing calls are known to be unreliable for discovering unknown content (an existing documentation frame, an occupied placement area). The skill asks directly rather than relying on that class of call. Direct reads of an already-known node are reliable and used throughout.

---

## License

CC0 - public domain. Covered by this repository's root `LICENSE` file, which applies to everything inside it. Use, adapt, and redistribute freely, no attribution required.
