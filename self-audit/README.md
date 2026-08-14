# Self-Audit - A Claude Skill

A claude.ai skill that runs a structured audit on a document, file, or artifact Claude just created or edited, checking for missed requirements, internal contradictions, misused creation process, and critical functional issues.

---

## The Problem It Solves

After Claude creates something actionable, a SKILL.md, a README, project instructions, a process file, it's easy for a requirement mentioned three turns ago to go unreflected, for a new document to quietly contradict an existing one, or for the process that was supposed to govern the creation (skill-creator's actual workflow, plan-then-approval, quote-don't-paraphrase discipline) to get skipped under time pressure. Writing out a detailed audit request by hand every time is repetitive. This skill packages that request into a single trigger phrase.

---

## What It Does

Once triggered, Self-Audit:

- Reads the actual current state of the artifact being audited, not an earlier draft from memory
- Reads the project's own instructions in full, if the conversation is inside a project, and checks the artifact against every stated rule, not just the obviously relevant ones
- Reads sibling documents the artifact is meant to follow the convention of (other skill folders, a process guide, a requirements doc)
- Checks four fixed categories, and only these four:
  - **Missed items** — requirements stated in the conversation or project files that aren't reflected in the artifact
  - **Contradictions** — internal inconsistency, or conflict with an existing sibling document or the project's instructions
  - **Misused process** — whether creation actually followed the process it was supposed to follow
  - **Critical issues** — anything that breaks functionality or fails structural validation. If the artifact is a SKILL.md, this includes actually running `quick_validate.py` against it, not just inferring whether it would pass
- Reports findings only, grouped by category, each citing its specific source, severity-tagged (critical / worth fixing / minor)
- Never applies a fix automatically, always waits for approval before changing anything

It does not invent hypothetical edge cases or pad an empty category with a minor nitpick to look thorough. If a category has nothing to report, it says so.

---

## Requirements

- A claude.ai account (Free, Pro, Max, Team, or Enterprise)
- Code execution enabled (Settings > Capabilities > Code execution and file creation), required for the Skills feature itself
- If auditing a SKILL.md specifically, the skill-creator toolkit's `quick_validate.py` needs to be reachable in the environment for the critical-issues check to actually run rather than infer

---

## Installation

1. Download `SKILL.md` from this repository
2. In claude.ai: go to **Customize > Skills**, click the "+" button, then "+ Create skill"
3. Select "Upload a skill," and upload a ZIP file containing the `SKILL.md` (a folder named `self-audit` containing the file)
4. The skill appears in your skills list, toggle it on

---

## Usage

This is a manual, one-shot skill. It never runs automatically after Claude creates or edits something, you have to ask for it explicitly.

**Trigger phrase:** `run self audit skill`

If it's unclear which artifact you mean (several created recently, or none this conversation), Claude asks before proceeding rather than guessing.

**Example:**

> You: [asks Claude to build a new SKILL.md]
> Claude: [builds it]
> You: "run self audit skill"
> Claude: [gathers the current file, the project's instructions, and sibling documents, then reports findings by category]

---

## A Note on AI Output

This audits structure and process, not subjective quality or argument strength, that's a different concern (a separate critical-thinking-style skill, if you have one, is the better tool for evaluating reasoning or decisions). Self-Audit only flags what traces to an actual stated requirement, an actual existing document, or an actual functional break. Findings are a starting point for a fix, not the fix itself, review before approving any change it recommends.

---

## License

CC0 - public domain. Covered by this repository's root `LICENSE` file, which applies to everything inside it. Use, adapt, and redistribute freely, no attribution required.
