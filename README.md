# Claude Skills

A growing library of custom Claude skills, built for real work and offered free to the community.

Each skill is a structured prompt that instructs Claude to perform a specific task consistently and reliably. Skills are installed once and invoked on demand - no setup required per conversation.

---

## Available Skills

| Skill | Description |
|---|---|
| [color-scale-generator](./color-scale-generator/) | Generates a complete color token system for a design system, sourced from Tailwind CSS's official palettes, from a WCAG-tested HTML preview through real Figma variables and a specimen frame. |
| [component-spec-generator](./component-spec-generator/) | Documents an existing Figma component, reading its variants, states, and properties directly from Figma, filling in the rest through a short interview, previewing it as a styled HTML document, then writing it into Figma as a documentation frame beside the component. |
| [critical-thinking](./critical-thinking/) | Applies the FORCE anti-sycophancy framework to evaluate proposals, decisions, and artifacts. Surfaces real flaws, assigns severity, and delivers a clear verdict. |
| [cro-audit-generator](./cro-audit-generator/) | Runs a conversion rate optimization (CRO) audit on a single page or a multi-step flow, scoring six persuasion elements against the site's actual business model, and producing a prioritized, PIE-scored HTML report grounded in cited benchmarks. |
| [developer-handoff-spec-generator](./developer-handoff-spec-generator/) | Documents an existing Figma component, component set, or composed frame for developers, sizing, states, behavior, accessibility, responsive rules, edge cases, and composition, read from real Figma data and written to Figma as a documentation frame beside the target. |
| [lean](./lean/) | Switches Claude into a tighter writing mode on command, cutting filler and padding while keeping full technical accuracy, shaped by clarity rules adapted from ASD-STE100 (Simplified Technical English). |
| [type-scale-generator](./type-scale-generator/) | Generates a brand-new typography type scale for a design system, from an HTML preview through real Figma variables, text styles, and a specimen frame. |

---

## License

CC0 - public domain. All skills in this library are free to use, adapt, and redistribute with no attribution required.
