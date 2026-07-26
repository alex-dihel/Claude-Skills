# Annotation Schema (load whenever the interview reaches Accessibility or Behavior for an interactive target)

This is the content structure for Accessibility and Behavior, not a generic prose prompt. Ask only the categories that actually apply to the target's real role, determined from its Figma structure (a "With Label" input instance triggers the Input category, a "Buttons" instance triggers Button, etc.), not every category for every target.

Run the Sourcing Check (`sourcing-check.md`) against each field below before asking it. Most of these are exactly the kind of thing that's already a Figma property, a Code Connect binding, or covered by an existing atom doc, ask only what's left.

## Classification rule (applies before any category below)

Classify by behavior, not by which Figma component was used to build it. If it navigates to a different page or location, it's a link, even if built from a button component. If it triggers an in-place action without navigating, it's a button, even if built from a link component. State the classification explicitly in Behavior, don't leave it implied by the component name.

## Button

- **Type**: submit, download (name the specific file), or open-modal (name the specific modal). Don't leave "type" generic, the specific target matters to a developer
- **Accessible name**: the name a screen reader announces
- **Accessible description**: additional context beyond the name, only if the name alone is ambiguous

## Link

- **Target**: destination page plus specific anchor/section if it's an in-page jump, not just "goes to page X"
- **Accessible name**: as above
- **Accessible description**: as above, only if needed

## Heading

- **Semantic level**: the programmatic heading level (h1-h6), which can diverge from the visual text style. A visually-styled H3 may need to be programmatic H2 to keep screen-reader heading order linear. Always state both the visual style and the semantic level if they differ, never assume they match
- **Description**: only if the heading text alone is ambiguous out of context

## Image / Icon

- **Element type**: image or SVG. SVGs used as pure decoration don't need an accessible name at all, state this explicitly rather than leaving it unaddressed
- **Accessible name**: alt text, only for informative images/icons, not decorative ones

## Input

- **Type**: the HTML input type (text, email, number, etc.), check WHATWG HTML Standard via `accessibility-defaults.md` if not otherwise specified
- **Required**: true/false
- **Autocomplete**: the autocomplete attribute value, if applicable
- **Inputmode**: the mobile keyboard type this should trigger
- **Min/max length**, if applicable
- **Disabled-state trigger logic**: always required per the standing rule, never leave "disabled" unexplained. State the exact condition that enables the field

## Landmark (target-level, not per-child)

- **Role**: section, main, form, footer, nav, etc., whichever applies to the target as a whole
- **Name**: the accessible name for that landmark region

This is the "always documented, widget-level" content from `composed-target-handling.md`. It applies once, to the target itself, regardless of what its children need individually.

## Source

CVS Health Web Accessibility Annotation Kit (Figma Community), as covered in the second reviewed transcript, cross-validated by GitHub's own rebuild of the same kit into Primer A11y Presets ("Design system annotations," parts 1 and 2, github.blog).
