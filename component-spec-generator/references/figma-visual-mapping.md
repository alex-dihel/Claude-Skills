# Figma Visual Mapping

Read this before building the frame in Step 5. `assets/preview-style-reference.html` sets the visual bar, this file is how to hit that same bar in Figma without hardcoding a single value. Every row below names the visual element, the Figma construct that realizes it, and the category of design-system token it binds to. If the target file's actual token names differ from the examples, bind to the real equivalent, don't invent a new token and don't fall back to a hardcoded value.

| Visual element in the reference | Figma construct | DS token category to bind |
|---|---|---|
| Document title ("Button") | Text node, large/heading text style | The file's largest available heading text style (e.g. `Type/heading/h1` if a Type Scale Generator run exists) |
| Meta block (Source, Documented, Scope lines) | Text node(s) below the title, smaller text style | A body or caption text style, muted/secondary text color token, not the default body color |
| Divider line under the meta block | A 1px line or rectangle | A border/divider color token, low-emphasis, not the primary accent |
| Section headers ("When to Use," "Properties," etc.) | Text node, mid-size heading text style, with a colored underline (rectangle or bottom border) directly beneath | Heading text style for the text; an accent color token (primary brand color, or its darker alias step) for the underline, matching the reference's colored section-header treatment |
| Table header row shading | A filled rectangle behind the header row, or the table header cells' own fill | A subtle surface or tint token derived from the primary/accent role (e.g. an alias step like `primary/subtle`, not the full-strength brand color), never a literal hex |
| Table header text | Text node, semibold or medium weight | Same accent color token used for section-header underlines, for visual consistency between headers and header rows |
| Table zebra striping (alternate row tint) | Alternating row fills | A very light neutral or surface token, distinctly lighter than the header shading, usually the neutral role's lightest or near-lightest step |
| Table gridlines | 1px borders between cells | A border color token, the file's standard default border, not the accent color |
| Callout notes (cross-reference and naming-coincidence notes) | A rectangle with a colored left border (3 to 4px) plus a tinted background fill, text inside | Left border binds to the accent token; background fill binds to the same accent's lightest/subtle alias step; text binds to a muted text color token |
| Badge pills (`UPDATED`, `Unverified`) | A small rounded rectangle (pill shape) with text inside | Background fill and text color bind to the semantic role matching the badge's meaning, `UPDATED` to an information or neutral-accent token, `Unverified` to a warning/caution token, using that role's on-color text token for the text, not a fixed hex pairing |
| Two-column Do/Don't layout | Two auto-layout frames side by side, each with its own heading ("Do," "Don't") and bulleted text | Headings use the same heading text style as other sections; body text uses the file's standard body text style; no special color treatment needed beyond standard text tokens |
| Property/variant tables | Standard table structure matching the tables above | Same header-shading, zebra-striping, and gridline tokens as every other table in the frame, consistency across all tables matters as much as matching the reference once |

## What "exact" means here

The reference asset uses a single hardcoded accent color throughout, that specific hex value is not the target, the target file's actual accent/primary token is. If two projects use this skill with different brand colors, the resulting frames should look structurally identical to each other (same shading pattern, same badge shapes, same underline treatment) while differing in the specific color, because each is correctly bound to its own file's tokens. A frame that skips the shading, striping, or badges entirely because "the file's colors are different" has misunderstood the requirement, the requirement is about the DS binding, not about reproducing this specific palette.

## If a needed token category doesn't exist in the target file

Don't invent one and don't hardcode a substitute. Stop and ask the user how they'd like to proceed, the same rule that already applies elsewhere in this skill when the file has no text styles or color variables at all.
