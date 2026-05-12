# Squarespace Custom HTML Timeline Block - Specification

## Purpose
Create a reusable timeline block for Squarespace that can be generated consistently by any AI from this specification.

This document defines:
- Content structure (author input format)
- Desktop layout behavior
- Mobile layout behavior
- Timeline line and marker behavior
- Animation behavior
- Variables and customization expectations
- Accessibility and implementation constraints

No implementation code is included here by design.

## 1) Authoring Input Contract
The timeline content must be authored as a single unordered list (`<ul>`) of timeline events (`<li>`).

Each `<li>` represents one time-event and supports up to 4 content elements in this order:
1. `year` (required)
2. `title` (required)
3. `description` (optional)
4. `image` (optional)

### Required semantics
- `year` and `title` must always be present.
- `description` and `image` may be missing independently.
- If either optional field is missing, the event still renders correctly with no layout break.

### Canonical event model
Treat each `<li>` as a data object:
- `year: string` (required)
- `title: string` (required)
- `description: string | null` (optional)
- `image: URL | null` (optional)

## 2) Desktop Layout Spec

### Column geometry
At desktop/tablet widths above the mobile breakpoint, the block is a 3-column layout:
- Left column: `45%`
- Middle timeline column: `5%`
- Right column: `45%`

The middle column is the timeline axis.

### Alternating event placement
Events are vertically stacked in chronological/source order and alternate sides, starting on the **right**:
- Event 1: content on right, matching blank spacer on left
- Event 2: content on left, matching blank spacer on right
- Event 3: content on right, matching blank spacer on left
- Event 4: content on left, matching blank spacer on right
- Continue alternating for all remaining events

### Spacer requirement
For each row, the side opposite the rendered event must include blank space with height equal to that event’s rendered height so rows remain aligned to the timeline center.

## 3) Timeline Axis and Markers

### Vertical line
- A single vertical line runs through the middle timeline column.
- Height: `100%` of the timeline block.
- Color: `color variable 1`.

### Event marker dots
- Each event has one circular marker placed on the timeline line.
- Marker vertical alignment must be at `50%` of that event row’s height.
- Marker color: `color variable 2`.
- Marker sits visually on top of the vertical line.

## 4) Mobile Layout Spec (iPhone 12 Pro Width and Below)

### Breakpoint behavior
At iPhone 12 Pro width and smaller, collapse to 2 columns:
- Left timeline column: `10%`
- Right events column: `90%`

### Event flow on mobile
- Disable alternating sides.
- All events render in the single right events column.
- Events keep chronological/source order.
- Add vertical spacing (margin) between events.

### Timeline behavior on mobile
- The vertical timeline line remains in the left column.
- Event marker dots align to each event’s vertical midpoint as on desktop.

## 5) Animation Requirements

### Entry animation
Apply a soft fade-in animation to the entire event container.

### Trigger behavior
- Fade in when the event first appears in the viewport.
- Do not fade out when leaving the viewport.
- Do not repeatedly re-run fade-in on every scroll pass unless explicitly configured otherwise.

### Style note
Use a built-in/simple CSS fade style (soft opacity transition feeling), not an aggressive motion effect.

## 6) Customization Variables
At minimum, support the following configurable values:
- `timeline line color` (color variable 1)
- `timeline dot color` (color variable 2)
- Event spacing (especially mobile gap)
- Optional typography sizing for year/title/description
- Optional marker size

## 7) Accessibility and Content Rules
- Preserve logical reading order in source: event 1, event 2, event 3, etc.
- Images must support meaningful `alt` text when provided.
- Missing optional fields (`description`, `image`) must not create empty visual artifacts.
- Maintain adequate color contrast for text and timeline elements.

## 8) Acceptance Criteria Checklist
A generated implementation is correct only if all are true:
1. Uses one `<ul>` with multiple `<li>` events.
2. Each event supports required `year` + `title` and optional `description` + `image`.
3. Desktop uses 45/5/45 three-column layout.
4. Alternation starts on the right for event 1 and continues right/left/right/left.
5. Opposite side contains equal-height blank spacer per event row.
6. Middle timeline line spans full block height.
7. Each event has a dot centered at 50% row height on the line.
8. Mobile breakpoint (iPhone 12 Pro width and below) becomes 10/90 two-column layout.
9. Mobile disables alternation and stacks all events on one side.
10. Vertical spacing exists between mobile events.
11. Soft fade-in occurs when event enters viewport.
12. No fade-out on viewport exit.

## 9) Implementation Hint for Future AI Instances
When generating from this spec, keep the author-facing content entry minimal:
- The user should only need to add/edit `<li>` items inside one `<ul>`.
- Internal rendering logic handles alternation, spacers, timeline markers, responsiveness, and entry animation.

