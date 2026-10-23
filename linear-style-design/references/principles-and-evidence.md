# Design Philosophy and Judgment Methods

## Let content earn the attention; let the shell provide direction

Linear's stated principle: "Don't compete for attention you haven't earned."

- The content area carries the task's primary contrast; navigation provides orientation, not a permanently glowing billboard.
- Make the current page, focus, selection, and results explicit instead of rendering everything equally bright.
- Keep the context the task needs; noise reduction is not hiding useful information three menus deep.
- One primary action per action area by default; having none is acceptable when nothing warrants it.

**How to judge:** Shrink a screenshot — the first thing you notice should be the current work object or primary information, not a sidebar brand block, ten colored badges, and a row of highlighted buttons.

## Structure should be felt, not seen

- Group with alignment and spacing first; add dividers only if still needed.
- When adjacent information shares context, prefer rows and groups; don't default to a card container per segment.
- Lines must explain a relationship: region boundaries, table headers, column alignment, drop targets — never "premium feel" alone.
- Don't mistake "cards have no shadow" for "everything must be a card".

## Precise, not rigid

- Same semantics, same tokens, sizes, and interactions.
- Optical alignment beats blind numbers: asymmetric icons, glyph sidebearings, and different languages can all need 1–2px adjustments.
- Keep default weights, radii, and transitions few; don't promote every sampled value into a global token.
- Don't spin up a whole variant system for one exception; first check whether the exception is real and recurring.

## Density serves efficiency, not "looking professional"

- Show the information users need to compare, not every field you can fetch.
- Desktop lists may be compact; reading, settings, and mobile interaction must leave room for text and input.
- Important headings may wrap; errors and help text must never be clipped by a fixed line height.
- Density stays consistent within one area; the sidebar and a settings form may use different densities.

## Fast means feedback, continuity, and recoverability

- Input and local interactions respond immediately; never mask blocking with animation.
- Preserve user context throughout an operation; avoid full-page flashing, scroll-to-top, re-sorting, or focus loss.
- Network failures produce an explicit outcome and recovery path; a fleeting toast is not a substitute for error handling.
- High-frequency, safely undoable actions suit optimistic updates; payments, permission changes, and hard deletes must never fake success for speed.

## Constrain brand expression; don't erase the brand

- One brand accent may have multiple lightness states; they still belong to one family.
- Charts, statuses, avatars, and user content are not counted against the "one color only" reading.
- Replace the brand color, language fonts, and product-specific layout freely; keep the order and restraint.
- Don't build your own brand identity by copying Linear's logo, wordmark, or proprietary graphics.

## Three context types

For the context split and each context's "inherit / don't copy verbatim" lists, see SKILL.md. **Architecture-agnostic does not mean context-agnostic**: the same principles adapt to any frontend, but not every frontend should be made to look like the same desktop project-management screenshot.

---

# Nature of the Evidence

This specification is a Linear-inspired implementation baseline built from official material, measurements of public pages, and WCAG arithmetic — not the official design system. The evidence labels and their limits:

| Label | Meaning | Limits |
|---|---|---|
| **Official** | Explicitly stated in Linear's articles, brand notes, or product docs | Proves exactly what was written; does not automatically prove unpublished specifics |
| **Measured** | Readings of public-page DOM, computed styles, fonts, and variables; or direct reads of files shipped inside a legitimately installed desktop client bundle | Represents only the specific page/bundle version and sampling moment — not the logged-in app, not future versions |
| **Verified** | Recomputed or checked against W3C rules | Used to correct arithmetic, standard interpretation, and implementation errors |
| **Normative** | Parameters, recipes, and engineering requirements chosen for consistent implementation | This specification's decisions; they don't impersonate official tokens |

In other files these labels are abbreviated as `[O]` = Official and `[M]` = Measured.

Apart from facts labeled "Official"/"Measured", every token table, component size, breakpoint, and CSS example in these references is "Normative" — a normalized implementation baseline, not a claim that "all of Linear always does this".

**Genuine ambiguities (don't trust a single source):**

- Inverted or brand-indigo primary buttons: both exist in Linear's real product (inverted in productivity UI, indigo in sign-in/brand flows); this spec distinguishes by context rather than picking one.
- The hover color for `accent.fill` cannot simply be the brighter `#828fff`: white text on it is only ~2.87:1 contrast — non-conforming.
- Dark mode is not a prerequisite for the Linear style; the product also ships a light theme, and light is not a second-class theme.
- Mono is not confined to code; a few brand-emphasized headings (e.g., error pages) use it, but it should not become the default for body headings.
- Buttons are not globally pill-shaped: system-level functional buttons commonly use 5–8px rectangular radii; pills are mostly for marketing CTAs, sign-in flows, and compact controls.
