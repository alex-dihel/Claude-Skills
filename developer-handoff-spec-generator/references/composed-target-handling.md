# Composed-Target Handling (load at Step 1, node-type detection)

## Detection

On reading the target node, determine which case applies:

- **Component or Component Set**, read its own variants/properties directly, the standard case.
- **Composed frame of instances**, a frame (not itself a Component/Component Set) whose functional parts are instances of already-existing components, plus possibly some non-componentized content (raw text, icons). Example encountered during scoping: a "Create Account" panel frame containing 4 instances of a "With Label" input component, 1 instance of a "Buttons" component, and non-componentized heading/hint/footer text.

For a composed frame, walk its children and read each instance's own properties individually. Do not expect top-level variant/property data on the frame itself, it won't have any. Non-componentized decorative content (labels, static copy) has no property data, treat it as content only. Non-componentized functional content with no internal variance (a plain link with a fixed href) does have real data, its actual target/destination, capture that rather than treating it as content-only or rejecting the target over it.

## Scope boundary

**In scope:** a single Component/Component Set, or a composed frame where every functional part with real internal complexity (variants, states) is an instance of an already-existing component, and the whole thing is a single, boundable unit (one screen's worth of one panel). A simple, fixed-destination element with no internal variance (a plain link with a static href) stays in scope even if it isn't componentized, there's nothing to component-ize, documenting it directly is correct.

**Out of scope, deferred to a future skill:** multi-screen flows, cross-page navigation, system-wide error-state bucketing, file-wide landmark/navigation structure. These are page/flow-level concerns, not properties of a single documented unit, and don't fit the current 8-field set (sizing, composition, etc. are defined per-unit, not per-page).

**Reject only on real internal complexity, not on any non-componentized element.** A frame is out of scope only if a functional part has real internal variance (variants, states, multiple modes) and isn't componentized. Nearly every real composed frame contains plain, non-componentized links (a "Sign in" link, legal text links), and rejecting on their presence would make the skill reject most real targets, including the exact panel used to validate this whole concept. Document simple non-componentized functional elements directly instead of rejecting them.

## Accessibility scope for composed targets

- **Always documented, widget-level:** the target's own landmark role and accessible name; internal tab order across its own children (instances and non-componentized elements alike), regardless of page context.
- **Per-child accessible name/description:** check first (via the sourcing check) whether the child instance already has its own Developer Handoff doc. Reference it if found. Document inline only if not.
- **Explicitly out of scope:** focus order and landmark relationships relative to sibling content *outside* the target. That varies by page and user context, it isn't a fixed property of the widget itself.

## Source

Scoping discussion in this project, tested directly against a real Figma node (`Create Account Panel`, fileKey `U1Ub8fVDQk8epGPhot7teM`). Cross-validated by GitHub's Preset annotation principle: document once at the level where a fact is invariant, only re-document what varies per instance or per composition.
