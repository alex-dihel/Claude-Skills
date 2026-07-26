# Accessibility & Behavior Defaults (load when the interview reaches these fields, and again at spec-writing time)

Used only as a fallback, per the Group 4 sequence in the setup interview:

1. Developer requirements already gathered? → use directly.
2. Not gathered → offer structured optional questions.
3. Declined/skipped → fall back to the defaults below, per sub-area.

**Disclosure is mandatory whenever a default is used instead of a developer answer.** Format:

`(Default)`, appended to the value, nothing longer. Never attribute confirmation to a specific role (developer, designer, etc.), the skill doesn't know who's running the interview. An actually-answered value gets no tag at all.

Mirrors the `→ Brand direct` disclosure convention already used in Color Scale Generator. Never let a fallback value read as if it were a confirmed developer decision.

## The defaults

| Sub-area | Default source | Fetch live or fixed? | Notes |
|---|---|---|---|
| State naming, per widget role | WAI-ARIA Authoring Practices Guide (APG), the pattern for the specific role in play (button, textbox, listbox, etc.) | Fetch live, scoped to the specific role. Not memorized generically. | Same discipline as the WCAG criteria already used in Color Scale Generator: check the actual doc at the point of use. |
| Input types / attributes | WHATWG HTML Standard, `type` and `inputmode` values for form inputs | Fetch live if uncertain; values are stable but check rather than assume | Same distinction GitHub's TextInput Preset example turned on: `type` affects validation and mobile keyboard behavior, and is invisible in Figma. |
| Error / server-state terminology | Standard HTTP status semantics (400, 401, 403, 404, 429, 5xx) | Fixed, well-known, no freshness check needed | Only applies when the target component surfaces server-driven error states. |
| Disabled-field unlock logic | No external standard, this is a documentation requirement, not a value | N/A | Always require a stated trigger condition for any disabled state. Never leave "disabled" unexplained. Sourced from the annotation-kit transcript as the most commonly missed detail in practice. |

## Source

WAI-ARIA APG (w3.org), WHATWG HTML Standard, standard HTTP status semantics; disabled-field rule sourced from the annotation-kit transcript reviewed during scoping.
