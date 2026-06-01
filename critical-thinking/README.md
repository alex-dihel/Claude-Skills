# Critical Thinking - A Claude Skill

A Claude skill that applies the FORCE anti-sycophancy framework to evaluate proposals, decisions, and artifacts. It surfaces real flaws, assigns severity, and delivers a clear verdict instead of the agreeable, hedge-everything feedback Claude produces by default.

Based on the FORCE framework by [Enovair](https://enovair.com). Original framework video: [The FORCE Framework](https://youtu.be/F4GmBmUJuGI?si=GIq73VvXhLRBlPoI). Framework guide available free at [enovair.com](https://enovair.com/offers/download-the-FORCE-Framework-Guide).

---

## The Problem It Solves

AI models are trained to be agreeable. In practice, this means they will find merit in a flawed business plan, confirm assumptions you should be testing, and soften criticism to avoid friction. For everyday use, this is fine. For high-stakes decisions, it is a liability.

This skill instructs Claude to evaluate without softening criticism, for the duration of the evaluation.

---

## What It Does

On invocation, the skill:

- Infers the appropriate critic persona from the task type (engineer, CFO, legal counsel, editor, and others)
- Evaluates the submitted material through that lens
- Returns findings ordered by severity, each tagged [Blocking], [Material], or [Minor]
- Closes with a one-line verdict: do not proceed, proceed after resolving blocking findings, or proceed

It does not produce balanced feedback. It does not look for positives. It reports what is wrong and what needs to happen before the thing should move forward.

---

## Requirements

- A Claude.ai account (Free, Pro, Max, Team, or Enterprise)
- Code execution enabled (Settings > Capabilities > Code execution and file creation)

---

## Installation

1. Download `SKILL.md` from this repository
2. Open [Claude.ai](https://claude.ai)
3. Go to **Customize > Skills > Add Skill**
4. Paste the contents of `SKILL.md` into the skill editor
5. Save

The skill is now available in any conversation or project where it is enabled.

---

## Usage

For best results, invoke the skill at the start of a new conversation.

Invoke the skill by including one of these phrases in your message:

- `use critical thinking`
- `apply critical thinking`
- `evaluate this with critical thinking`

Then paste or describe the thing you want evaluated. The skill handles the rest.

**Example invocation:**

> Use critical thinking to evaluate this business plan: [paste document]

**Phrases that do not activate the skill:**

> "Review this" / "What do you think?" / "Be harsh" / "Don't hold back"

These are normal requests. The skill only fires on explicit invocation.

---

## Output Format

```
Persona: [assigned critic role]

Findings

[Blocking] Finding title
- Original: the specific element being evaluated
- Issue: what fails and why
- Solution: concrete fix, or what decision/information is needed first

[Material] Finding title
...

[Minor] Finding title
...

Verification
- Do not trust without independent verification: [specific claims]
- Missing context that would change this analysis: [gaps]

Verdict: [Do not proceed / Proceed after resolving blocking findings / Proceed]

"Evaluation complete."
```

---

## Supported Task Types

The skill selects a critic persona based on task type. Supported domains:

| Task Type | Persona |
|---|---|
| Code / Technical solution | Senior engineer |
| Architecture / System design | Principal engineer |
| Business strategy / Product | Senior adviser |
| Writing / Argument | Editor |
| Creative / Fiction | Developmental editor |
| Hiring / Team decision | Skeptical CHRO |
| Financial / Investment | CFO |
| Design / UX | Design director |
| Security / Infosec | Security engineer |
| Legal / Compliance | Legal counsel |
| Data / Research | Senior analyst |
| Instructions / Specifications / Prompts | Systems reviewer |

For two-domain tasks, the skill prioritizes the domain where failure is hardest to recover from.

---

## Examples

The `examples/` folder contains a worked evaluation:

- `test-artifact.md` - a CMO business plan proposing a 48% design headcount reduction justified by AI productivity gains
- `example-output.md` - the full skill output against that plan, including 7 findings, verification, and verdict

---

## A Note on AI Output

This skill is a structured prompt. It instructs Claude to be critical and direct, but it does not guarantee accuracy. Claude can miss flaws, misread context, or produce confident-sounding findings that are wrong. The output of this skill is a starting point for your own judgment, not a replacement for it. Do not act on findings without independently verifying the ones that matter.

---

## License

CC0 - public domain. Use, adapt, and redistribute freely, no attribution required.
See `LICENSE` for details.
