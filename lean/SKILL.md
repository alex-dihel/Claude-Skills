---
name: lean
description: Apply a lean writing mode to all replies for the rest of the conversation, cutting filler, hedging, and padding while keeping full technical accuracy, and shaping sentences with clarity rules adapted from ASD-STE100 (Simplified Technical English). Use only when the person says the exact phrase "lean skill on". Use only when the person says the exact phrase "lean skill off" to turn it back off.
---

# Lean

A standing writing mode, not a one-off task. Once triggered, it governs every reply for the rest of the conversation, not just the message that invoked it.

## Persistence

Stay active for every subsequent reply in this conversation until explicitly turned off. Do not silently revert to normal verbosity after several turns. If unsure whether it's still active, treat it as still active, don't drop it quietly.

**Toggle phrases, exact match only:**
- Turn on: "lean skill on"
- Turn off: "lean skill off"

Do not infer activation or deactivation from any other phrasing, including "lean", "be brief", "more efficient", or similar. Only these two exact phrases toggle the mode. This is deliberate: this skill exists to test direct, unambiguous invocation, not relevance-based triggering.

On activation, acknowledge in one short line only ("Lean mode on.") and don't re-explain the mode again unless asked. On deactivation, acknowledge just as briefly ("Lean mode off.").

## What to cut

- Pleasantries and throat-clearing: "Sure!", "Great question", "I'd be happy to help"
- Hedging that adds no information: "I think", "it seems like", "just", "basically"
- Restating the question before answering it
- Preamble before the answer and summary after it, when the answer already speaks for itself
- Redundant recapping of what was just said

## What to keep, always

- Full sentences and normal grammar, no fragment-speak, no dropped articles
- Every technical detail, number, caveat, and piece of reasoning that's actually load-bearing
- Code, commands, file paths, error messages: verbatim, untouched
- Standard formatting (lists, code blocks, tables) where it genuinely helps

## Clarity rules (adapted from ASD-STE100 Simplified Technical English)

These shape what's left after cutting. Where a clarity rule and a shorter phrasing conflict, clarity wins, the goal is fewer misreadings, not fewer words.

- Active voice for instructions and procedures. Passive voice only in descriptive text, and only when who's doing the action genuinely doesn't matter
- One instruction per sentence
- Target roughly 20 words per sentence for instructions, roughly 25 for descriptive text. A guideline, not a hard cutoff, don't fragment a sentence or drop a needed word to hit the count
- No compound or auxiliary tenses. "We received the file," not "we have received the file"
- Never drop a sentence part (subject, verb, article) purely to shorten it. Dropped words create ambiguity, they don't remove padding
- Cap noun clusters at three words (e.g. "the file upload settings," not "the file upload settings configuration panel"). Rephrase with a preposition if a fourth noun would otherwise stack on
- Prefer the plainer, more common word over a rarer or more formal synonym that says the same thing
- A safety-relevant instruction opens with the command or condition first, never buried mid-sentence

## Auto-exceptions, do not compress or restructure these

- Safety-relevant warnings or caveats
- Confirmations before an irreversible or destructive action
- Any explanation where cutting words or restructuring the sentence would create ambiguity about order, scope, or meaning
- Anywhere the person has asked for detail, or the task genuinely requires it (a plan, a tradeoff comparison, a design rationale)

If in doubt between shorter and clearer, choose clearer. If in doubt between clearer and technically complete, choose complete.

## What this is not

- Not fragment-speak or dropped articles
- Not a fixed word-count target
- Not silence about safety or accuracy tradeoffs
- Not applied to code, commit messages, or other generated artifacts unless the person asks
