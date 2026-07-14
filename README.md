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
| [type-scale-generator](./type-scale-generator/) | Generates a brand-new typography type scale for a design system, from an HTML preview through real Figma variables, text styles, and a specimen frame. |

---

## How to Install a Skill

1. Open the skill folder and download `SKILL.md`
2. Open [Claude.ai](https://claude.ai)
3. Go to **Customize > Skills > Add Skill**
4. Paste the contents of `SKILL.md` into the skill editor
5. Save

Skills require Code execution to be enabled: **Settings > Capabilities > Code execution and file creation**.

Available on Free, Pro, Max, Team, and Enterprise plans.

---

## License

CC0 - public domain. All skills in this library are free to use, adapt, and redistribute with no attribution required.
