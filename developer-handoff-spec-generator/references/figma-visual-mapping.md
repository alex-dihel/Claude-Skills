# Figma Visual Mapping

Read this before building the frame in Step 5. `assets/dev-handoff-style-reference.html` sets the visual bar.

## The key difference from Component Spec Generator's approach

This skill's chrome, the card border, section header style, divider color, badge palette, check/x icon colors, is a **fixed preset**, not derived from the target file's own design tokens. Every Developer Handoff frame looks the same regardless of which file or brand it documents. This is a deliberate divergence from the DS-token-bound chrome approach used elsewhere in this project, don't pull the target file's primary/accent color into headers or badges here.

**This applies only to chrome, never to content.** Every actual value shown in a table (a sizing number, a token name, a color hex, a state name) is still real data read from the target file, sourced per `sourcing-check.md`, never invented. Only the document's own visual scaffolding is fixed.

| Visual element | Figma construct | Treatment |
|---|---|---|
| Document title, meta lines | Text nodes, heading + small body text style | Use the target file's own heading/body text styles if present (typography still reads naturally in-file), but text color for meta lines is the fixed muted gray from the preset, not a token pulled from the target |
| Divider between sections | 1px line/rectangle | Fixed light gray (`#e4e4e7`), not a bound border token |
| Section headers | Text node, semibold, no colored underline or accent bar | Fixed dark neutral (`#18181b`), same weight/size for every section, no per-file color variation |
| Table header row | Filled rectangle behind header cells | Fixed light gray (`#fafafa`), fixed muted-gray uppercase small text (`#71717a`), same for every table regardless of file |
| Table gridlines | 1px borders | Fixed light gray (`#e4e4e7`) |
| Badge, `(Default)` | Small rounded rectangle, outline style | Fixed neutral outline (border `#d4d4d8`, text `#71717a`, transparent fill), never the target's semantic/warning tokens |
| Badge, `UPDATED` | Small rounded rectangle, filled | Fixed light blue (`#eff6ff` fill, `#1d4ed8` text, `#bfdbfe` border), same every time |
| Check / X marks | Inline glyph or small icon | Fixed green (`#16a34a`) for check, fixed red (`#dc2626`) for X. Use for genuinely boolean cells only (a field is required or not, a value is token-bound or not), not decoratively |
| Callout notes | Bordered box, no colored left accent bar | Fixed light gray border and background (`#e4e4e7` / `#fafafa`), plain, no accent color |

## If the target file has no text styles at all

Fall back to a plain system sans-serif at the sizes shown in the reference asset. This is the one case where falling back on the document's own chrome typography is fine even though it isn't a bound style, since there's nothing in the file to bind to and the alternative is inventing a fake text style.

## What never changes regardless of preset

Every content value (sizes, token names, hex values, state names, accessible names) still comes from real Figma data or a disclosed default, per `sourcing-check.md` and `accessibility-defaults.md`. The preset governs how the document looks, not what it says.
