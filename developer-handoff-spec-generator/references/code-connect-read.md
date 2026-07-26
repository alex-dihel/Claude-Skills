# Code Connect: Read-Only Boundary

This skill treats Code Connect as a **read-only source**, checked as step 2 of the sourcing check (see `sourcing-check.md`).

## Availability, check before asserting a reason for absence

Code Connect requires an Organization or Enterprise plan, with a Dev or Full seat within that plan. It is not available on the Professional (Pro) plan at all, regardless of seat type, seat type is irrelevant if the workspace itself is on Pro. If a sourcing check finds no Code Connect binding, don't guess at or assert a specific reason (plan, seat, or otherwise) unless the actual tool response states it. If the reason isn't confirmed, report the absence plainly without explaining why.

## What this skill does

- Checks whether a given attribute (e.g. an accessible name, a validation message) is already bound via Code Connect on the target or its child instances.
- If found, the spec **references** the bound property (names it) rather than restating its value as separate prose. This avoids two sources of truth that can drift apart.

## What this skill never does

- Never creates a new Code Connect mapping.
- Never edits an existing Code Connect mapping.
- Creating/editing Code Connect mappings belongs entirely to the `figma-code-connect` skill. If a gap is found that looks like a good Code Connect candidate (an attribute with nowhere else to live, hidden component property style), the spec can note this as an advisory, a pointer for the user to consider, not an action this skill takes.

## Why this boundary exists

Two skills independently maintaining the same kind of binding data is exactly the drift risk this project already guards against elsewhere (the "two separate skills, not a mode switch" principle for Component Spec vs. Developer Handoff Spec Generator). Read-only access gets the benefit (no redundant documentation) without creating a second write path for the same data.

## Source

GitHub Accessibility Design team, "Design system annotations, part 2" (github.blog). Their IconButton example: a hidden Figma layer holds an `aria-label` text property, bound via Code Connect, exported directly into generated code, no annotation needed for that attribute once it exists.
