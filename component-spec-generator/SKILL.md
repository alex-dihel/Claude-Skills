---
name: component-spec-generator
description: Generates a design spec document for an existing Figma component, reading its variants, states, and properties from Figma via MCP, filled out with a definition, usage guidance, and accessibility notes via interview, and written to Figma as a documentation frame beside the component. Covers ten fixed sections - definition, when to use, when not to use, variants, states, anatomy, properties, usage rules (do/don't), accessibility notes, related atom components. Structured for human and AI readability, so other skills can read this as context later. Supports first-time documentation and updating an existing frame against the component's current state, diffing stored values. Use whenever the user wants to document a Figma component, update component documentation, write a component spec, create design system documentation, or asks for usage guidelines, do's and don'ts, or accessibility notes, especially mentioning component spec, component documentation, design spec, or a documentation frame in Figma.
---

# Component Spec Generator

## Mandatory first response
State: "This skill can create documentation for a component that doesn't have it yet, or update an existing documentation frame against the component's current state. Which one do you need?" Wait for the answer before proceeding, this determines the rest of Step 1.

## Step 1: Setup interview

Ask in small grouped batches, not all at once. Wait for each answer before moving to the next group. Use the platform's native structured selection tool when available, fall back to plain numbered text only if none exists. Across every question in this interview, never label an option "(recommended)," use "(default)" instead, "recommended" implies a judgment this skill hasn't earned, "default" just states what happens if the user doesn't specify otherwise.

**Group 1, Figma integration (ask first, before any Figma query):**
- Ask which Figma integration to use: official Figma MCP, or Figma Console MCP / Desktop Bridge plugin. Never default to either without asking
- Confirm it's connected and responding. If not, give specific connection steps, don't fall back to a different one silently
- Confirm the Knowledge Work Figma plugin (bundles the Figma skills this skill depends on for reading and writing nodes) is installed. If it isn't, point the user to it and stop until it's confirmed installed, don't attempt Figma reads or writes assuming it's there
- Use this same integration for every Figma interaction in this run
- Ask for the exact page name the target file is on, and confirm that page is currently open in the Figma desktop or web app before any read. The Figma MCP has a known, repeated issue reading nodes on a page that isn't the currently open one, even when given a direct node link. Confirming the page name and open state upfront avoids the failure rather than debugging it after a read comes back empty or wrong

**Group 2, component identification:**
- New mode: ask for the Figma link to the specific component or component set to document, not just the file. A file-level link isn't enough, the skill needs the exact node
- Update mode: ask for both the Figma link to the component and the Figma link to the existing documentation frame. Both are required, don't proceed with only one
- Confirm every link provided resolves to an actual node via the chosen integration before continuing
- If the component resolves to a component set, ask whether this spec covers the whole set (the default assumption, since the ten sections are written to cover the full range of variants) or a single representative variant only. If a single variant, note that plainly in the draft and in `spec/meta` in Step 5, don't let it pass as if it covers the full set
- If it resolves to a single component with no variant properties at all, note this too, the Variants section in the output will read "Not applicable, single component" rather than being left blank or invented

**Existing documentation check (action, not a question):**
- New mode: once the component resolves, scan the same page (and the new documentation page, if one already exists for this component) for an existing documentation frame associated with it. Found → stop, tell the user it already has documentation, ask how to proceed (leave it alone, or they've confirmed this really is a fresh build and the existing frame is stale/unrelated). Do not silently build a second one or overwrite. Not found → state that plainly and proceed
- Update mode: read the frame at the link provided in Group 2. If it doesn't match the `spec/section-name` structure this skill builds, stop and ask the user to confirm it's actually a frame this skill produced, don't attempt to update a frame with an unrecognized structure

**Group 3, content that Figma cannot supply:**
Figma can tell this skill what a component's variants, states, and properties are, but not why it exists or how it should be used. Ask for:
- Tell the user upfront: "Once I've read the component's structure, I'll draft a suggested definition, when-to-use, and when-not-to-use for you to keep, edit, or replace, rather than asking you to write them from a blank page." This is a heads-up only, the actual drafting happens in Step 2 once real component data exists to draft from, not here
- Usage rules: do's and don'ts
- Accessibility notes: keyboard behavior, screen reader behavior, any other behavioral notes. Also ask directly whether this is confirmed, verified behavior (checked against a real implementation or a developer) or the intended/design-target behavior that hasn't been verified yet. Record which one it is, don't let an unverified answer get written into the final document with the same unqualified confidence as a verified one, since this document is meant to be read as ground truth by other skills later
Offer the option to point to an existing brief or doc to pull these from instead of typing them fresh, if the user has one.

**Group 4, related atom components (the question only, not the detection):**
- Ask whether this component is composed of smaller atom components that should be cross-referenced in the spec, and whether the user wants these auto-detected from the Figma layer structure or will specify them directly
- Do not attempt auto-detection here, the component hasn't been read yet at this point in the flow. The actual detection and the user's confirmation of the detected list both happen in Step 2's checkpoint, once real data exists to confirm against

## Step 2: Read the component from Figma

- Using the chosen integration, pull the component's actual node data: all variant properties and their options, boolean/instance-swap/text properties, layer structure for anatomy naming, and any nested component instances
- If two or more properties share the exact same display name but differ in type (e.g. a Boolean `iconLeft` and an Instance-swap `iconLeft`), keep both rows using that same exact name, don't invent a numeric suffix or any other altered name to tell them apart. Use the Type column to distinguish them and add the standard cross-reference note explaining it's a naming coincidence in the source file, not a duplicate row, exactly as shown in `references/template-and-examples.md`
- If Group 4 indicated auto-detection, this is where it actually happens: scan the layer structure for nested component instances and build the candidate atom-component list from real data, not before
- Draft the definition, when-to-use, and when-not-to-use fields now, using this real component data (name, anatomy, properties, nested atom components) plus ordinary component-taxonomy reasoning, a component with a click-trigger anatomy and a text label reads as a button-type control, a component wrapping a checkbox atom reads as a selectable list item, and so on. Use `assets/preview-style-reference.html` as the quality and tone bar, concrete and specific, not generic filler. Present each draft plainly for the user to keep, edit, or replace, never commit a drafted field without that confirmation
- Interactive states (hover, focus, pressed, disabled, etc.) are not always encoded as Figma variants. Check variant properties first; for any state that isn't represented as a variant, ask the user directly rather than guessing or inventing one
- Update mode only: also read the existing documentation frame's stored values (from Group 2's second link). Compare each section's stored content against what was just pulled from the live component. Build a list of what differs, this drives the checkpoint below and the `UPDATED` badge marking in Step 3 and Step 5. The frame's `spec/meta` `Last updated` date already covers timing, individual changed fields get the badge only, not a repeated per-field date
- Present what was extracted back to the user as a single checkpoint before drafting the remaining sections: the variant list (or the "not applicable" note if it's a single component), the properties table, the anatomy names, and the detected atom components for confirmation or edits. Update mode additionally shows exactly which values differ from what's currently stored in the frame. This is the point where a misread part name, a missed variant, a wrong atom-component guess, or a false-positive diff is cheapest to catch, don't let it ride silently into the draft
- Never invent a variant, state, property, or anatomy name that isn't actually present in the Figma node data or explicitly supplied by the user in Step 1

## Step 3: Draft the ten-section content

Combine the Step 2 checkpoint data, the Step 2 drafted definition/when-to-use/when-not-to-use, and the Step 1 Group 3 interview answers into the fixed ten-section structure. Carry the verified/unverified flag from the accessibility answers straight into the Accessibility Notes section, if unverified, label it plainly as such in the output (e.g. "Unverified, design intent only") rather than presenting it with the same confidence as confirmed behavior. Update mode only: any value that differs from what's currently stored in the existing frame gets an explicit `UPDATED` badge next to it, no per-field date, the frame's `Last updated` line in `spec/meta` already covers timing. Everything unchanged stays as is with no badge. See `references/template-and-examples.md` for the exact fill-in template and two fully worked examples (a simple component and a composite one), read it before drafting so the output format stays consistent across runs rather than reinvented each time.

Never use an em dash or a double hyphen anywhere in the generated content, any section text, table cell, or note. Use a comma, a period, or a parenthetical instead.

The ten sections, in order: definition, when to use, when not to use, variants, states, anatomy, properties, usage rules, accessibility notes, related atom components.

## Step 4: HTML preview (external file, before touching Figma)

- Build a standalone HTML file rendering all ten sections with the real, drafted content, not placeholders
- Replicate the visual structure of `assets/preview-style-reference.html` as closely as possible: the meta header block, underlined section headers, tables with a shaded header row, callout notes with a left accent border for cross-reference and naming-coincidence notes, the flag badge style for unverified accessibility content, and the two-column do/don't layout. This preview is a review tool, not a shipped artifact, its colors and fonts don't need to bind to the target file's design system, keep them hardcoded exactly as in the reference asset
- Show the properties and variants as an actual table, matching what will be written to Figma
- Update mode only: render each `UPDATED` badge visibly next to the value it applies to, no per-field date, using the same flag-badge visual treatment as the unverified-accessibility marker so changed values are easy to spot at a glance
- Save it as an actual file the user can open and download, do not rely on an in-chat-only render
- Do not touch Figma during this step
- Present for approval, iterate until approved

## Step 5: Build the Figma documentation frame (only after preview approval)

**Pre-build check:**
- Reconfirm the Figma integration chosen in Step 1 is still connected and responding
- Reconfirm the target component link, and, in update mode, the existing documentation frame link
- New mode: re-run the existing-documentation scan from Step 1 one more time immediately before writing, in case anything changed since setup
- Update mode: re-run the diff from Step 2 one more time immediately before writing, in case the component changed since the checkpoint was shown

**Placement:**
- New mode only, first determine whether the linked node is an original (the main component or component set definition) or an instance (a placed copy referencing that definition elsewhere in a design)
- If original: create a new page in the same document, named `↘ [Component Name]` (the arrow prefix marks it as a generated documentation page, not a regular design page). Duplicate the component onto that new page (the original stays untouched wherever it already lives), then build the documentation frame beside the duplicate on this new page
- If instance: build the documentation frame directly beside the instance, on its current page, no new page and no duplication
- Update mode: the frame already exists at its current location, don't move it, don't create a new page, edit it in place
- Frame name: `[Component Name] - Spec` (a plain hyphen, not an em dash, matching the no-em-dash rule that applies to this skill's own output too)
- New mode only, before placing: check the target area isn't already occupied by another frame or element. If it is, move the new documentation content (the frame, and the duplicate if one was created) to a clear spot, never the original component or the instance being documented

**Structure, built for both human and AI readability:**
A future skill (or a future Claude session) may need to read this frame back as context without a human walking it through. That requirement drives the structure below, it is not optional formatting:
- One top text layer named `spec/meta` holding plain `Key: Value` lines: `Component: [name]`, `Source: [link to the component node]`, `Documented: [ISO 8601 date, YYYY-MM-DD]`, and, if Step 1 Group 2 determined this covers a single variant rather than the whole set, `Scope: single variant, [variant name]`. Update mode additionally adds `Last updated: [ISO 8601 date]` on top of the original `Documented` line, which stays as the original creation date. This gives any reader, human or AI, a fast anchor without parsing the rest of the frame
- Each of the ten sections is its own named layer or group, using a flat, consistent naming scheme: `spec/definition`, `spec/when-to-use`, `spec/when-not-to-use`, `spec/variants`, `spec/states`, `spec/anatomy`, `spec/properties`, `spec/usage-rules`, `spec/accessibility`, `spec/related-atoms`. Flat naming (`spec/section-name`), not nested subgroups, matching this project's established naming convention
- Section headers are visible, human-readable text (e.g. "When to Use"), the layer name carries the machine-readable key, the visible text carries the human-readable label. Both exist on the same layer, don't split them across two separate nodes
- Match the layout and set of elements shown in `assets/preview-style-reference.html`, exactly, not an approximation. See `references/figma-visual-mapping.md` for the specific Figma construct and DS-token category to use for each visual element (header shading, zebra striping, title accent, badge pills, callout borders, table gridlines), read it before building this frame. A plain-text, thin-gridline result with no shading or accents is a failure of this requirement, not an acceptable simplification of it. Unlike the HTML preview, every text style and fill/border color used to achieve this layout binds to the target file's actual text styles and color variables. If the file has no text styles or color variables yet (no prior Type Scale Generator or Color Scale Generator run), stop and ask the user how to proceed rather than hardcoding a fallback silently, building an unbound frame defeats the point of matching the rest of the design system
- Properties and variants render as actual tables in the frame, not paragraphs, matching the preview
- Every property in the Properties table that is also one of the variant properties listed in the Variants section gets an explicit cross-reference note (e.g. "Same property as Variants: icon"), don't rely on matching names alone to signal this to a reader. Names can be identical, similar, or unrelated between a variant's conceptual role and a property's technical listing, so the link has to be stated, not implied
- If two or more properties share an exact display name but differ in type, keep the shared name on both rows and rely on the Type column plus the cross-reference note, per the Step 2 rule, never invent a suffix to tell them apart
- Update mode: write only the layers whose content actually changed per the Step 2 diff, tagged with the `UPDATED` badge from Step 3, no per-field date. Every other layer on the existing frame is left completely untouched, don't rewrite or re-touch a layer just because the build process passes through it
- Any marker that appears in the approved HTML preview, the `UPDATED` badge or the unverified-accessibility flag, must appear in the Figma build in the same place. A marker present in the approved preview but missing from the build is a build failure, not a minor omission
- Never hardcode a value that Figma can supply directly, if a property list changes on the source component later, that's a stale-doc problem for a future run to catch, not something this skill silently re-derives

**Component description and documentation link (write these on the component node itself, in addition to the frame):**
This is the actual discovery mechanism for a future reader, not the frame's proximity or its name. Use Figma's native `description` and `documentationLinks` fields rather than relying on placement alone:
- Set `description` on the component or component set to a short summary: the one-sentence definition, plus the single most load-bearing usage caution, whichever of "when to use" or "when not to use" is most likely to prevent real misuse for this specific component. A few sentences, not the full spec, Figma's own guidance is explicit that this field can't and shouldn't try to hold everything
- If this spec covers a component set, additionally set a short `description` on each individual variant: one sentence naming what specifically distinguishes that variant from its siblings, when to reach for this exact combination of variant properties over the others. This is not a repeat of the set-level description, it's the variant-specific detail the set-level summary can't carry
- Set `documentationLinks` to a single link pointing at the documentation frame, on the component or component set, and also on each individual variant if variant-level descriptions were set. The API only supports one link per node, don't attempt to add more than one
- Update mode: only rewrite a `description` or `documentationLinks` value if the underlying content actually changed per the Step 2 diff, leave unaffected variants' fields untouched
- Confirm after writing that every field actually reads back correctly via the chosen integration, don't just trust that the write call succeeded

**On failure:** stop immediately, report exactly what was created and what wasn't, ask before retrying or rolling back.

## Guardrails

- New mode and update mode both supported. New mode never overwrites or duplicates a frame that already exists for a component without explicit confirmation. Update mode only ever touches layers whose content actually changed per the Step 2 diff, everything else on the existing frame stays untouched
- Never modify the source component itself beyond its `description` and `documentationLinks` fields, this skill does not touch layers, variants, or properties on the component
- Never invent a variant, state, property, or anatomy name. Everything comes from the actual Figma node data pulled via the chosen integration, or from the user's direct answers in Step 1
- Never invent a numeric suffix or altered name to disambiguate two properties that share an exact display name but differ in type. Keep the real name on both, distinguish via the Type column and the standard cross-reference note
- Never attempt atom-component auto-detection before Step 2, the component hasn't been read yet in Step 1, that data doesn't exist until Step 2 actually pulls it
- The definition, when-to-use, and when-not-to-use fields get a drafted default in Step 2, once real component data exists, never in Step 1 before that data is available. Always present the draft for the user to keep, edit, or replace, never commit it silently
- Never guess at an interactive state that isn't encoded as a Figma variant, ask the user
- Never present unverified accessibility information with the same confidence as verified behavior. Ask which one it is in Step 1, carry the distinction into the output, label unverified content plainly
- Never skip the HTML preview or touch Figma before it's approved
- Never default to a specific Figma integration, always ask in Step 1 before any Figma query
- Never proceed with Figma reads or writes before confirming the Knowledge Work Figma plugin is installed
- Always confirm the target page name and its open state before a Figma read, the MCP is known to fail reading nodes on a page that isn't currently open even when given a direct link
- Never label an interview option "(recommended)," use "(default)"
- Never use an em dash or a double hyphen anywhere in generated output, content sections, the frame, or the HTML preview. This applies to this skill's own naming choices too, frame and page names use plain hyphens
- The frame's layer-naming convention (`spec/section-name`, flat, not nested) is fixed, not a suggestion, it exists so other skills can reliably parse this frame later
- The `description` and `documentationLinks` fields, not the frame's placement or name, are the actual mechanism a future skill uses to find this documentation. Set both on the component and, for a component set, on each variant too, confirm they read back correctly after writing
- `description` stays short, a few sentences, per Figma's own documented guidance, don't try to compress the full spec into it
- The HTML preview's visual style is hardcoded from `assets/preview-style-reference.html` and does not bind to the target file's design system. The Figma frame matches that same layout and set of elements exactly, per `references/figma-visual-mapping.md`, but must bind every text style and color to the target file's actual styles and variables, never hardcoded there. A visually stripped-down Figma result, missing header shading, zebra striping, accents, or badges present in the approved preview, is a failure of this requirement, not an acceptable reading of "as closely as capabilities allow"
- Update mode markers are a plain `UPDATED` badge only, never a per-field date, the frame's `Last updated` line in `spec/meta` is the single source of timing information. Any marker present in the approved HTML preview must also appear in the Figma build, in the same place, a missing marker is a build failure
- On partial build failure, stop and report, don't proceed silently
- Follow the fill-in template in `references/template-and-examples.md` for content structure and tone, don't reinvent the section format per run
