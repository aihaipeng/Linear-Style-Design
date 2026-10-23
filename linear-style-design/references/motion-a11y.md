# Interaction, Motion, and Feedback

## Motion timing

| Context | Default | Acceptable range |
|---|---:|---|
| Hover / color feedback | 100ms | High-frequency states may change instantly |
| Menu / small popover enter | 160ms | High-frequency command menus may use 0ms |
| Dialog / drawer enter | 200ms | Interaction must not wait for the animation |
| Close/exit | 100ms | Usually no slower than enter |
| Partial view switch | 160ms | Simple data swaps may use 0ms |
| One-off marketing reveals | ≤300ms as a starting suggestion | Serve the content; no identical scroll-in for every section |

- Default easing `cubic-bezier(.25,.46,.45,.94)` (`--ls-ease-out` in `assets/linear-tokens.css`); a chosen curve, not the one official curve.
- "High-frequency" means actions repeated many times per session — menu opens, tab switches — not first-run or one-off reveals.
- Colors, backgrounds, and borders may transition briefly; spatial changes prefer transform and opacity.
- Dialogs may use opacity + `scale(.98 → 1)`; menus may nudge 2–4px; no bouncing or forced spring-back.
- Avoid `transition: all`; layout, color, and size shouldn't accidentally animate together.
- Don't turn ordinary hover into a scaling card; platform haptics, dragging, and direct gestures are a different feedback class.
- Direct-manipulation interactions must be interruptible and reversible; spring parameters depend on platform and implementation — no cross-framework stiffness number as a universal rule.
- Animations never block the keyboard; don't artificially delay responses so spectators can admire the transition.
- Under `prefers-reduced-motion`, remove non-essential spatial motion, pulsing, and autoplay decoration; keep instant state feedback.

## Loading and network feedback

| Situation | Default feedback |
|---|---|
| Local actions, existing data | Show the new state immediately |
| Short requests | Button/local busy state; avoid skeleton flashes |
| ~200ms+ with content genuinely missing | A stable skeleton or local progress |
| Longer tasks | Say what's happening; real progress when measurable; cancellation where appropriate |
| Refreshing an existing list | Keep old content marked as refreshing; don't blank the page |
| Request failure | In-place error with retry; a notice if needed — never swallow context |

- Spinners aren't banned: small inline spinners suit short indeterminate tasks; avoid unexplained full-page spinners.
- More skeletons isn't better; they can't replace real empty states, permission errors, or failures.
- Don't hold skeletons a few extra hundred ms after responses to "feel smoother".
- Keep placeholder sizes, image ratios, and button widths stable to reduce layout shift.

## The bounds of optimistic updates

**Suited to:** undoable, low-risk actions — favorites, light state toggles, list reordering — provided failures are handled correctly.

**Never reported as done prematurely:** payments, hard deletes, permission and sharing changes, publishing irreversible content, security-verification states.

When enabling optimistic updates, you must:

1. Distinguish local pending from server-confirmed state.
2. On failure, keep/restore the correct state and offer retry or undo.
3. A stale request's failure must not roll back a newer, already-successful operation.
4. No duplicate side effects; no bypassing auth, server-side validation, or audit.
5. Screen-reader users learn key outcomes too — not just a flash of color.

## Keyboard and shortcuts

- Every operable feature has a plain keyboard path; shortcuts are not a replacement for Tab navigation.
- Not every action needs a shortcut; provide a few consistent, discoverable accelerators for high-frequency actions.
- Don't intercept browser/host shortcuts unless the environment explicitly supports it with an equivalent path.
- No character shortcuts during editing, `contenteditable`, composition, or IME candidate states.
- **Avoiding input fields isn't enough:** single-letter and sequential shortcuts must be remappable, disablable, or scoped to a focused component (WCAG 2.1.4).
- `?` is a character shortcut too; requiring Shift grants no exemption.
- Esc is handled topmost-first, closing submenus/popovers before anything else — never exiting the whole flow in one press.
- Menus and lists use semantics-appropriate arrow-key policies; ordinary pages don't hijack all arrow keys globally.
- Tooltips and menus may display shortcuts; users never need to memorize them to find features.

## Copy and recovery

- One name per action across buttons, confirms, notifications, and history.
- Errors say what failed, whether anything was kept, and what to do next — not just "something went wrong".
- No cuteness or user-blaming on system errors; no implementation jargon in place of task names.
- Destructive actions name the object and outcome; provide undo when it truly works — fake undo is no substitute for irreversible confirms.
- Empty-state actions match the cause: no search results offer "clear filters", not always "create project".

---

# Accessibility and Cross-Platform Requirements

This section lists key implementation baselines; it does not claim to cover every WCAG 2.2 success criterion. Full conformance still requires auditing the real product.

## Contrast and color

- Normal text at least 4.5:1; large text may use 3:1 per the standard, but small UI text is always verified as normal text (WCAG 1.4.3).
- Placeholders, auxiliary text, unselected navigation, and tooltips are all bound by text contrast requirements.
- Essential non-text graphics, states, and control-recognition cues at least 3:1; decorative dividers are not mechanically held to this (WCAG 1.4.11). "Essential" means the sole visual basis for recognizing the control — the weak-border allowance lives in `color.md`.
- Disabled content may dim, but users should still understand why; normal information may not lower contrast by claiming to be disabled.
- Test the final composited color across default, hover, pressed, selected, focus, and error fills.
- Body links are underlined by default; "blue slightly different from body text" alone doesn't say clickable.
- States and selection never rely on hue alone; use text, shape, checks, position, or other cues.

**Common misconceptions to correct:**

1. **14px/510 is not WCAG large bold text.** Large text is at least 18pt (24 CSS px), or at least 14pt (~18.67 CSS px) bold; 510 must not be treated as bold automatically.
2. **Placeholders are text**; the name doesn't exempt them from normal text contrast.
3. **Metadata, timestamps, and non-current navigation are neither "disabled" nor "decorative".** If it must be read, it's information text.
4. **44×44px is not a blanket WCAG 2.2 AA minimum.** AA 2.5.8 is 24×24 CSS px or its spacing exceptions; 44px is the touch default and a common higher-tier baseline.
5. **"No scrolling at 200% zoom" is not a completed Reflow check.** Also verify the 320 CSS px equivalent; necessary two-dimensional tables have local exceptions.

## Focus and semantics

- Every interactive element has visible focus; default is a 2px solid outline with 2px offset.
- Focus is never fully covered by sticky headers, modals, or bottom bars, and never clipped by containers.
- Selection, current position, active keyboard item, and focus are distinct concepts — not one blurry purple glow.
- Prefer platform-native semantics; custom controls must fully implement name, role, state, and keyboard behavior.
- Icon buttons have accessible names; repeated names gain object context, e.g., "Remove member Alex".
- Feedback uses appropriate live regions; don't announce every hover or every skeleton block.
- Modals manage background inertness and focus return; writing `aria-modal` alone implements none of it.

## Size, zoom, and touch

- Check web pointer targets against WCAG 2.2 AA 24×24px or its specific exceptions — not just visual size in a screenshot.
- Touch defaults to ≥44×44px; native platforms use their pt/dp and guidelines to reach at least equal usability.
- 200% font/page zoom loses no content or actions; also verify reflow at the 320 CSS px equivalent.
- Genuinely two-dimensional content (data tables, maps, timelines) may scroll locally; ordinary text and toolbars must still reflow.
- Never disable user zoom or auto-shrink font size after magnification to cancel the user's intent.
- No clipped content under text-spacing overrides, larger system fonts, or long localized copy.
- A minimum button height doesn't replace vertical padding and growable layouts.

## Motion, high contrast, and platforms

- Respect reduced-motion preferences; automatic animation must not hinder reading or operation.
- When supporting high contrast/forced colors, don't override system colors in ways that erase focus or check states.
- Don't trust "antialiasing toggles" for cross-platform weight consistency; test fonts on the target systems.
- Non-web platforms keep their own control semantics, back gestures, focus, IMEs, and system accessibility.
- Never treat CSS px as physical pixels on every platform; map to logical units and test.

## W3C source reference (for verification)

| Used for | W3C source |
|---|---|
| Normal text 4.5, large text 3, placeholders, disabled/decorative exceptions | [Understanding SC 1.4.3 — Contrast (Minimum)](https://www.w3.org/WAI/WCAG22/Understanding/contrast-minimum.html) |
| Essential control boundaries, states, and graphics 3:1 | [Understanding SC 1.4.11 — Non-text Contrast](https://www.w3.org/WAI/WCAG22/Understanding/non-text-contrast.html) |
| AA 24px targets and spacing exceptions | [Understanding SC 2.5.8 — Target Size (Minimum)](https://www.w3.org/WAI/WCAG22/Understanding/target-size-minimum.html) |
| 320px equivalent, zooming, two-dimensional content exceptions | [Understanding SC 1.4.10 — Reflow](https://www.w3.org/WAI/WCAG22/Understanding/reflow.html) |
| Character shortcut disabling, remapping, or focus scoping | [Understanding SC 2.1.4 — Character Key Shortcuts](https://www.w3.org/WAI/WCAG22/Understanding/character-key-shortcuts.html) |
| Modal focus management, background inertness, Esc, focus return | [APG — Dialog (Modal) Pattern](https://www.w3.org/WAI/ARIA/apg/patterns/dialog-modal/) |
