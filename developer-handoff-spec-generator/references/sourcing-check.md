# Sourcing Check (load before generating any interview question)

Applies to every field in the 8-field set (parts, sizing, states, behavior, accessibility, responsive rules, edge cases, composition). Before turning a field into an interview question, check in this order. Only what survives all four gets asked.

1. **Figma component property/variant**, is this already exposed as a property on the component or component set itself?
2. **Code Connect binding**, is this already bound via Code Connect? Read-only check. This skill never creates or edits Code Connect mappings, that's `figma-code-connect`'s job. If found, the spec references the bound property rather than restating its value.
3. **Existing atom doc**, if the target is a composed frame, does the child instance already have its own Developer Handoff or Component Spec documentation frame? If found, reference it, don't restate it.
4. **Existing codebase/docs**, has the user pointed to a codebase or design-system doc that already covers this?

**State back what was resolved this way before continuing.** Show the user which fields were skipped and why, don't let the interview just get shorter silently.

## Source

GitHub Accessibility Design team, "Design system annotations, part 1 & 2" (github.blog). Their rebuild of the CVS Health Web Accessibility Annotation Kit into Primer A11y Presets was built on this exact principle: only capture what isn't already conveyed visually, in component properties, or in the coded implementation. Redundant restatement is itself a failure mode, not just wasted effort, since it creates two sources of truth that can drift.
