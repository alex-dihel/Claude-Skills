# Claude Skills

A growing library of custom Claude skills, built for real work and offered free to the community.

Each skill is a structured prompt that instructs Claude to perform a specific task consistently and reliably. Skills are installed once and invoked on demand - no setup required per conversation.

---

## Available Skills

| Skill | Description |
|---|---|
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
