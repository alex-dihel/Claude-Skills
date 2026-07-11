---
name: color-scale-generator
description: Generates a complete color token system for a design system, sourced from Tailwind CSS's official palettes (verified, freshness-checked against Tailwind's live docs each run). Produces Brand (raw palettes) plus 2-tier (Brand + Alias, usage tokens bound directly to Brand) or 3-tier (Brand + Alias + Mapped, Alias as a 6-step semantic scale, Mapped as usage tokens) structure. Always includes system roles (error, warning, success, information, neutral) alongside user-picked brand roles (primary, secondary, optional tertiary). Usage tokens are WCAG 2.1 AA contrast-tested with light/dark support. Builds fresh or adds to an existing file. Ends with a single-pass HTML preview, then a Figma build and specimen frame. Use whenever the user wants color tokens, a color scale, a color system, or color variables for a design system, especially mentioning color scale, color tokens, Tailwind colors, or building color variables in Figma.
---

# Color Scale Generator

## Fixed structure, read this before anything else

The Brand / Alias / Mapped / Responsive collection naming, and the convention of organizing each collection into groups by slash-prefix (`Type/...`, `Scale/...`, `Color/...`), is an established, shared architecture used across multiple skills in this design system, not something specific to this run or open to redesign. If you find yourself about to create a collection or naming pattern that isn't what this file specifies, that is a signal to stop and re-read this file, not a signal that the situation calls for a different structure. This applies with full force whether building fresh or adding to an existing file. Reasoning your way to a different structure because the existing file's current content looks unrelated to color is exactly the mistake this note exists to prevent, an existing `Brand` collection is a shared primitives container by its structure, regardless of what topics its groups currently happen to cover.

## Step 0: Freshness check (always run first, before anything else)

- Fetch Tailwind's official color reference (the OKLCH values, plain fetchable text) and compare against the OKLCH source strings stored in this file's baked-in data table (see bottom of this file)
- This is a plain text comparison, not a conversion. Only compare the OKLCH strings, cheap and exact
- If every family and step matches, and no new Tailwind family exists live that's absent from the table, proceed silently to Step 1
- If anything differs, or a new family exists live that isn't in the table, stop and alert the user with the specific entries that differ or are new. Ask permission to update
- If declined, proceed to Step 1 using the baked values as-is, note to the user that they're proceeding with potentially outdated data
- If approved, do NOT proceed to Step 1. Instead:
  - Fetch the live OKLCH values for only the changed or new entries, not the whole table
  - Convert each using a spec-accurate CSS Color 4 gamut-mapping method (not a naive per-channel clamp, this matters, out-of-gamut Tailwind colors are common and a naive clamp is measurably wrong for a meaningful fraction of the palette)
  - Do not rely on third-party color-chart sites to verify the conversion. They can be stale (some still show Tailwind's old v3 palette) or use inconsistent methods. Spec compliance is the standard, not agreement with a random site
  - Regenerate this SKILL.md file with only the data table section replaced (see delimiters at the bottom), copy every other section of this file byte for byte, unchanged
  - Output the regenerated file, tell the user to reinstall it and restart the process. Stop here, don't continue this run with ad-hoc unbaked values

## Step 1: Setup interview

Ask in small grouped batches, not all at once. Wait for each answer before moving to the next group. Use the platform's native structured selection tool when available, fall back to plain numbered text only if none exists.

**Figma integration (ask first, before any other Figma query):**
- Ask which Figma integration to use: official Figma MCP, or Figma Console MCP / Desktop Bridge plugin. Never default to either without asking
- Confirm it's connected and responding. If not, give specific connection steps, don't fall back to a different one silently
- Use this same integration for every Figma interaction in this run
- To specify the target file, ask for the Figma file link only. Don't offer reading the currently open file as an alternative, that path doesn't work reliably

**Existing file check:**
- Scan the target file for existing Brand, Alias, or Mapped collections
- Found and unambiguous → build inside them. This means inspecting how that collection is actually organized (e.g. groups split by slash-prefix like `Type/...`, `Scale/...`) and adding a new `Color/...` group that follows the exact same convention, inside that same collection. Do not create a separate, differently-named collection (e.g. a new "Color Brand" collection) because the existing one's current contents happen to be a different topic (typography, spacing, etc). A collection organized into prefixed groups is a general-purpose primitives container by its structure, not by whatever topic its groups currently happen to cover. The test is "does a collection with this name already exist," not "does this collection's current content look like it's about color already"
- Found but unclear which is which → ask
- Any existing color tokens specifically → ask whether to delete them or leave them alone. If left alone, prefix the existing names (e.g. `legacy-`) to separate them from the new build and avoid name collisions
- Never modify or delete any pre-existing token, of any kind, color or otherwise, whether created by this run's target file or by another skill entirely (e.g. a prior Type Scale Generator run's `Type` or `Scale` groups). The one exception is color tokens the user explicitly authorized for deletion in this step. Everything else is permanently hands-off

**Mandatory checkpoint, right here, before asking anything else in Step 1:** state back to the user, explicitly, what was found (which collections exist, how they're organized) and exactly how the new color tokens will be added given that. If nothing existing was found, state that this will be a fresh build following the fixed structure above. Either way, this statement must be shown to the user before continuing to the rest of Step 1, not silently decided and revealed only once Figma is actually being written to. This is the point where a wrong structural plan is cheapest to catch, don't let it ride silently through the rest of setup.

**Tier structure:**
- Ask 2-tier or 3-tier
- 2-tier: Alias skips the 6-step semantic scale entirely and instead holds the usage-level tokens directly (what 3-tier calls Mapped), bound straight to Brand steps
- 3-tier: Alias holds the full 6-step semantic scale per role (subtle/muted/default/strong/hover/active), Mapped holds the usage-level tokens, referencing Alias

**Brand roles:**
- Ask for primary, secondary, and optionally tertiary
- For each, ask which Tailwind color family to use. Any family name is acceptable, verify it exists in the baked-in table (or the live check from Step 0) before proceeding
- For each, ask the starting step: default (500), lighter (300-400), darker (600-700), or a specific custom step. Each role's step choice is independent

**Opacity scale (Brand, always included, not user-configurable):** a fixed, general-purpose scale, values 5, 10, 15, 20, 30, 40, 50, 60, 70, 80, 90, 100. This exists for later use when building components (applying transparency to a solid role color), it is not combined with role colors into new persisted tokens, and it is not itself gated on anything, it's a plain utility scale like Brand>Scale in the Type Scale Generator.

**System roles (always generated, five total, never optional):**
- error, warning, success, information, neutral
- For each, offer a constrained list of logically valid Tailwind families, or let the user name their own (verified same as above):
  - error: red, rose, pink
  - warning: amber, orange, yellow
  - success: green, emerald, lime, teal
  - information: blue, sky, cyan, indigo, violet
  - neutral: slate, gray, zinc, neutral, stone
- Same starting-step question as brand roles, asked per role

**Usage-token content (lives in Alias for 2-tier, Mapped for 3-tier):**
- Default pattern, not a rigid fixed list: every group carries the standard system-role set (action, success, warning, error, information) applied consistently, plus items specific to that group's own purpose
- Groups: `text` (headings, body, action, action-hover, disabled, information, success, error, warning, on-action, on-disabled), `icon` (same full set as text: headings, body, action, action-hover, disabled, information, success, error, warning, on-action, on-disabled, icons are used in the same contexts as text and need the same cases, not a smaller set), `surface` (page, primary, success, disabled, disabled-selected, plus the standard system set), `border` (including group-specific items like focus)
- Every surface role gets exactly one token, solid fill only. There is no tinted variant persisted as a token, transparency is a component-level concern using the Opacity scale above, not something this skill builds into the token set
- Ask if the user wants to tweak this default before proceeding

## Step 2: Compute Alias values (3-tier only)

For every role (brand and system), in a 3-tier build only. 2-tier skips this step entirely, its usage tokens are computed directly in Step 4.

- Apply the standard offset mapping from the chosen starting step: subtle (-400), muted (-200), default (0), hover (+100), strong (+200), active (+300)
- Clamp every computed step to Tailwind's real range, 50 to 950. Never generate a step outside that range
- After clamping, check all six values for collisions (two labels landing on the same step). If any exist, alert the user with the specific collision before creating anything, let them choose how to resolve it
- The six-step scale is a default, not a hard ceiling. When a Mapped token needs a value that isn't one of the six (a structural extreme like `page` or `disabled-selected` that was always going to fall outside subtle/muted/default/hover/strong/active by design), add a new, explicitly named Alias step for it rather than having Mapped reach past Alias into Brand directly. Contrast-escalation cases are different, the exact step needed depends on the specific family and can't be pre-named ahead of time, those may legitimately need to bind straight to Brand, but that must always be disclosed, never silent, see the reference column in Step 4's detail table and the build note in Step 5

## Step 3: Build Brand and the semantic Alias scale (3-tier), no gating needed

Brand (raw palettes) and, for 3-tier, the 6-step semantic Alias scale don't involve backgrounds or contrast, so they don't need approval gating. They can be built directly from Step 1 and Step 2's answers once the HTML preview in Step 4 has shown them for a sanity check (not a hard approval requirement, just visibility).

## Step 4: HTML preview

Build a standalone HTML file, not an in-chat-only render. Save it into the project folder (not a temp or scratch location), and if the preview gets revised, overwrite that same saved file in place rather than creating additional copies.

Build everything in a single pass: raw scale swatches, surface, text, icon, and border, all together, one approval. There is no staged sequence here, don't split this into multiple rounds for computational convenience or any other reason.

**Consistency requirement:** every role/case shown in one token section (text, icon, border) must also appear for every other section, using the same set of items throughout. If `text` shows `on-action` and `on-disabled`, `icon` must show the exact same cases too, don't let sections drift into showing different subsets. All borders in the preview render at a forced 1px, regardless of whatever width gets used in the real build, so border color is what's being evaluated, not width.

**Layout requirement:** uniform padding and spacing throughout, no exceptions for any section, including sample components (buttons, containers). Inconsistent spacing driven by variable-length label text is a bug, not a visual detail, fix it by moving labels out of the flow that causes it (see presentation rule below), not by leaving it uneven.

**Presentation:** light and dark go in fully separate containers, never shown together in one shared view. Within each container, keep the actual sample clean: a surface sample shows only the color, nothing overlaid. A text sample shows the text rendered on its surface, nothing else. An icon sample shows the icon on its surface. A border sample shows the border on a clean, unlabeled container. Every sample is clearly labeled with which role it represents, text, icon, and border samples need this as much as surface samples do, don't leave the viewer guessing which case they're looking at.

Identifying text (role name, state) appears in two places: once directly beside its sample, compact, for at-a-glance identification, and again in a full detail list below the whole grid, alongside hex value and contrast ratio. The beside-sample label must not sit below the sample, that's what caused the padding inconsistency in the first place.

**Contrast testing anchor:** never test against pure black (#000000) or pure white (#FFFFFF), those aren't part of the generated color system. Use the darkest and lightest steps of whichever Tailwind family was picked for the neutral role instead (e.g. neutral-950 and neutral-50, or the equivalent steps of whatever family the user chose for neutral).

**Contrast scope, corrected:** WCAG contrast is a relationship, text-against-background, an icon against its background, or a UI component's identifying boundary against what's adjacent to it. A background/surface fill has no contrast requirement on its own. Text and icon get an enforced pass/fail check. Border is shown as informational reference only, ratio and pass/fail state displayed, but never gated, never triggers a question, and never blocks anything. Surface-vs-page contrast is the same, informational only.

Source of truth for what each criterion actually covers: `https://www.w3.org/WAI/WCAG21/Understanding/contrast-minimum.html` (1.4.3, text) and `https://www.w3.org/WAI/WCAG21/Understanding/non-text-contrast.html` (1.4.11, icons and UI component boundaries). If it's ever unclear whether a given token type needs a contrast check, or what ratio applies, fetch and check the relevant document rather than assume. Unlike the Tailwind color data, these documents are a ratified, stable spec, not something that goes stale, so there's no freshness check needed here, just the discipline of checking the source instead of reasoning from assumption when in doubt.

**Build contents:**
- Raw color scale swatches for every selected Brand family and (3-tier) the computed 6-step Alias values per role, labeled with real Tailwind names (e.g. "emerald 400"), not just hex
- The Opacity scale itself, shown once as a plain reference swatch strip (a role color at each of the 12 opacity steps), clearly labeled as illustrative reference only, not a token being built
- Surface tokens: one solid value per role. Solid fill for `primary` and every system role (action, success, warning, error, information) uses that role's starting step from Step 1 directly, unchanged between light and dark mode, no derivation, no mirroring, it's the same value both places. Only `page` uses the same-palette light/dark mirror (e.g. a light page at neutral-50 becomes a dark page at neutral-950), and only `page`, this derivation does not apply to any other surface role. `disabled` and `disabled-selected` keep their own existing light/dark treatment
- Sample buttons and content containers using these surface values
- Illustrative only, not gated, not a persisted token: for each role, one example of its solid surface color shown at a representative Opacity step (e.g. 20), with a solid-color border of the same role, so the user can see roughly how a future tinted component might look. Label this clearly as illustrative, computed live for the preview, not something the skill is building as a token
- Text and icon tokens, tested against their surface (they're used on the same backgrounds). These are the ones with real pass/fail enforcement: if a role's chosen step fails its target ratio (4.5:1 text, 3:1 large text/icons), alert the user with the specific failure, suggest a replacement by stepping through the same palette toward better contrast, and let them confirm or choose something else. Only step on an actual failure, stop at the first passing value, never optimize past a value that already passes. If no step in the palette can ever pass, stop, tell the user, ask for a different palette for that role, never substitute silently. Never auto-apply a change without confirmation
- Border tokens, including the standalone border group (e.g. `focus`), shown with their contrast ratio as reference information, no pass/fail gating, no questions, no auto-correction
- Present the whole thing for approval. Iterate until approved before touching Figma

Do not touch Figma at any point during this step.

**Full detail table, one combined table at the end of the document, not two separate light/dark blocks:**
- One row per token. Light and dark columns sit side by side in the same row, not in separate blocks
- The name column shows the full path exactly as it will exist in Figma (Collection > Group > Token), not just the bare leaf name. This is what catches naming and structure problems before anything gets built, not after
- A reference column for both light and dark showing exactly what that token is bound to: an Alias reference (e.g. `Alias/primary/default`), or, if it genuinely has to bind straight to Brand (an out-of-range contrast escalation, or a structural extreme that isn't one of the six default Alias steps), show that plainly as `→ Brand direct` so it's visible and reviewable, never a silent skip. If a token shows `→ Brand direct` and that wasn't an intentional, disclosed exception, that's the signal something is wrong before you approve it, not after

## Step 5: Build in Figma

Only after the preview is approved.

**Pre-build check:**
- Reconfirm Figma access is still working
- Confirm the target Figma file/page link
- Re-read this SKILL.md in full. Quote the relevant bullet points from the "Existing file check" and "Build" sections verbatim as part of your restated plan, don't paraphrase from memory of earlier in the session. If your planned structure doesn't match what you just quoted, stop and fix the plan before writing anything

**Build:**
- `Brand`: raw Tailwind palettes for only the families actually selected in Step 1 (not the full catalog), all 11 steps each, from the baked-in table (or the freshness-updated values if Step 0 triggered an update)
- `Brand`: the fixed Opacity scale (5, 10, 15, 20, 30, 40, 50, 60, 70, 80, 90, 100), always included
- 3-tier only: `Alias` 6-step semantic scale per role, from Step 2, referencing Brand, plus any additional named steps added for structural extremes per Step 2
- 2-tier: `Alias` holds the usage-level tokens directly, bound to Brand, using the approved values from Step 4
- 3-tier: `Mapped` holds the usage-level tokens, referencing Alias, using the approved values from Step 4. Every Mapped token binds to Alias. If a specific token genuinely needs to bind straight to Brand (a contrast-escalation case that couldn't be pre-named as an Alias step), that must match exactly what was disclosed as `→ Brand direct` in Step 4's detail table and approved there, never introduced for the first time during the actual build
- Every value in every group binds to its source variable. Nothing hardcoded, including in the specimen frame in Step 6

**On failure:** stop immediately, report exactly what was created and what wasn't, ask before retrying or rolling back. Do not proceed to Step 6 with a partial build.

## Step 6: Figma specimen frame

- Create a new frame, apply the actual built tokens (not copies) to labeled sample components, mirroring what was shown in the HTML preview
- Include both light and dark mode sections, clearly labeled

## Guardrails

- Never invent a color name or value. Every Tailwind name and value comes from the verified baked-in table, freshness-checked at the start of every run, never a value pulled from memory outside that table
- Never rely on third-party color-chart sites for verification, they can be stale or methodologically inconsistent. Spec-compliant conversion is the standard
- Never modify or delete any pre-existing token, of any kind, with the sole exception of color tokens explicitly authorized for deletion in Step 1. This applies to tokens from any source, including other skills
- Never create a new collection with a different name when a matching one already exists, just because its current contents look like a different topic. Match the existing collection's actual organizational convention and add to it. This applies with the same force as the no-modification rule above, imposing new structure is its own kind of unwanted change
- Never test contrast against pure black or pure white. Use the darkest and lightest steps of the neutral role's chosen family instead
- Contrast pass/fail enforcement applies only to text and icon tokens. Surface fills and border (including the standalone border group) are never gated, never trigger a question, and are never auto-corrected on contrast, they're shown as informational reference only
- There is no persisted tinted surface token. Transparency is a component-level concern using the Opacity scale, shown in the preview only as an illustrative reference, never built as a color token
- Icon tokens always mirror text's full set of cases, never a reduced subset
- Preview borders always render at a forced 1px, independent of the real build's border width
- Preview layout is uniform throughout, no exceptions for any section. Identifying text sits beside each sample, not below it, plus a full detail table below with the full path, both modes side by side, and what every token is actually bound to
- Every sample in the preview is labeled with the role it represents, not just surface samples
- Build the entire preview in a single pass, one approval, never split into multiple rounds for computational convenience
- Alias is a default six-step scale, not a hard limit. Extend it with explicitly named steps for structural extremes rather than letting Mapped skip a tier into Brand. Contrast-escalation binds straight to Brand only when disclosed in the preview's detail table and approved there, never introduced silently during the build
- Never touch Figma before the preview is approved
- Never hardcode a value anywhere, including preview/specimen elements, everything binds to a variable
- Never auto-apply a contrast-driven color change without the user's confirmation
- The five system roles (error, warning, success, information, neutral) are always generated, never optional, never skipped
- On partial build failure, stop and report, don't proceed to the next step
- When regenerating this file after a freshness update, copy every section except the data table byte for byte. Never rewrite instructions while updating data

---

## Baked-in Tailwind color data

Do not hand-edit this section. It is replaced wholesale during a freshness update (Step 0), and only this section changes when that happens.

<!-- TAILWIND_COLOR_DATA_START -->
<!-- Format: family/step: hex [oklch_source] -->
<!-- Last verified: 2026-07-08, Tailwind CSS v4.3 (26 families, 286 entries) -->
<!-- Do not hand-edit. This block is replaced wholesale during a freshness update. -->

amber: 50=#FFFBEB[oklch(98.7% 0.022 95.277)] 100=#FEF3C6[oklch(96.2% 0.059 95.617)] 200=#FEE685[oklch(92.4% 0.12 95.746)] 300=#FFD237[oklch(87.9% 0.169 91.605)] 400=#FABC00[oklch(82.8% 0.189 84.429)] 500=#F69E00[oklch(76.9% 0.188 70.08)] 600=#DA7700[oklch(66.6% 0.179 58.318)] 700=#B55200[oklch(55.5% 0.163 48.998)] 800=#953D00[oklch(47.3% 0.137 46.201)] 900=#7B3306[oklch(41.4% 0.112 45.904)] 950=#461901[oklch(27.9% 0.077 45.635)]
blue: 50=#EFF6FF[oklch(97.0% 0.014 254.604)] 100=#DBEAFE[oklch(93.2% 0.032 255.585)] 200=#BEDBFF[oklch(88.2% 0.059 254.128)] 300=#91C5FF[oklch(80.9% 0.105 251.813)] 400=#56A2FF[oklch(70.7% 0.165 254.624)] 500=#3280FF[oklch(62.3% 0.214 259.815)] 600=#155DFC[oklch(54.6% 0.245 262.881)] 700=#1447E6[oklch(48.8% 0.243 264.376)] 800=#193CB8[oklch(42.4% 0.199 265.638)] 900=#1C398E[oklch(37.9% 0.146 265.522)] 950=#162456[oklch(28.2% 0.091 267.935)]
cyan: 50=#ECFEFF[oklch(98.4% 0.019 200.873)] 100=#CEFAFE[oklch(95.6% 0.045 203.388)] 200=#A2F4FD[oklch(91.7% 0.08 205.041)] 300=#53EAFD[oklch(86.5% 0.127 207.078)] 400=#00D1EC[oklch(78.9% 0.154 211.53)] 500=#00B6D4[oklch(71.5% 0.143 215.221)] 600=#0091B3[oklch(60.9% 0.126 221.723)] 700=#007491[oklch(52.0% 0.105 223.128)] 800=#005F78[oklch(45.0% 0.085 224.283)] 900=#104E64[oklch(39.8% 0.07 227.392)] 950=#053345[oklch(30.2% 0.056 229.695)]
emerald: 50=#ECFDF5[oklch(97.9% 0.021 166.113)] 100=#D0FAE5[oklch(95.0% 0.052 163.051)] 200=#A4F4CF[oklch(90.5% 0.093 164.15)] 300=#5EE9B5[oklch(84.5% 0.143 164.978)] 400=#00D294[oklch(76.5% 0.177 163.223)] 500=#00B981[oklch(69.6% 0.17 162.48)] 600=#009669[oklch(59.6% 0.145 163.225)] 700=#007857[oklch(50.8% 0.118 165.612)] 800=#005F46[oklch(43.2% 0.095 166.913)] 900=#004E3B[oklch(37.8% 0.077 168.94)] 950=#002C22[oklch(26.2% 0.051 172.552)]
fuchsia: 50=#FDF4FF[oklch(97.7% 0.017 320.058)] 100=#FAE8FF[oklch(95.2% 0.037 318.852)] 200=#F6CFFF[oklch(90.3% 0.076 319.62)] 300=#F2A9FF[oklch(83.3% 0.145 321.434)] 400=#EC6DFF[oklch(74.0% 0.238 322.16)] 500=#E12AFB[oklch(66.7% 0.295 322.15)] 600=#C500DA[oklch(59.1% 0.293 322.896)] 700=#A600B4[oklch(51.8% 0.253 323.949)] 800=#8A0194[oklch(45.2% 0.211 324.591)] 900=#721378[oklch(40.1% 0.17 325.612)] 950=#4B004F[oklch(29.3% 0.136 325.661)]
gray: 50=#F9FAFB[oklch(98.5% 0.002 247.839)] 100=#F3F4F6[oklch(96.7% 0.003 264.542)] 200=#E5E7EB[oklch(92.8% 0.006 264.531)] 300=#D1D5DC[oklch(87.2% 0.01 258.338)] 400=#99A1AF[oklch(70.7% 0.022 261.325)] 500=#6A7282[oklch(55.1% 0.027 264.364)] 600=#4A5565[oklch(44.6% 0.03 256.802)] 700=#364153[oklch(37.3% 0.034 259.733)] 800=#1E2939[oklch(27.8% 0.033 256.848)] 900=#101828[oklch(21.0% 0.034 264.665)] 950=#030712[oklch(13.0% 0.028 261.692)]
green: 50=#F0FDF4[oklch(98.2% 0.018 155.826)] 100=#DCFCE7[oklch(96.2% 0.044 156.743)] 200=#B9F8CF[oklch(92.5% 0.084 155.995)] 300=#7BF1A8[oklch(87.1% 0.15 154.449)] 400=#05DF72[oklch(79.2% 0.209 151.711)] 500=#00C65A[oklch(72.3% 0.219 149.579)] 600=#00A447[oklch(62.7% 0.194 149.214)] 700=#00813A[oklch(52.7% 0.154 150.069)] 800=#016630[oklch(44.8% 0.119 151.328)] 900=#0D542B[oklch(39.3% 0.095 152.535)] 950=#032E15[oklch(26.6% 0.065 152.934)]
indigo: 50=#EEF2FF[oklch(96.2% 0.018 272.314)] 100=#E0E7FF[oklch(93.0% 0.034 272.788)] 200=#C7D2FF[oklch(87.0% 0.065 274.039)] 300=#A4B4FF[oklch(78.5% 0.115 274.713)] 400=#7D87FF[oklch(67.3% 0.182 276.935)] 500=#6260FF[oklch(58.5% 0.233 277.117)] 600=#4F39F6[oklch(51.1% 0.262 276.966)] 700=#432DD7[oklch(45.7% 0.24 277.023)] 800=#372AAC[oklch(39.8% 0.195 277.366)] 900=#312C85[oklch(35.9% 0.144 278.697)] 950=#1E1A4D[oklch(25.7% 0.09 281.288)]
lime: 50=#F7FEE7[oklch(98.6% 0.031 120.757)] 100=#ECFCCA[oklch(96.7% 0.067 122.328)] 200=#D8F999[oklch(93.8% 0.127 124.321)] 300=#BBF451[oklch(89.7% 0.196 126.665)] 400=#9DE500[oklch(84.1% 0.238 128.85)] 500=#83CC00[oklch(76.8% 0.233 130.85)] 600=#64A300[oklch(64.8% 0.2 131.684)] 700=#4B7C00[oklch(53.2% 0.157 131.589)] 800=#3D6300[oklch(45.3% 0.124 130.933)] 900=#35530E[oklch(40.5% 0.101 131.063)] 950=#192E03[oklch(27.4% 0.072 132.109)]
mauve: 50=#FAFAFA[oklch(98.5% 0.0 0.0)] 100=#F3F1F3[oklch(96.0% 0.003 325.6)] 200=#E7E4E7[oklch(92.2% 0.005 325.62)] 300=#D7D0D7[oklch(86.5% 0.012 325.68)] 400=#A89EA9[oklch(71.1% 0.019 323.02)] 500=#79697B[oklch(54.2% 0.034 322.5)] 600=#594C5B[oklch(43.5% 0.029 321.78)] 700=#463947[oklch(36.4% 0.029 323.89)] 800=#2A212C[oklch(26.3% 0.024 320.12)] 900=#1D161E[oklch(21.2% 0.019 322.12)] 950=#0C090C[oklch(14.5% 0.008 326.0)]
mist: 50=#F9FBFB[oklch(98.7% 0.002 197.1)] 100=#F1F3F3[oklch(96.3% 0.002 197.1)] 200=#E3E7E8[oklch(92.5% 0.005 214.3)] 300=#D0D6D8[oklch(87.2% 0.007 219.6)] 400=#9CA8AB[oklch(72.3% 0.014 214.4)] 500=#67787C[oklch(56.0% 0.021 213.5)] 600=#4B585B[oklch(45.0% 0.017 213.2)] 700=#394447[oklch(37.8% 0.015 216.0)] 800=#22292B[oklch(27.5% 0.011 216.9)] 900=#161B1D[oklch(21.8% 0.008 223.9)] 950=#090B0C[oklch(14.8% 0.004 228.8)]
neutral: 50=#FAFAFA[oklch(98.5% 0.0 0.0)] 100=#F5F5F5[oklch(97.0% 0.0 0.0)] 200=#E5E5E5[oklch(92.2% 0.0 0.0)] 300=#D4D4D4[oklch(87.0% 0.0 0.0)] 400=#A1A1A1[oklch(70.8% 0.0 0.0)] 500=#737373[oklch(55.6% 0.0 0.0)] 600=#525252[oklch(43.9% 0.0 0.0)] 700=#404040[oklch(37.1% 0.0 0.0)] 800=#262626[oklch(26.9% 0.0 0.0)] 900=#171717[oklch(20.5% 0.0 0.0)] 950=#0A0A0A[oklch(14.5% 0.0 0.0)]
olive: 50=#FBFBF9[oklch(98.8% 0.003 106.5)] 100=#F4F4F0[oklch(96.6% 0.005 106.5)] 200=#E8E8E3[oklch(93.0% 0.007 106.5)] 300=#D8D8D0[oklch(88.0% 0.011 106.6)] 400=#ABAB9C[oklch(73.7% 0.021 106.9)] 500=#7C7C67[oklch(58.0% 0.031 107.3)] 600=#5B5B4B[oklch(46.6% 0.025 107.3)] 700=#474739[oklch(39.4% 0.023 107.4)] 800=#2B2B22[oklch(28.6% 0.016 107.4)] 900=#1D1D16[oklch(22.8% 0.013 107.4)] 950=#0C0C09[oklch(15.3% 0.006 107.1)]
orange: 50=#FFF7ED[oklch(98.0% 0.016 73.684)] 100=#FFEDD5[oklch(95.4% 0.038 75.164)] 200=#FFD7A8[oklch(90.1% 0.076 70.697)] 300=#FFB970[oklch(83.7% 0.128 66.29)] 400=#FF8B1F[oklch(75.0% 0.183 55.934)] 500=#FC7100[oklch(70.5% 0.213 47.604)] 600=#EC5600[oklch(64.6% 0.222 41.116)] 700=#C43E00[oklch(55.3% 0.195 38.402)] 800=#9F2D00[oklch(47.0% 0.157 37.304)] 900=#7E2A0C[oklch(40.8% 0.123 38.172)] 950=#441306[oklch(26.6% 0.079 36.259)]
pink: 50=#FDF2F8[oklch(97.1% 0.014 343.198)] 100=#FCE7F3[oklch(94.8% 0.028 342.258)] 200=#FCCEE8[oklch(89.9% 0.061 343.231)] 300=#FDA5D5[oklch(82.3% 0.12 346.018)] 400=#FB64B6[oklch(71.8% 0.202 349.761)] 500=#F6339A[oklch(65.6% 0.241 354.308)] 600=#E30076[oklch(59.2% 0.249 0.584)] 700=#C2005C[oklch(52.5% 0.223 3.958)] 800=#A2004C[oklch(45.9% 0.187 3.815)] 900=#861043[oklch(40.8% 0.153 2.432)] 950=#510424[oklch(28.4% 0.109 3.907)]
purple: 50=#FAF5FF[oklch(97.7% 0.014 308.299)] 100=#F3E8FF[oklch(94.6% 0.033 307.174)] 200=#E9D5FF[oklch(90.2% 0.063 306.703)] 300=#D8B4FF[oklch(82.7% 0.119 306.383)] 400=#BF7EFF[oklch(71.4% 0.203 305.504)] 500=#AB4EFF[oklch(62.7% 0.265 303.9)] 600=#9810FA[oklch(55.8% 0.288 302.321)] 700=#8200D9[oklch(49.6% 0.265 301.924)] 800=#6E11B0[oklch(43.8% 0.218 303.724)] 900=#59168B[oklch(38.1% 0.176 304.987)] 950=#3C0366[oklch(29.1% 0.149 302.717)]
red: 50=#FEF2F2[oklch(97.1% 0.013 17.38)] 100=#FFE2E2[oklch(93.6% 0.032 17.717)] 200=#FFCACA[oklch(88.5% 0.062 18.334)] 300=#FFA3A4[oklch(80.8% 0.114 19.571)] 400=#FF6568[oklch(70.4% 0.191 22.216)] 500=#FB2C36[oklch(63.7% 0.237 25.331)] 600=#E40016[oklch(57.7% 0.245 27.325)] 700=#BF000F[oklch(50.5% 0.213 27.518)] 800=#9F0712[oklch(44.4% 0.177 26.899)] 900=#82181A[oklch(39.6% 0.141 25.723)] 950=#460809[oklch(25.8% 0.092 26.042)]
rose: 50=#FFF1F2[oklch(96.9% 0.015 12.422)] 100=#FFE4E6[oklch(94.1% 0.03 12.58)] 200=#FFCCD3[oklch(89.2% 0.058 10.001)] 300=#FFA3AE[oklch(81.0% 0.117 11.638)] 400=#FF6880[oklch(71.2% 0.194 13.428)] 500=#FF2357[oklch(64.5% 0.246 16.439)] 600=#E60045[oklch(58.6% 0.253 17.585)] 700=#C1003A[oklch(51.4% 0.222 16.935)] 800=#A30037[oklch(45.5% 0.188 13.697)] 900=#8B0836[oklch(41.0% 0.159 10.272)] 950=#4D0218[oklch(27.1% 0.105 12.094)]
sky: 50=#F0F9FF[oklch(97.7% 0.013 236.62)] 100=#DFF2FE[oklch(95.1% 0.026 236.824)] 200=#B8E6FE[oklch(90.1% 0.058 230.902)] 300=#78D4FF[oklch(82.8% 0.111 230.318)] 400=#00BBFD[oklch(74.6% 0.16 232.661)] 500=#00A5EA[oklch(68.5% 0.169 237.323)] 600=#0084C7[oklch(58.8% 0.158 241.966)] 700=#0069A2[oklch(50.0% 0.134 242.749)] 800=#005986[oklch(44.3% 0.11 240.79)] 900=#024A70[oklch(39.1% 0.09 240.876)] 950=#052F4A[oklch(29.3% 0.066 243.157)]
slate: 50=#F8FAFC[oklch(98.4% 0.003 247.858)] 100=#F1F5F9[oklch(96.8% 0.007 247.896)] 200=#E2E8F0[oklch(92.9% 0.013 255.508)] 300=#CAD5E2[oklch(86.9% 0.022 252.894)] 400=#90A1B9[oklch(70.4% 0.04 256.788)] 500=#62748E[oklch(55.4% 0.046 257.417)] 600=#45556C[oklch(44.6% 0.043 257.281)] 700=#314158[oklch(37.2% 0.044 257.287)] 800=#1D293D[oklch(27.9% 0.041 260.031)] 900=#0F172B[oklch(20.8% 0.042 265.755)] 950=#020618[oklch(12.9% 0.042 264.695)]
stone: 50=#FAFAF9[oklch(98.5% 0.001 106.423)] 100=#F5F5F4[oklch(97.0% 0.001 106.424)] 200=#E7E5E4[oklch(92.3% 0.003 48.717)] 300=#D6D3D1[oklch(86.9% 0.005 56.366)] 400=#A6A09B[oklch(70.9% 0.01 56.259)] 500=#79716B[oklch(55.3% 0.013 58.071)] 600=#57534D[oklch(44.4% 0.011 73.639)] 700=#44403B[oklch(37.4% 0.01 67.558)] 800=#292524[oklch(26.8% 0.007 34.298)] 900=#1C1917[oklch(21.6% 0.006 56.043)] 950=#0C0A09[oklch(14.7% 0.004 49.25)]
taupe: 50=#FBFAF9[oklch(98.6% 0.002 67.8)] 100=#F3F1F1[oklch(96.0% 0.002 17.2)] 200=#E8E4E3[oklch(92.2% 0.005 34.3)] 300=#D8D2D0[oklch(86.8% 0.007 39.5)] 400=#ABA09C[oklch(71.4% 0.014 41.2)] 500=#7C6D67[oklch(54.7% 0.021 43.1)] 600=#5B4F4B[oklch(43.8% 0.017 39.3)] 700=#473C39[oklch(36.7% 0.016 35.7)] 800=#2B2422[oklch(26.8% 0.011 36.5)] 900=#1D1816[oklch(21.4% 0.009 43.1)] 950=#0C0A09[oklch(14.7% 0.004 49.3)]
teal: 50=#F0FDFA[oklch(98.4% 0.014 180.72)] 100=#CBFBF1[oklch(95.3% 0.051 180.801)] 200=#96F7E4[oklch(91.0% 0.096 180.426)] 300=#46ECD5[oklch(85.5% 0.138 181.071)] 400=#00D3BD[oklch(77.7% 0.152 181.912)] 500=#00B9A6[oklch(70.4% 0.14 182.503)] 600=#009488[oklch(60.0% 0.118 184.704)] 700=#00776E[oklch(51.1% 0.096 186.391)] 800=#005F5A[oklch(43.7% 0.078 188.216)] 900=#0B4F4A[oklch(38.6% 0.063 188.416)] 950=#022F2E[oklch(27.7% 0.046 192.524)]
violet: 50=#F5F3FF[oklch(96.9% 0.016 293.756)] 100=#EDE9FE[oklch(94.3% 0.029 294.588)] 200=#DDD6FF[oklch(89.4% 0.057 293.283)] 300=#C4B4FF[oklch(81.1% 0.111 293.571)] 400=#A686FF[oklch(70.2% 0.183 293.541)] 500=#8D56FF[oklch(60.6% 0.25 292.717)] 600=#7F22FE[oklch(54.1% 0.281 293.009)] 700=#7008E7[oklch(49.1% 0.27 292.581)] 800=#5D0EC0[oklch(43.2% 0.232 292.759)] 900=#4D179A[oklch(38.0% 0.189 293.745)] 950=#2F0D68[oklch(28.3% 0.141 291.089)]
yellow: 50=#FEFCE8[oklch(98.7% 0.026 102.212)] 100=#FEF9C2[oklch(97.3% 0.071 103.193)] 200=#FFF085[oklch(94.5% 0.129 101.54)] 300=#FFE030[oklch(90.5% 0.182 98.111)] 400=#F7C900[oklch(85.2% 0.199 91.936)] 500=#EAB300[oklch(79.5% 0.184 86.047)] 600=#CA8A00[oklch(68.1% 0.162 75.834)] 700=#A26200[oklch(55.4% 0.135 66.442)] 800=#874C00[oklch(47.6% 0.114 61.907)] 900=#733E0A[oklch(42.1% 0.095 57.708)] 950=#432004[oklch(28.6% 0.066 53.813)]
zinc: 50=#FAFAFA[oklch(98.5% 0.0 0.0)] 100=#F4F4F5[oklch(96.7% 0.001 286.375)] 200=#E4E4E7[oklch(92.0% 0.004 286.32)] 300=#D4D4D8[oklch(87.1% 0.006 286.286)] 400=#9F9FA9[oklch(70.5% 0.015 286.067)] 500=#71717B[oklch(55.2% 0.016 285.938)] 600=#52525C[oklch(44.2% 0.017 285.786)] 700=#3F3F46[oklch(37.0% 0.013 285.805)] 800=#27272A[oklch(27.4% 0.006 286.033)] 900=#18181B[oklch(21.0% 0.006 285.885)] 950=#09090B[oklch(14.1% 0.005 285.823)]

<!-- TAILWIND_COLOR_DATA_END -->
