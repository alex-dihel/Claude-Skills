---
name: developer-handoff-spec-generator
description: Generates a developer-facing handoff spec for an existing Figma target, a single Component/Component Set, or a composed frame built entirely from already-existing component instances, reading its structure via Figma MCP and filling out sizing, states, behavior, accessibility, responsive rules, edge cases, and composition via interview. Written to Figma as a documentation frame beside the target. Covers eight fixed sections, implementation detail for developers, not usability guidance, that's Component Spec Generator's job. Supports first-time documentation and updating an existing frame, diffing stored values. Checks Figma properties, Code Connect bindings, existing atom specs, and existing docs/code before asking a question, falling back to disclosed industry-standard defaults only when nothing else is available. Use whenever the user wants a developer handoff spec, implementation notes, a dev-facing component doc, or sizing/token/accessibility detail for engineering.
---

# Developer Handoff Spec Generator

## Mandatory first response
State: "This documents one existing target for developers, a single Component/Component Set, or a composed frame built entirely from already-existing component instances (like a form panel made of input and button components). It's implementation detail for developers, not usability guidance, that's Component Spec Generator's job. It's not for multi-screen flows, full pages, or cross-page navigation, that's a different tool. This skill can create documentation that doesn't exist yet, or update an existing documentation frame against the target's current state. Which do you need?" Wait for the answer before proceeding, this determines the rest of Step 1.

## Step 1: Setup interview

Ask in small grouped batches, not all at once. Wait for each answer before moving to the next group. Use the platform's native selectable-option tool when one is available (Claude.ai's option buttons, Claude Code's AskUserQuestion). Check for this tool before assuming it's absent, don't default to typed prose without verifying first. Fall back to plain numbered text only if no such tool exists. Never label an interview option "(recommended)," use "(default)" instead.

**Group 1, Figma integration (ask first, before any Figma query):**

*Questions to ask the user, via Claude.ai's option buttons or Claude Code's AskUserQuestion tool, whichever is actually present, not typed-out prose choices:*
- Which Figma integration to use: official Figma MCP, or Figma Console MCP / Desktop Bridge plugin. Never default to either without asking
- Which interface this is running in: Claude.ai, or Claude Code/Cowork. Skip this question only if genuinely obvious from context (active file-system or bash tools present), never guess silently otherwise
- The exact page name the target file is on

*Checks to perform after those answers, not user-facing questions:*
- Confirm the chosen integration is connected and responding. If not, give specific connection steps, don't fall back to a different one silently
- Claude.ai: confirm Figma MCP is connected via Settings > Connectors, tools actually respond
- Claude Code/Cowork: confirm the Knowledge Work Figma plugin is installed. If not installed, give install instructions (Customize > Plugins > Partners > Figma) and wait until the user confirms it's installed before continuing
- Confirm the named page is currently open before any read. The Figma MCP has a known, repeated issue reading nodes on a page that isn't the currently open one, even with a direct node link, and reads silently return wrong or empty data rather than erroring
- Once connection and plugin/connector are both confirmed, verify for real: load the component link the user gave in their first prompt and confirm it actually resolves. A positive self-report on connection/plugin isn't sufficient on its own
- Before any `use_figma` write call anywhere in this skill, load the `figma-use` skill first, every time, this is a hard prerequisite, not satisfied by the checks above
- Use this same integration for every Figma interaction in this run. If a read returns unexpected or empty data at any point, re-confirm the open page before assuming the data itself is wrong

**Group 2, target identification and detection:**
- New mode: ask for the Figma link to the specific target, component, component set, or composed frame. A file-level link isn't enough
- Update mode: ask for both the target link and the existing documentation frame link. Both are required
- Confirm every link resolves to an actual node via the chosen integration before continuing
- Detect the target type: Component/Component Set, or composed frame of instances. See `references/composed-target-handling.md`, read it before proceeding, it defines the read strategy for each case and the scope boundary (single unit vs. multi-screen flow, out of scope for the latter)
- If the target doesn't fit either case (a frame containing a functional part with real internal complexity, variants or states, that isn't componentized), stop and tell the user this falls outside what the skill can document. Suggest breaking the target down: document that part as a real component first, then re-run this skill. A simple, fixed-destination link or similar non-componentized element with no internal variance stays in scope, document it directly, no rejection needed

**Existing documentation check:**
- New mode (a real question, via selectable options, not a scan): ask the user directly whether a `Sidebar - Handoff`-style frame already exists near this target. Do not attempt to auto-discover it via a Figma metadata/page-listing call, that class of call (enumerating unknown content on a page) is unreliable regardless of which page is confirmed open, unlike a direct read of an already-known node ID, which is reliable. Found → ask how to proceed. Not found (or user isn't sure) → state that and proceed
- Update mode (action, this one is fine as a direct read): read the frame at the link provided in Group 2. This targets a specific known node ID, not an enumeration, so the same reliability concern doesn't apply here. If it doesn't match the `handoff/section-name` structure this skill builds, stop and ask the user to confirm it's actually a frame this skill produced

**Group 3, sourcing check (action, not a question):**
Load `references/sourcing-check.md` (folds in `references/code-connect-read.md` as its Code Connect step) and run it now, before generating any question for Groups 4-5. For each of the 8 fields, check: Figma property/variant → Code Connect binding (read-only) → existing atom doc, if the target is a composed frame → existing codebase/docs the user can point to. State back which fields resolved this way and which remain open, before continuing.

**Group 4, developer requirements:**
- Ask: have developer preferences/conventions already been gathered for this project?
- Yes → ask for them directly, use as given
- No → offer the structured optional questions: state-naming convention, error/edge-case terminology, disabled-field unlock logic, validation-message ownership
- Declined or skipped → load `references/accessibility-defaults.md` and use the listed industry-standard default for that sub-area. Every fallback value gets the disclosure tag `(Default)` appended, nothing longer. A value that was actually answered in the interview gets no tag at all, silence means confirmed, never write "confirmed" or attribute the answer to a specific role (developer, designer, etc.), the skill doesn't know who's running the interview
- Regardless of which tier resolves it: disabled-state logic always requires a stated trigger condition. Never leave "disabled" unexplained

**Group 5, remaining field gaps:**
Only what survived Groups 3 and 4 gets asked here, batched: sizing/tokens → states/behavior → accessibility → responsive rules → edge cases/composition.
- Sizing batch: covers width, height, padding, gap, radius, and token reference for each, per `project-requirements.md` §2. Most of this should already resolve via the sourcing check (it's usually a real Figma property), ask only what's left unresolved
- Parts batch: current name and built-from are already resolved in Step 2 via direct Figma reads, never asked here. Only ask if which state a part is present in genuinely isn't determinable from the target's own variant structure
- Composition batch: only the top-level structural fact (e.g. "Component Set, boolean variant `status`"). Don't re-enumerate the instances already listed in Parts, that's duplication, not new information
- Accessibility and behavior batch: load `references/annotation-schema.md`. For the target itself, landmark role, accessible name, and internal tab order are widget-level facts that must be determined, but if Group 3's sourcing check actually found them (a real Figma property or existing doc), use that, don't re-ask. Only ask what Group 3 left open. For each child (or the target itself, if not composed), determine its role from its real Figma structure and ask only the matching category from `annotation-schema.md`, button, link, heading, image/icon, or input, never every category for every target. Per-child accessible name/description: only ask if Group 3 found no existing atom doc to reference for that child. Apply the classification rule (behavior determines category, not which component was used) before picking which category to ask
- Responsive rules batch: don't assume a dedicated mobile variant is needed. Ask whether this target collapses predictably at smaller widths (most simple components do) or needs distinct mobile-specific behavior (most navigation/structural components do), and scope the question accordingly
- Edge cases batch: ask about missing content, overflow, and long-text handling for this target specifically. If the target is a composed frame, ask this once per relevant child, not once for the whole panel, different children can fail differently

## Step 2: Read the target from Figma

- Component/Component Set: pull variant properties and options, boolean/instance-swap/text properties, layer structure, nested instances
- Composed frame: walk the children per `references/composed-target-handling.md`, read each instance's own properties individually. Non-componentized decorative content (raw labels, static copy) has no property data to pull, treat it as content only. Non-componentized functional elements with no internal variance (a plain link with a fixed href) do have real data to pull, capture their actual target/destination, don't treat them as content-only
- For every instance child, resolve its original/canonical component name via its main-component reference, a direct Figma relationship, every instance has one. This is never an interview question, the current instance name may have been renamed locally, but the link back to its source component is always readable directly. Only ask the user if the direct read genuinely fails to resolve it
- Cross-check every field against the Group 3 sourcing-check results now that real node data exists. If a field the sourcing check marked "resolved via Figma property" turns out not to actually be there, flag it and fall through to the interview rather than silently leaving it blank
- Update mode only: read the existing documentation frame's stored values, compare against what was just pulled and against the current interview answers. Build the diff list, this drives the `UPDATED` badge in Step 4 and Step 5
- Present a checkpoint before drafting: target type, children if composed (with which already have atom docs found), the sourcing-check resolution list, and, in update mode, exactly which values differ from what's stored. This is the cheap point to catch a wrong read, not after the draft is written
- Never invent a part, sizing value, state, or property not actually present in the Figma data or explicitly supplied by the user

## Step 3: Draft the eight-section content

Combine the Step 2 checkpoint data, Group 3's sourcing-check resolutions, and Groups 4-5's interview answers into the fixed eight-section structure: parts, sizing, states, behavior, accessibility, responsive rules, edge cases, composition.

- Parts: current name plus original component name for every part, no usage description. Reference the original component's own doc via the sourcing check rather than restating what it does. Table columns: Instance, Built from, Present in, never label a column "Composition," that collides with the separate Composition section
- Composition: the top-level structural fact only (variant structure, how many top-level parts compose it). Never re-list what Parts already names, that's the same information twice

- Every value resolved via the sourcing check (Figma property, Code Connect, existing atom doc, existing codebase/docs) is written as a reference to that source, not restated as independent prose. A reader should be able to tell where each value actually lives
- Every fallback-default value carries its disclosure tag inline, in the same place a confirmed value would go, not in a separate footnote
- Update mode: any value differing from the existing frame gets an explicit `UPDATED` badge, no per-field date, the frame's `Last updated` line covers timing. Unchanged values carry no badge
- Never use an em dash or a double hyphen anywhere in generated content

## Step 4: HTML preview (external file, before touching Figma)

- Build a standalone HTML file rendering all eight sections with real, drafted content, not placeholders
- Match `assets/dev-handoff-style-reference.html`'s visual language exactly: fixed neutral card chrome, plain divider lines, shaded table headers, outline `(Default)` badges, filled `UPDATED` badges, check/x marks for genuinely boolean cells. This is a fixed preset, not derived from the target file's own brand colors, see `references/figma-visual-mapping.md` for the full chrome-vs-content distinction
- Render the sourcing-check references, the disclosure tags, and (update mode) the `UPDATED` badges using the same badge visual treatment, so a reader can distinguish confirmed values, referenced values, defaulted values, and changed values at a glance
- Sizing and properties render as actual tables, matching what will be written to Figma
- Save as an actual file, not an in-chat-only render. Filename: `[target-name]-handoff-preview.html`, same file overwritten in place on revision, not a new copy each time, this exact path is what Step 5's compare pass re-reads. Do not touch Figma during this step
- Present for approval, iterate until approved

## Step 5: Build the Figma documentation frame (only after preview approval)

**Pre-build check:**
- Reconfirm the Figma integration is still connected
- Reconfirm the target link, and, in update mode, the existing documentation frame link
- Reconfirm the target page is still the currently open one before any write, the page-read/write issue applies to writes too, not just reads
- Load the `figma-use` skill before the first `use_figma` call in this step, this is a hard prerequisite, not assumed from Group 1
- New mode: reconfirm the Group 2 answer on whether an existing frame is present, ask again only if meaningful time passed or the user seems unsure, don't run a fresh Figma metadata/page-listing call here either, same reliability concern as Group 2
- Update mode: re-run the diff immediately before writing, in case the target changed since the checkpoint

**Placement:**
- New mode: determine whether the target is an original definition or an instance. If original, create a new page named `↘ [Target Name]`, duplicate the target there, build the frame beside the duplicate. If instance, build beside it on its current page, no duplication
- Update mode: the frame already exists, edit in place, don't move or recreate it
- If a Component Spec Generator frame already exists for this same target, place the new frame consistently alongside it, matching that existing arrangement rather than imposing a different layout, per `process-guide.md`'s naming/structure convention
- Frame name: `[Target Name] - Handoff` (plain hyphen)
- New mode: don't rely on a Figma metadata/page-listing call to check whether the placement area is occupied, same reliability concern as the existing-documentation check in Group 2. Place the frame at a fixed offset from the target (e.g. directly to its right, a set distance clear of its bounds) and visually confirm via the chosen integration that the frame itself, once placed, doesn't overlap anything. If it turns out to overlap something after placing, move the new content, never the original target or instance

**Structure:**
- One top text layer `handoff/meta`: `Target: [name]`, `Type: [Component/Component Set/Composed Frame]`, `Source: [link]`, `Documented: [ISO 8601 date]`. Update mode adds `Last updated: [ISO 8601 date]` above the original `Documented` line
- Eight sections as flat, named layers: `handoff/parts`, `handoff/sizing`, `handoff/states`, `handoff/behavior`, `handoff/accessibility`, `handoff/responsive-rules`, `handoff/edge-cases`, `handoff/composition`. Flat naming, not nested, matching this project's convention
- **Mandatory prerequisite, load before writing a single layer**: `references/figma-visual-mapping.md`. Do not build any part of this frame from memory of what the mapping said earlier in this run, load and apply it fresh for every section. Match the layout and set of elements shown in `assets/dev-handoff-style-reference.html` exactly, header shading, table borders, badge fills, plain bordered callout boxes, check/x marks, all of it. A frame built only from text layers with no fills or strokes is not a simplified version of this requirement, it's a failure of it, stop and rebuild rather than presenting it as done. Chrome (card border, divider color, header shading, badge palette, check/x colors) is a fixed preset, hardcoded per `figma-visual-mapping.md`, not bound to the target file's own tokens. Content (every actual sizing value, token name, state name, accessible name) is never hardcoded, it comes from real Figma data or a disclosed default. If the file has no text styles at all for the document's own typography, fall back to a plain system font per `figma-visual-mapping.md`, that's the one chrome exception allowed to substitute rather than stop and ask
- **Every table's own binding-status column (Sizing's Token column, and any other table showing whether a value is DS-token-bound) uses a real check/x icon element plus a real badge frame, the same construct already required for the Accessibility table's `(Default)` badges.** Applying this treatment to only some tables and falling back to plain text joined by punctuation for others is exactly the kind of partial application this guardrail exists to prevent, verify every table got it, not just the first one built. Never join a value and its badge label with an em dash or any punctuation as a substitute for the actual badge construct
- **The States section's Figma-property disclosure uses the same real badge construct, not parenthetical text.** No exceptions for single-line sections, a one-line section still gets a real badge, not a text description of one
**Compare-and-correct pass (before reporting the build as complete, second checkpoint, not the build session's own summary):**
- Read the saved preview file (`[target-name]-handoff-preview.html`) and the built frame, both fresh, not from memory of drafting either one
- Walk section by section, in this order: meta, parts, sizing, states, behavior, accessibility, responsive rules, edge cases, composition, note. For each: does the built frame use the same construct as the preview (real badge vs. plain text, real check/x icon vs. none), and does the wording match, not just visually but the actual text
- List every mismatch found before fixing anything, then fix each one, then re-check that specific section only, don't re-verify sections that already matched
- The preview is ground truth. The build is a transcription of what was approved, not a fresh drafting pass, if wording differs, match the preview, don't rationalize the build's version as equally valid
- Update mode: this still covers all nine parts of the frame, since the preview always renders full content regardless of mode, but only write corrections to layers that actually have a real mismatch, untouched layers that already match don't need rewriting
- Only report the build complete after this pass finds nothing left to fix
- Sizing and properties render as actual tables
- References (Code Connect bindings, atom-doc links, existing codebase/doc pointers) render as visible links or named pointers, not just prose mentions
- Update mode: write only layers whose content actually changed per the Step 2 diff, tagged `UPDATED`, everything else untouched
- Never hardcode a value Figma can supply directly

**Target node fields (write on the target itself, in addition to the frame):**
- Set `description`: a short summary, one sentence on the target's dev-relevant nature (e.g. "Composed panel, 4 input instances + 1 button instance") plus the single most load-bearing implementation caution
- Set `documentationLinks` to the handoff frame. If a Component Spec frame's link is also present, don't overwrite it, this skill adds its own link, it doesn't replace another skill's
- Update mode: only rewrite these if the underlying content changed
- Confirm every field reads back correctly after writing

**On failure:** stop immediately, report exactly what was created and what wasn't, ask before retrying or rolling back.

## Guardrails

- New mode and update mode both supported. New mode never overwrites or duplicates an existing frame without confirmation. Update mode only touches layers that actually changed
- In scope: a single Component/Component Set, or a composed frame where every functional part with real internal complexity (variants, states) is an instance of an already-existing component. A simple, fixed-destination non-componentized element (a plain link) stays in scope, document it directly. Out of scope: multi-screen flows, cross-page navigation, system-wide error bucketing, file-wide landmark structure. Redirect, don't attempt
- Two separate skills, not a mode switch: never create or edit Component Spec Generator documentation
- Code Connect is read-only. Reference bindings, never create or edit a mapping, that's `figma-code-connect`'s job
- Before generating any interview question, run the sourcing check (Figma property → Code Connect → existing atom doc → existing docs/code). Only what survives gets asked. State back what was skipped and why
- Every fallback-default value gets the tag `(Default)` appended, nothing longer, no attribution of who did or didn't confirm it. Answered values get no tag at all, don't invent "confirmed with [role]" language anywhere, the skill doesn't know who's running the interview. Disabled-state logic always requires a stated trigger condition regardless of which tier resolved it
- Composed-frame detection happens before anything else in Step 1. For composed frames, walk instance children individually, don't expect top-level variant data
- Always document, widget-level, for the target itself: landmark role, accessible name, internal tab order. Never document focus order or landmark relationships relative to content outside the target
- Per-child accessible name/description: reference an existing atom doc if the sourcing check finds one, document inline only if it doesn't
- Accessibility and behavior content follows `references/annotation-schema.md`'s role-conditional categories (button, link, heading, image/icon, input), determined from real Figma structure. Never ask every category for every target, and never classify an interactive element by which component built it, classify by what it actually does (navigates vs. acts in place)
- Never invent a part, sizing value, state, property, or composition entry not actually present in Figma data or explicitly supplied by the user
- Parts carries name pairs (current + built-from) and no usage descriptions. Composition carries only the top-level structural fact, never a re-listing of what Parts already names. Parts table columns never include one named "Composition"
- Never skip the HTML preview or touch Figma before it's approved
- Never default to a specific Figma integration, always ask in Step 1
- Never proceed with Figma reads or writes before confirming the interface-appropriate connection: Figma MCP via Settings > Connectors in Claude.ai, or the Knowledge Work Figma plugin in Claude Code / Cowork
- Never call `use_figma` without loading the `figma-use` skill first, every time, not just once per run. This applies to every write in Step 5, including page creation, not only the final frame build
- Always confirm the target page name and open state before a Figma read, first, before any other Group 1 step. If a read comes back empty or unexpected, re-check the open page before trusting the data
- Never label an interview option "(recommended)," use "(default)"
- Never use an em dash or a double hyphen anywhere in generated output
- The frame's layer-naming convention (`handoff/section-name`, flat) is fixed
- `description` and `documentationLinks`, not frame placement or name, are the actual discovery mechanism. Set both, confirm they read back correctly
- `references/figma-visual-mapping.md` is a mandatory prerequisite for Step 5, load it fresh before writing, not from memory of an earlier read. The HTML preview matches `assets/dev-handoff-style-reference.html`'s visual language exactly, a fixed neutral preset, not derived from the target file's brand colors. A frame built from text layers alone, no fills, no strokes, no shading, no badges, is a failed build, not an acceptable simplification. Verify by reading the built frame back and confirming fill/stroke properties are actually present before reporting success
- Update mode markers are a plain `UPDATED` badge only, never a per-field date
- On partial build failure, stop and report, don't proceed silently
- If this target already has a Component Spec Generator frame, place this frame consistently alongside it and never overwrite that other frame's link in `documentationLinks`
