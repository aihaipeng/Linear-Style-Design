# Micro-Details: Counter-Examples and Fixes

Small, fragmented flaws. Each looks minor in code; in the render they read as "off", and together they read as sloppy. Most of these alone don't break a layout — which is exactly why they survive every structural check; the exceptions collide with a hard floor (visible focus) or AP-14…15, and those are full defects. Use this file during the fix rounds of "Critique the render, not the code" (`review-checklist.md`): sweep in the order below, at actual size and again at 200% zoom. Every entry is a counter-example: what it looks like, why it jars, the fix.

## 1. Geometry and nesting

| Counter-example | Why it jars | Fix |
|---|---|---|
| A menu (12px radius) sitting in a panel (12px radius) — the same radius inside the same radius | The nested object reads bloated, like a sticker slapped on a box | Concentric radii as a guideline (`layout.md`): inner = outer − gap. If the gap already exceeds the outer radius, use no radius at all |
| An input with an attached button where the halves have different heights or unrelated radii | One control, two opinions | Composite controls share one height; the radius goes flat at the joint, full on the outer corners |
| A hairline border that vanishes on one display and looks harsh on another | The structure line flickers in and out of existence | Weight is not a color problem: keep the soft color, go to 2px when 1px disappears |
| A focus ring sliced in half by an `overflow: hidden` container | Keyboard focus appears and vanishes on the same control | Let the ring escape the clip or pad the container; focus visibility beats tidy clipping (`components.md`) |
| A 16px icon scaled up to 40px in an empty-state hero | Chunky, crude strokes — the icon was never drawn for that size | Icons render at their intended size; large visuals get dedicated artwork or a colored container |

## 2. Optical alignment

| Counter-example | Why it jars | Fix |
|---|---|---|
| A play triangle mathematically centered in its circle | Visual mass sits on the flat side; it reads pushed left | Nudge toward the point (~5–8% of the glyph — ≈1–2px at the standard 16–20px icon sizes), ideally inside the SVG so every use inherits it |
| Icon and label of different sizes vertically centered on one line | Different-size text centered by box reads misaligned | `align-items: baseline` for mixed-size content on one line |
| An icon beside a two- or three-line label, centered on the whole block | The icon floats mid-block instead of starting with the text | Align the icon to the first line (a `1lh`-high wrapper), not the block center |
| Button label sitting visibly low or high inside a sized control | Font-metric ghost space, plus lost UA centering once `display` changes | Controls own their centering: inline-flex + `line-height: 1` on single-line text (`components.md`); `text-box: trim-both cap alphabetic` as progressive enhancement |
| A display headline whose first line starts a hair right of the body below | Side bearings break the shared left edge | A small negative nudge (`margin-left: -0.05em`) — left-aligned contexts only |
| A chevron after a label spaced at grid gap | The arrow reads detached from its label | Optical gap for trailing arrows is tighter than the grid (~4–6px) |
| A solid icon visually shouting next to quiet text | Solid fills cover more surface than the text they accompany | Match the icon's color strength down (or a lighter stroke), not the text up |

## 3. Text behavior

| Counter-example | Why it jars | Fix |
|---|---|---|
| Proportional digits in table amounts, timers, counters | `$1.00 → $4.00` makes the whole column wobble; timers jitter | `font-variant-numeric: tabular-nums` wherever digits change or compare (`typography.md`) |
| A heading or paragraph ending in one orphan word | The rag reads accidental | `text-wrap: pretty` on paragraphs; `balance` on centered headings (≤4 lines) |
| End-truncated filenames, IDs, URLs — every row starts with the same words | The truncated part is exactly what made the values distinguishable | Middle truncation when the tail carries the identity (`Screensho…at 14.32.08.png`) |
| Truncated cell or menu text with no path to the full value | Data is unreadable exactly where it matters | Ellipsis + full text through a visible, actionable path (details, expand, the menu); `title`/`aria-label` are supplements, never the sole path; critical terms, form labels, and primary buttons are never truncated (`typography.md`) |
| A long URL, ID, or token blowing out the row and the page with it | One user string breaks the layout for everything else | `overflow-wrap: anywhere` (or `break-words`) inside the bounded container; in tightly constrained rows (lists, tables), middle-truncate identifiers instead (`components.md`) |
| ALL-CAPS micro-labels at default tracking | Caps at sentence-case tracking look cramped | Positive letter-spacing (~0.05em) on caps labels |
| Hover swapping the weight 400 → 600 | Text width jumps and the row reflows under the cursor | Animate color or background, not weight; if weight must change, reserve the width |

## 4. Scroll and overflow affordances

| Counter-example | Why it jars | Fix |
|---|---|---|
| A scrollable list whose container ends exactly at an item boundary | Nothing hints that more content exists; users miss it entirely | Size the container so the last visible item is partially cut, or add an overflow fade at the edge |
| Scrollbars stripped out entirely (`scrollbar-width: none` everywhere) | Long content becomes undiscoverable | Thin scrollbars colored from your tokens are fine; never remove them from content that scrolls |
| The page behind an open modal scrolling with the wheel | Context creeps; on close the user is somewhere else | Lock body scroll or `overscroll-behavior: contain` on the panel |
| Overscroll exposing a raw canvas-colored band (iOS) | A stripe of the wrong background at the top/bottom | Root background continuity with the page surface |
| A horizontal chip/card rail with no arrows, no fade, no peeking item | Pure gesture knowledge — invisible affordance | Edge fade, arrow, or a partially visible next item before relying on a gesture |

## 5. States that read true

| Counter-example | Why it jars | Fix |
|---|---|---|
| Hover lifting, scaling, or shifting a whole table row | Rows are reading surfaces; movement reads as navigation | List-row hover shifts background only — no lift, scale, or translate; hover-revealed actions stay allowed (`components.md`) |
| `:active` firing as slowly as hover | Press feels disconnected from the finger | Press feedback is faster than hover (≤80ms); subtle scale (0.97–0.98) or 1px translate |
| A disabled control that still lights up on hover | Conflicting signals: is it operable or not? | Disabled removes hover effects; dim per `components.md`, reason reachable |
| A caret invisible on the dark input, or selection color unreadable | The user types blind for a character or two | `caret-color` and `::selection` as project tokens, checked in dark and light |
| A focus ring that ignores the control's radius or shape | The ring floats as a rectangle around a rounded control | The ring follows the shape — inherit the radius or draw the custom ring to match |

## 6. Micro-consistency

| Counter-example | Why it jars | Fix |
|---|---|---|
| Sidebar icons at 12px in one section, 16px in another | Labels shift sideways between sections; the rail loses its line | One size per icon role; labels align to one vertical rhythm |
| A submenu whose first item sits above or below its trigger | The eye stumbles on every level change | First submenu item aligns exactly with the trigger row |
| The same widget behaving differently per screen — dropdown below here, above there; validation on blur here, on submit there | Feels like two products stitched together | One pattern per role across the product (`principles-and-evidence.md`: precise, not rigid) |
| Same-row same-role controls at different heights (a 32px input next to a 28px button) | The row reads accidental | One height per control row; equal roles, equal heights (`components.md`) |

## Sweep order and judgment

Sweep in file order — geometry, optical, text, scroll, states, consistency — because a geometry fix can dissolve an apparent alignment bug. Findings use the same levels as anti-patterns: a counter-example matching an AP entry or a hard floor is Fail; the rest are Needs reason; purely cosmetic polish may be listed as an advisory without affecting the verdict. Anything arguable is judged on the render at actual size, not in code. When a counter-example has a matching anti-pattern (AP-14 or AP-15, `review-checklist.md`), cite both.
