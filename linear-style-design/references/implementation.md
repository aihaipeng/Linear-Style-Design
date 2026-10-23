# Architecture-Agnostic Implementation Contract

## Three layers suffice

```text
Design decisions: color roles, type scale, density, state rules
    ↓
Shared semantic tokens: bg.surface / text.muted / focus.ring / action.primary.bg
    ↓
Platform & component implementation: existing CSS, theme objects, native resources, or design-tool variables
```

- Colors, text, borders, and action states are consumed through semantic tokens.
- Component recipes combine tokens and interaction behavior; no token per CSS declaration.
- No framework/library migration, theme engine, or state-management dependency is required.
- Reuse mature component behavior; adjust visuals and tokens as needed — don't rewrite accessibility foundations for looks.
- The CSS examples are not an architecture; any stack can implement the same semantics.

## Mapping per frontend type

| Environment | How to land it | What must not happen |
|---|---|---|
| Plain HTML / MPA / server templates | Shared stylesheet + root theme attribute | Unnecessary build tooling just for tokens |
| Any component-based web framework | Root semantic variables + existing component implementations | Each component defining its own dark/light hex |
| Utility CSS | Map semantic names to shared variables; configure per version in use | Copying one version's config as universally valid |
| CSS-in-JS / CSS Modules | Reference the same semantic variables or existing theme object | Two independently evolving color sets |
| SSR / streaming / static sites | Resolve the theme early; ship a consistent first paint; keep client state in sync | First-paint flash; hydration theme mismatch |
| Desktop web containers | Same tokens; handle native window safe areas and shortcuts | Faking window controls; overriding system keyboard behavior |
| Native mobile / desktop | Map roles onto native themes, fonts, and shape resources | Translating CSS line-by-line; unverified platform API names |
| Design tools | Same semantic names, theme modes, density, component states | Design and code each hand-maintaining separate palettes |

No framework-version-specific API or CLI code is provided; verify against your project's current version and target-platform docs.

## Single source of truth and theme boundaries

- Pick one maintainable token source per project; never hand-maintain CSS, JSON, and design-tool copies as rival authorities.
- Persist the adaptation: the brand-adapted token file plus the chosen context and density are the project's design source. Later UI tasks in the same project read that file first instead of re-deriving from this skill — per-page re-adaptation drifts into a second system.
- Small projects can use the CSS or existing theme file directly (see `assets/linear-tokens.css`); multi-platform projects export platform resources as needed.
- If you need JSON, store semantics, values, types, and units; colors/sizes must not float free of platform units.
- Theme scope must cover portals, tooltips, toasts, and root-level dialogs — not just a local page node.
- User input, permissions, and request logic are not skin; switching themes never resets data state.
- Data and business-state colors map independently; rebranding must not change what historical data states mean.

## Minimal implementation order

1. **Pick context and density:** productivity UI, marketing, or reading; define theme support scope.
2. **Lay down tokens:** surfaces, text, borders, actions, feedback, fonts, spacing, motion (copy `assets/linear-tokens.css` as the starting point, brand-adapted and persisted per "Single source of truth" above).
3. **Finish base components:** buttons, inputs, selects, navigation, lists — with full states (see `components.md`).
4. **Finish overlays:** focus, positioning, scrolling, and scrim for menus and dialogs; command menus only on demand.
5. **Feed it real content:** CJK, long names, empty data, no permission, slow requests, failures, destructive operations.
6. **Calibrate the visuals:** grays, alignment, radii, density; decoration comes last.
7. **Verify on the render** — the loop in SKILL.md Intent A step 6 — then the Definition of Done: dark/light, keyboard, touch, zoom, contrast, reduced motion, and key-flow regression (see `review-checklist.md`).

## CSS reference token conventions

`assets/linear-tokens.css` is a web reference matching this spec's defaults; it ships no font files, component library, or theme-switcher logic:

- Without an explicit theme it follows the system; without a system dark preference it falls back to light.
- Explicit themes set `data-theme="light"` or `data-theme="dark"` on `html` only; no nested local theming.
- Pages declare a correct `lang`; Chinese UI uses the 14px step; CJK doesn't inherit Latin negative tracking.
- Only tokens and a few labeled base recipes; no global reset, no rewriting default `body/h1/button` styles.
- The default system monospace suffices; projects must license and load Inter themselves, with fallback fonts on failure.
- Native platforms reuse the same semantic roles; no CSS required.

**Deliberately not in this CSS** (projects supply their own): full component behavior (button/menu/dialog interaction — presentational CSS is not a component), global resets / OpenType / antialiasing / `outline: none` / `font-variation-settings` overrides, fixed `height`, `0.01ms !important` animation kills, business-state and chart colors (they need a real domain model), and extra dependencies like theme generators.

For zero-build compatibility, the dark role values are duplicated in the system-dark branch. If your project has token tooling, generate from one source — never let the two copies evolve independently.
