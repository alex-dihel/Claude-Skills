---
name: type-scale-generator
description: Generates a brand-new typography type scale for a design system, from initial setup through an HTML preview file for approval, then Figma variables and text styles built via Figma MCP, ending with a specimen frame in Figma for final visual QA. Supports 2 or 3 responsive modes (Desktop+Mobile, or +Tablet). Restricted to Google Fonts only. All values are px, sourced from a shared Brand>Scale token set, never rem or pt. This skill is for NEW type scale creation only, it does not update or modify an existing one. Use this skill whenever the user wants to set up type styles, a typography scale, heading and body text styles, or type variables for a new design system, new project, or new client, especially when they mention "type scale," "typography setup," "text styles," "heading styles," or building type variables in Figma from scratch.
---

# Type Scale Generator

## Mandatory first response
State: "This skill creates a new type scale from scratch. It does not update, extend, or modify an existing type scale or design system. If you're looking to update an existing one, this isn't the right tool, say so and stop here." Wait for confirmation before proceeding.

## Step 1: Setup interview

Ask in small grouped batches, not all at once. Wait for each answer before moving to the next group. Suggested grouping: (1) base size and typefaces, (2) heading/body structure and responsive modes, (3) weights, (4) scale ratio, (5) Figma integration.

When the current platform provides a structured selection tool (e.g. single-select or multi-select options, not just free text), use it for each question instead of writing the choices out as plain text. Fall back to plain numbered text only if no such tool is available.

**Base inputs:**
- Base font size (px)
- Typefaces: Google Fonts only, one for heading, one for body. Ask for a font name or Google Fonts URL for each
- Heading levels: H1-H6 by default, ask if an optional "Hero" level above H1 is needed
- Body sizes: sm/md/lg by default, confirm
- Responsive modes: Desktop + Mobile by default, ask if Tablet should be added as a third mode

**Figma integration:**
- Before any Figma query or write, ask which Figma integration to use: official Figma MCP, or Figma Console MCP / Desktop Bridge plugin. Do not default to either without asking, both are valid depending on what the user has set up
- Confirm the chosen integration is connected and responding. If not, give the user the specific steps to connect that integration, do not silently fall back to a different one or proceed without it
- Use this same integration for every subsequent Figma interaction in this run, including font verification below

**Font verification:**
- Do not assume a requested Google Font is available. Check via the chosen Figma integration before any build step
- If not found, tell the user directly and ask for an alternative, never substitute silently

**Weight variants:**
- Default: heading uses semibold, body uses regular
- Ask per role (heading, body) whether a different weight or additional variants are wanted, let the user specify freely, not limited to a fixed list
- Whatever is chosen applies uniformly across all sizes/levels within that role, no per-size weight variation
- These become variables under Brand>Type>font-weight, not raw hardcoded values. They do not reference Brand>Scale, weight isn't a spacing/sizing value, it's a separate concept entirely
- Each font-weight variable must be a string holding the exact Figma-installed style name for that font (e.g. `Regular`, `SemiBold`, `Bold`, `Medium`), never a number. A numeric value cannot bind to Figma's font-style property, only the string name can

**Scale ratio (fontsize):**
- Named modular ratio (Minor Second 1.067, Major Second 1.125, Minor Third 1.2, Major Third 1.25, Perfect Fourth 1.333, Augmented Fourth 1.414, Perfect Fifth 1.5, Golden Ratio 1.618), or "best-practice auto" based on base size and use case (tighter ratios for UI-dense products, looser for editorial/marketing). State which one and why if auto

**Lineheight (fixed rule, not a setup question):**
- Less than or equal to 16px: 1.5x font-size
- 17-24px: 1.4x
- 25-36px: 1.3x
- 37-48px: 1.2x
- Greater than 48px: 1.1x

**Snapping to Scale:**
- Compute raw fontsize values from the chosen ratio
- Compute lineheight values via the bracket rule above
- Round every value (fontsize and lineheight) to the nearest existing Brand>Scale step (0, 1, 2, 4, 6, 8, 12, 14, 16, 18, 20, 24, 28, 32, 40, 48, 56, 64, 96, 128, 192, 256), not nearest multiple of 4
- If a raw value is exactly equidistant between two Scale steps, round up to the larger step
- Show a table of raw vs. snapped values, flag any step where snapping shifted the value by more than 2px

**Breakpoint scaling:**
- Body fontsize and lineheight values: identical across every responsive mode, no reduction
- Heading fontsize and lineheight values: each mode below Desktop shifts one full step down the fixed heading sequence (Hero, H1, H2, H3, H4, H5, H6) relative to the mode directly above it. Desktop uses the values computed and snapped above. If Tablet is included, Tablet's value at each level equals Desktop's value at the next level down. Mobile's value at each level equals the value directly above it (Tablet's if Tablet exists, otherwise Desktop's), one further level down
- H6 has no lower level to draw from. Once a mode's shift would go past H6, that mode uses H6's own value and does not shift further

## Step 2: HTML preview (external file, before touching Figma)
- Build a standalone HTML file with real sample sentences at every level, not lorem ipsum
- Match font family, size, weight, line height to what will be built, shown for each responsive mode side by side
- Save it as an actual file the user can open and download, do not rely on an in-chat-only render
- Do not touch Figma during this step
- Present for approval, iterate until approved

## Step 3: Build Figma variables (only after preview approval)

**Pre-build check:**
- Reconfirm the Figma integration chosen in Step 1 is still connected and responding. Do not ask which integration to use again, that was already decided
- Confirm the target Figma file/page link
- Scan the target file for an existing Brand collection with a Scale or Type group, or an existing Responsive collection. If found, stop and ask the user how to proceed. Do not create alongside or overwrite silently

**Build, collection and group names exactly as follows:**
- `Brand` collection > `Type` group, two sub-groups:
  - `type-face`: `heading`, `body`, values are the Google Font family name
  - `font-weight`: one variable per role (and per extra variant if configured, e.g. `heading`, `body`, `body-bold`), values are the exact Figma-installed style name as a string. Never a number. Not scale-referenced, weight isn't a spacing/sizing concept
- `Brand` collection > `Scale` group: full general-purpose numeric scale, named `0`, `25 (1)`, `50 (2)`, `100 (4)`, `150 (6)`, `200 (8)`, `300 (12)`, `350 (14)`, `400 (16)`, `450 (18)`, `500 (20)`, `600 (24)`, `700 (28)`, `800 (32)`, `900 (40)`, `1000 (48)`, `1100 (56)`, `1200 (64)`, `1300 (96)`, `1400 (128)`, `1500 (192)`, `1600 (256)`, values matching the number in parentheses
- `Responsive` collection > `fontsize` group > `body` (sm/md/lg) and `heading` (hero if selected, h1-h6): each variable's value per mode is an alias reference to the matching Brand>Scale step, not an independent number
- `Responsive` collection > `lineheight` group > `body` and `heading`, same structure and referencing rule
- Only Brand and Responsive collections are touched by this skill. Do not create or write to Alias or Mapped

**On failure:** stop immediately, report exactly what was created and what wasn't, ask before retrying or rolling back.

## Step 4: Build Figma text styles
- One text style per role and level (e.g. `heading/h1`, `body/sm`), plus one additional style per configured extra weight variant (e.g. `body-semibold`, applied uniformly, not per size)
- font-family binds to Brand>Type>type-face>[role], font-weight/style binds to Brand>Type>font-weight>[role or variant], font-size binds to the matching Responsive>fontsize variable, line-height binds to the matching Responsive>lineheight variable
- Never hardcode any of these four properties, every one binds to a variable
- Do not multiply styles per breakpoint, mode switching is handled by the variables themselves
- On failure: same rule as Step 3

## Step 5: Figma specimen frame
- Create a new frame, apply each actual text style (not a copy) to labeled sample text
- Include a labeled section per responsive mode (Desktop, Mobile, and Tablet if selected), each with that mode applied so resolved values differ correctly between sections

## Guardrails
- New type scale creation only, never modify an existing one
- Google Fonts only, verify before building, never substitute silently
- Never skip the HTML preview or touch Figma before it's approved
- Always confirm the exact Figma file/frame link before pushing anything
- Never silently snap a ratio scale without showing what changed
- Never use rem, pt, or percent-based values, everything is px via Brand>Scale
- Never touch Alias or Mapped collections
- Never build over an existing conflicting Brand or Responsive collection without asking
- Never default to a specific Figma integration (official MCP vs. Console MCP/Desktop Bridge). Always ask which one to use in Step 1, before any Figma query, including font verification
- Never hardcode font-family, font-weight/style, fontsize, or lineheight. Every text style property binds to a variable
- Font-weight variables must be string type, holding the exact Figma-installed style name. Never bind a numeric weight value to the style property, it will not work
- On partial build failure, stop and report, don't proceed to the next step
