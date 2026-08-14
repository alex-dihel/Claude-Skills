---
name: self-audit
description: Run a structured audit on a document, file, or artifact Claude just created or edited (SKILL.md, README, project instructions, process files, or any actionable deliverable), checking for missed requirements, internal contradictions, misused creation process, and critical functional issues, always cross-checked against the project's own instructions and existing sibling documents when present. Use only when the person says the exact phrase "run self audit skill". Do not run automatically after creating or editing a document, manual trigger only.
---

# Self-Audit

A manual, one-shot check. Runs only when explicitly triggered, never automatically after a creation or edit task.

## Trigger

Exact phrase only: "run self audit skill". Do not infer this from general requests like "check this" or "review this" unless the person uses the exact phrase, or directly confirms they mean this skill when asked.

If it's unclear which artifact to audit (multiple files created recently, or none created this conversation), ask which one before proceeding.

## Step 1: Gather source material before auditing

Do not audit from memory. Before checking anything, gather:

- The artifact itself, read in full, current state, not an earlier draft from earlier in the conversation
- The project's own instructions/custom instructions, if this is a project conversation, read in full
- Any sibling documents the artifact is meant to sit alongside or follow the convention of (e.g., other skill folders, a process guide, a requirements doc)
- The specific conversation turns where the artifact's requirements were actually stated, not a summary of them

This step is not optional. An audit run without checking the actual current file and the actual current project instructions is not a self-audit, it's a guess.

## Step 2: Check four categories, and only these four

**Missed items** — requirements stated in the conversation, project files, or prior turns that aren't reflected in the artifact.

**Contradictions** — internal inconsistency within the artifact, or conflict with an existing sibling document or with the project's own instructions.

**Misused process** — did creation follow the process it was supposed to follow (e.g., skill-creator's actual workflow if this is a skill, plan-then-approval, quote-don't-paraphrase discipline, any process rule stated in the project's instructions).

**Critical issues** — anything that breaks functionality, violates a stated guardrail, or fails structural validation. If the artifact being audited is a SKILL.md, this means actually running `quick_validate.py` against it (from `/mnt/skills/examples/skill-creator/scripts/`), not just checking for the kinds of problems that script would catch. Report the script's actual pass/fail result as part of this category, don't infer it.

## Step 3: Mandatory project-instructions cross-check

If the conversation has project instructions (custom instructions for the current project), this check is required, not optional, regardless of what the other three categories find.

- Read the project instructions in full, not from memory of earlier in the conversation
- Check the artifact line by line against every rule stated there, not just the ones that seem obviously relevant
- Report any conflict as a Contradiction finding, citing the specific instruction line and the specific artifact line that conflict
- If there are no project instructions in the current context, state that plainly and skip this check, don't invent a placeholder finding

## Scope discipline

Flag only what traces to an actual stated requirement, an actual existing document, or an actual structural/functional break. Do not invent hypothetical misuse scenarios, do not generate a hedge-everything list, do not manufacture an edge case with no anchor in what was actually asked for or actually written. An empty category is a valid result, state "none found," don't pad it with a minor nitpick to look thorough.

## Output format

Structured findings, grouped under the four category headers plus the project-instructions cross-check. Each finding states:
- What's wrong
- Why, citing the specific source it conflicts with or omits (a quote or line reference, not a paraphrase)
- Severity: critical / worth fixing / minor

Report only. Do not apply any fix automatically. Wait for explicit approval before changing anything, per standard plan-then-approval discipline.
