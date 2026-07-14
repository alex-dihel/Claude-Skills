# Component Spec Template and Worked Examples

Read this before drafting Step 3 content. The template keeps output format consistent across runs. The two worked examples anchor tone and level of detail, one simple component, one composite, since complexity is where documentation quality actually matters most.

## Fill-in template

```
Component: {component_name}
Source: {figma_component_link}
Documented: {date}

## Definition
{one_sentence_definition}

## When to Use
{bulleted_specific_scenarios}

## When Not to Use
{bulleted_common_misuse_cases}

## Variants
{table: variant_property | options}

## States
{table: state_name | trigger | visual_change}

## Anatomy
{table: part_name | description}

## Properties
{table: property_name | type | options_or_range | default | linked_variant}
(linked_variant is blank unless this property is also listed in the Variants section above, in which case name the specific variant property it's the same as, e.g. "Same as Variants: icon")

## Usage Rules
Do:
{bulleted_dos}
Don't:
{bulleted_donts}

## Accessibility Notes
Keyboard: {keyboard_behavior}
Screen reader: {screen_reader_behavior}
Other: {any_additional_behavioral_notes}

## Related Atom Components
{bulleted_list_with_purpose_per_item}
```

## Worked example 1: simple component (Button)

```
Component: Button
Source: figma.com/design/abc123/DesignSystem?node-id=45-102
Documented: 2026-07-11

## Definition
A clickable element that triggers a single action, used for the primary interactive calls to action across the product.

## When to Use
- Submitting a form
- Triggering a single, immediate action (save, confirm, delete)
- Navigating to a new view when the action is the primary path forward

## When Not to Use
- Navigating between pages as a persistent nav item, use a Link instead
- Toggling a setting on/off, use a Switch or Checkbox instead
- Multiple equal-weight choices, use a Segmented Control instead

## Variants
| Property | Options |
|---|---|
| style | primary, secondary, tertiary, destructive |
| size | sm, md, lg |
| icon | none, leading, trailing |

## States
| State | Trigger | Visual Change |
|---|---|---|
| default | resting | base fill and text color |
| hover | pointer over | fill darkens one step |
| pressed | pointer down | fill darkens two steps |
| focus | keyboard tab | 2px focus ring, outside offset |
| disabled | disabled prop true | 40% opacity, no pointer events |
| loading | loading prop true | spinner replaces label, button remains sized |

## Anatomy
| Part | Description |
|---|---|
| container | the clickable bounding area |
| label | the button's text content |
| leading icon | optional icon before the label |
| trailing icon | optional icon after the label |
| spinner | replaces label content during loading state |

## Properties
| Property | Type | Options / Range | Default | Linked Variant |
|---|---|---|---|---|
| style | variant | primary, secondary, tertiary, destructive | primary | Same as Variants: style |
| size | variant | sm, md, lg | md | Same as Variants: size |
| icon | variant | none, leading, trailing | none | Same as Variants: icon |
| label | text | any string | "Button" | N/A |
| disabled | boolean | true, false | false | N/A |
| loading | boolean | true, false | false | N/A |

## Usage Rules
Do:
- Use one primary-style button per view for the main action
- Keep labels short, verb-first ("Save changes," not "Changes will be saved")
- Pair a destructive-style button with a confirmation step for irreversible actions

Don't:
- Don't use more than one primary-style button in the same section
- Don't disable a button without a visible reason nearby, disabled with no context blocks the user silently
- Don't put a button inside another button

## Accessibility Notes
Keyboard: Focusable via Tab, activates on Enter or Space, focus ring visible per the focus state above
Screen reader: Announces label text and role "button"; loading state should also update to an accessible "busy" announcement, not rely on the visual spinner alone
Other: Disabled buttons are removed from the tab order, not just visually dimmed

## Related Atom Components
- Icon: used for leading/trailing icon variants
- Spinner: used for the loading state
```

## Worked example 2: composite component (List Item, nested Checkbox inside a Menu)

```
Component: Menu List Item (checkbox variant)
Source: figma.com/design/abc123/DesignSystem?node-id=88-410
Documented: 2026-07-11

## Definition
A single selectable row inside a Menu, combining a Checkbox with a label and optional metadata, used when a menu needs multi-select rather than single-select rows.

## When to Use
- A menu where a user can select more than one option before confirming
- A filter list where multiple values can be active at once

## When Not to Use
- Single-select menus, use the standard Menu List Item without the checkbox variant
- Standalone form fields outside a menu context, use Checkbox directly

## Variants
| Property | Options |
|---|---|
| density | comfortable, compact |
| metadata | none, trailing-text, trailing-icon |

## States
| State | Trigger | Visual Change |
|---|---|---|
| default | resting, unchecked | base row, empty checkbox |
| hover | pointer over | row background tints |
| checked | checkbox selected | checkbox fill, checkmark visible |
| focus | keyboard navigation | 2px focus ring around the full row, not just the checkbox |
| disabled | disabled prop true | 40% opacity, no pointer events, checkbox unreachable by tab |

## Anatomy
| Part | Description |
|---|---|
| row container | full-width clickable area, clicking anywhere toggles the checkbox |
| checkbox | the nested Checkbox atom, mirrors row state |
| label | the option's primary text |
| trailing metadata | optional text or icon on the row's right edge |

## Properties
| Property | Type | Options / Range | Default | Linked Variant |
|---|---|---|---|---|
| density | variant | comfortable, compact | comfortable | Same as Variants: density |
| metadata | variant | none, trailing-text, trailing-icon | none | Same as Variants: metadata |
| label | text | any string | "Option" | N/A |
| checked | boolean | true, false | false | N/A |
| disabled | boolean | true, false | false | N/A |

## Usage Rules
Do:
- Make the entire row clickable, not just the checkbox itself, the checkbox is a visual indicator of row state, not the only hit target
- Keep row order stable while items are being selected, don't reorder on check
- Use compact density only inside menus with more than roughly 8 items visible at once

Don't:
- Don't nest another interactive control (a button, a second checkbox) inside the row beyond the one checkbox and optional trailing icon
- Don't rely on trailing-icon metadata alone to convey state that also needs to be readable by screen reader, pair it with text or an accessible label

## Accessibility Notes
Keyboard: Full row is focusable via Tab (not the checkbox as a separate stop), Space toggles the checked state
Screen reader: Row announces as a checkbox with its label and current checked/unchecked state, not as a generic row with a nested checkbox, the row and the checkbox share one accessible node
Other: When disabled, the row is removed from the tab order entirely, matching the atom Checkbox's own disabled behavior

## Related Atom Components
- Checkbox: nested inside the row, drives the checked/unchecked visual and accessible state
- Icon: used for trailing-icon metadata variant
```
