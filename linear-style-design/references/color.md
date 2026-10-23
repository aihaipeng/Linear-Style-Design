# Color, Themes, and Semantic Tokens

## Four responsibilities; never mix them

| System | Role | Example tokens | Never mix |
|---|---|---|---|
| Neutral base | Canvas, surfaces, text, borders | `bg.canvas`, `text.muted` | Don't replace structural grays with brand purple |
| Brand & interaction | Brand actions, links, focus | `accent.fill`, `text.link`, `focus.ring` | Don't reuse a button fill as the text-link color |
| System feedback | Success, warning, error, info | `feedback.danger.fg` | Not the same as your project's workflow Done / In Progress |
| Business & data | Statuses, priorities, categories, charts | `workflow.review`, `chart.series.1` | Never overrides system errors, focus, or brand actions |

**Naming: name by purpose, not by appearance.** `text.muted` beats `gray.500`; `action.primary.bg` beats a vague `primary`.

## Unified surface and text tokens

| Semantic name | CSS variable | Dark | Light | Use |
|---|---|---|---|---|
| `bg.canvas` | `--ls-bg-canvas` | `#09090a` | `#f9f9fa` | Application root canvas |
| `bg.surface` | `--ls-bg-surface` | `#121213` | `#ffffff` | Work surfaces, panels |
| `bg.subtle` | `--ls-bg-subtle` | `#161617` | `#f3f4f5` | Secondary regions; does not imply elevation |
| `bg.raised` | `--ls-bg-raised` | `#1b1b1c` | `#ffffff` | Menus, dialogs, tooltips |
| `bg.control` | `--ls-bg-control` | `#1b1b1c` | `#ffffff` | Base fill for form controls |
| `state.hover` | `--ls-state-hover` | White 4% | Black 4% | Hover, layered over the existing surface |
| `state.pressed` | `--ls-state-pressed` | White 7% | Black 7% | Pressed feedback |
| `state.selected` | `--ls-state-selected` | White 6% | Black 6% | Neutral selection fill; still needs a selection indicator |
| `accent.subtle` | `--ls-accent-subtle` | `rgba(94,106,210,.14)` | `rgba(94,106,210,.10)` | Optional brand selection fill; never the sole carrier of selection |
| `text.primary` | `--ls-text-primary` | `#f7f8f8` | `#1b1b1b` | Headings, current content, input values |
| `text.secondary` | `--ls-text-secondary` | `#d0d6e0` | `#5b5c5e` | Body text, secondary descriptions |
| `text.muted` | `--ls-text-muted` | `#8a8f98` | `#68696f` | Auxiliary text that must stay readable; placeholders |
| `text.disabled` | `--ls-text-disabled` | `#62666d` | `#9c9c9e` | Genuinely inoperable disabled content |
| `text.link` | `--ls-text-link` | `#828fff` | `#4f5bc4` | Links; underlined by default inside body text |
| `border.subtle` | `--ls-border-subtle` | White 6% | Black 6% | Non-critical grouping lines |
| `border.default` | `--ls-border-default` | White 9% | `#e2e2e5` | Ordinary container outlines |
| `border.strong` | `--ls-border-strong` | White 14% | `#d2d2d6` | Overlay structural lines; not an accessibility guarantee |
| `border.control` | `--ls-border-control` | `#777980` | `#82838a` | Input/select boundaries that must be recognizable |
| `focus.ring` | `--ls-focus-ring` | `#828fff` | `#4f5bc4` | Visible keyboard focus |
| `overlay.scrim` | `--ls-overlay-scrim` | Black 60% | Black 35% | Modal backdrop scrim |

**Three tiers of readable text are enough.** Don't add a fourth "ultra-faint" tier for refinement; disabled text is not a fourth body tier.

## Surface rules

1. In dark mode, overlays are usually slightly lighter than their backdrop; nested containers are not required to step up in lightness every level.
2. In light mode, overlays may match the white work surface; boundaries, scrim, and shadow carry the elevation — it is not "the higher, the grayer".
3. Sidebars match the canvas or stay low-presence; there is no rule that "the sidebar must be darker than the canvas".
4. `bg.subtle` is a regional purpose, not physical elevation. Don't rank every token into one forced ladder.
5. Semi-transparent state colors must be checked composited over the underlying surface; never judge by the literal rgba.
6. `state.hover` is an overlay color; painting it over an opaque background lets transparent regions show through. Use a background layer or pre-compose the color.
7. Calibrate text and states independently for each theme; swapping backgrounds alone is not enough.
8. `text.muted` passing on the base surface doesn't guarantee it passes over a selected/pressed fill. On neutral pressed/selected fills, this spec raises readable text to at least `text.secondary` while keeping focus and selection markers.
9. `state.selected` opacity must differ from `state.pressed` and `state.hover`; identical values let selection get swallowed by hover/pressed effects, contradicting the state-priority requirements (see the state contract in `components.md`).

## Primary actions and accent colors

| Role | Dark default | Light default |
|---|---|---|
| `action.primary.bg` | `#e5e5e6` | `#1b1b1b` |
| `action.primary.fg` | `#09090a` | `#ffffff` |
| `action.primary.hover` | `#ffffff` | `#303033` |
| `accent.fill` | `#5e6ad2` | `#5e6ad2` |
| `accent.fillHover` | `#5661c4` | `#5661c4` |
| `accent.fillPressed` | `#4f5bc4` | `#4f5bc4` |
| `accent.onFill` | `#ffffff` | `#ffffff` |

- Productivity products default to an inverted primary action to reduce large blocks of brand color.
- Sign-in, onboarding, and key brand-conversion flows may use an accent primary action; keep it consistent within the flow.
- Accent-button hover is not forced to "lighten in dark mode"; white-text contrast comes first — this spec chooses a slightly darker hover.
- One primary action per action group by default; while a modal is open on desktop, the background page's primary button is not a competing action because the background is not interactive.
- Focus, link, and selected states can coexist; never hide any of them for the sake of "one colored element".
- No hard 2%/5% pixel-budget thresholds; judge by visual weight and task priority.

## System feedback and business states

| System meaning | Dark foreground | Light foreground | Use |
|---|---|---|---|
| success | `#27a644` | `#198038` | Save confirmed, validation passed |
| warning | `#f2c94c` | `#8a6500` | Risk, needs attention |
| danger | `#eb5757` | `#c03434` | Errors, destructive operations |
| info | `#8fa6ff` | `#4f5bc4` | Neutral informational notices |

- Feedback text may use the matching foreground directly, but re-check it against its actual background; don't assume every 12% tinted fill passes.
- Badges default to neutral text + a status dot/icon; add a low-opacity fill only when stronger feedback is needed.
- Workflow states must be named and mapped by business meaning: `workflow.todo / active / review / done / canceled`.
- Done is not required to be green or indigo, and In Progress is not required to be green or yellow — business-state colors and system feedback colors are two independent mappings.
- Keep text, shape, icons, or other recognizable cues beyond color.
- Charts may use more than one color; keep mappings stable, use legends and labels, add line styles or textures when necessary, and don't dye every series brand color.

## Accessible thin lines ≠ every border is faint

Weak container lines may fall below 3:1 because they are not the sole visual basis for recognizing controls. **But text-input borders, unchecked checkboxes, switch tracks, and important status indicators must not rely on a 6% white border alone — each needs `border.control` or another boundary/background that meets the recognition requirement.**

- Inputs default to `border.control`, or provide another boundary/background that meets recognition requirements.
- Buttons with clear text or icons may use weak borders; that does not generalize to every form control.
- The focus ring must be a clear, substantive indicator; a 25%-transparent diffuse glow is supplementary only.
- High-contrast modes may strengthen boundaries; "more visible" does not violate Linear Style.

## Theming strategy and generators

Linear officially generates themes in LCH from three inputs — base, accent, contrast — instead of hand-setting 98 variables per theme. [O]

**Implementation defaults:**

1. Start with static light/dark semantic tokens; don't build a 98-color generator to imitate Linear.
2. If the product needs only one theme, implement only that one; once you claim dual-theme support, every state must be verified in both.
3. Theme priority: explicit user choice > follow system > product fallback. System theme changes never override an explicit user choice.
4. Theming must cover overlays, native form colors, scroll areas, and embedded regions — not just content cards.
5. Re-branding must replace the brand fill, its foreground, links, focus, subtle fills, and hovers — not just one hex.

**If dynamic theming is truly required:**

- Perceptual color spaces improve derivation consistency but do not replace contrast testing.
- CIELCH and OKLCH have different numeric scales; `[12,2,270]` or `[55,80,300]` cannot be pasted into `oklch()` as-is.
- Out-of-gamut colors must be mapped; check contrast with the final output color.
- The `contrast` input does not automatically equal WCAG conformance; high-contrast modes still need verification.
- Reverse-engineered parameters are research leads only, not the "official generation formula".
