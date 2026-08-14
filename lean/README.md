# Lean - A Claude Skill

A claude.ai skill that switches Claude into a tighter writing mode on command, cutting filler and padding while keeping full technical accuracy, and shaping what's left with clarity rules adapted from ASD-STE100 (Simplified Technical English).

---

## The Problem It Solves

A chunk of any AI reply is padding: pleasantries, hedging, restating the question before answering it, summarizing what was just said. None of it's wrong, it's just weight that isn't doing anything. Lean strips that weight on demand, in any claude.ai conversation, without needing a project-level setup.

This isn't a fragment-speak or robotic style. Full sentences and full grammar stay. What's cut is padding, not substance, and what remains is shaped for clarity, not just brevity.

---

## What It Does

Once triggered, Lean:

- Cuts pleasantries, hedging language, restated questions, and redundant recapping from every reply
- Keeps full sentences and normal grammar, no fragment-speak, no dropped articles
- Applies ASD-STE100-derived clarity rules to what remains: active voice for instructions, one instruction per sentence, a soft ~20-25 word sentence-length target, no compound tenses, no stacked noun clusters past three words, plain words over rare synonyms
- Never touches code, commands, file paths, or error messages, those stay verbatim
- Never compresses safety-relevant warnings, confirmations before an irreversible action, or anywhere cutting words would create ambiguity
- Stays active for the rest of the conversation once triggered, not just the one reply

It does not shorten answers that genuinely need length. A plan, a tradeoff comparison, a design rationale still gets the space it needs. Where brevity and clarity conflict, clarity wins.

---

## Requirements

- A claude.ai account (Free, Pro, Max, Team, or Enterprise)
- Code execution enabled (Settings > Capabilities > Code execution and file creation), required for the Skills feature itself

---

## Installation

1. Download `SKILL.md` from this repository
2. In claude.ai: go to **Customize > Skills**, click the "+" button, then "+ Create skill"
3. Select "Upload a skill," and upload a ZIP file containing the `SKILL.md` (a folder named `lean` containing the file)
4. The skill appears in your skills list, toggle it on

---

## Usage

Lean only activates on an exact trigger phrase, deliberately, it does not fire on the bare word "lean" or on general requests to "be brief," since those risk false triggers in ordinary conversation.

| Phrase | Effect |
|---|---|
| `lean skill on` | Activates lean mode for the rest of the conversation |
| `lean skill off` | Deactivates it |

No other phrasing toggles the mode. If you say "lean skill on" again while it's already active, Claude treats it as a no-op and confirms briefly.

**Note on reliability:** claude.ai skills are invoked based on Claude matching your message against the skill's description, not a hardcoded command. This skill's description is written to make the exact-phrase match as unambiguous as possible, but it is still subject to the same invocation mechanics as any other claude.ai skill, not a guaranteed slash command. If it doesn't fire, repeating the exact phrase, or saying "use the Lean skill," is the documented fallback.

---

## A Note on AI Output

This is a style constraint, not a content constraint. It changes how much padding surrounds an answer, not the accuracy of the answer itself. The judgment call between shorter and clearer is made by Claude in the moment, not guaranteed perfect on every reply.

Related project: this skill's rule set is adapted from [`brevity-filter.md`](https://github.com/alex-dihel/Claude-Skills/tree/main/brevity-filter), a project-scoped reference-file version of the same idea, itself inspired by [Caveman](https://github.com/JuliusBrussee/caveman), a Claude Code skill for aggressive token-saving compression. Lean is built for direct, explicit invocation in claude.ai specifically, rather than always-on project instructions.

---

## License

CC0 - public domain. Covered by this repository's root `LICENSE` file, which applies to everything inside it. Use, adapt, and redistribute freely, no attribution required.
