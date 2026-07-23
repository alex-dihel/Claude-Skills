# Brevity Filter - A Claude Reference File

A reusable reference file that makes Claude's replies in a project tighter, cutting filler and padding without sacrificing accuracy, technical detail, or completeness.

---

## The Problem It Solves

Claude's default replies carry a certain amount of throat-clearing: pleasantries, hedging, restating the question before answering it, summarizing what was just said. None of that is wrong, exactly, it's just weight that isn't doing any work. In a project where every reply is read closely, that weight adds up.

This isn't a "talk like a robot" filter. Full sentences, full grammar, and every piece of load-bearing technical detail stay exactly as they were. What gets cut is padding, not substance.

---

## What It Does

Once added to a project, the filter:

- Cuts pleasantries, hedging language, restated questions, and redundant recapping from every reply
- Keeps full sentences and normal grammar, no fragment-speak, no dropped articles
- Never touches code, commands, file paths, or error messages, those stay verbatim
- Never compresses safety-relevant warnings, confirmations before an irreversible action, or anywhere cutting words would create ambiguity
- Gives a short, one-time notice at the start of each new conversation in the project, stating that the filter is active and exactly how to turn it off
- Responds to two plain-language toggle phrases, on and off, for the rest of that conversation

It does not shorten answers that genuinely need length, a plan, a tradeoff comparison, a design rationale still gets the space it needs. The rule is: if in doubt between shorter and clearer, choose clearer.

---

## Requirements

- A Claude.ai project (Free, Pro, Max, Team, or Enterprise)
- No Figma, no code execution, no other capability required, this is a plain reference file plus a custom instructions pointer

---

## Installation

1. Download `brevity-filter.md` from this repository
2. In Claude.ai: open the target project, upload `brevity-filter.md` to its knowledge base
3. In that same project's custom instructions, add this pointer:

   > Read `brevity-filter.md` in the project knowledge base and apply it to every reply in this project, per its own rules, including the startup notice at the start of each new conversation and the toggle phrases it defines.

That's it. The filter is now active for every conversation in that project.

---

## Usage

No invocation needed, it applies automatically once installed. Each new conversation in the project opens with a short notice:

> Brevity filter is active in this project. Say **"brevity off"** to turn it off, **"brevity on"** to turn it back on.

**Toggle phrases**, exact wording:

| Phrase | Effect |
|---|---|
| `brevity off` | Suspends the filter for the rest of the current conversation |
| `brevity on` | Resumes it |

The toggle is per-conversation. It doesn't change anything at the project level, the next new conversation in that project starts with the filter active again, per its own default.

---

## Turning It Off

- **For one conversation:** say "brevity off" at any point
- **For the whole project:** remove the pointer line from custom instructions, or remove `brevity-filter.md` from the project's knowledge base

---

## A Note on AI Output

This applies a style constraint, not a content constraint. It changes how much padding surrounds an answer, not the accuracy of the answer itself. If a reply ever feels like it's cutting a detail you actually needed, say so, or turn the filter off for that conversation. The rule built into the file is to favor clarity over brevity whenever the two are in tension, but that judgment call is made by Claude in the moment, not guaranteed perfect every time.

---

## License

CC0 - public domain. Covered by this repository's root `LICENSE` file, which applies to everything inside it. Use, adapt, and redistribute freely, no attribution required.
