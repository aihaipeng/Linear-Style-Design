# Spacing, Density, and Layout

## Spacing scale

Default scale (see `--ls-space-*` in `assets/linear-tokens.css`):

```text
4 · 8 · 12 · 16 · 20 · 24 · 32 · 40 · 48 · 64 · 80 · 96 · 128
```

| Context | Default rhythm |
|---|---|
| Icon to text | 6–8px; consistent within a component type |
| Inside small widgets | 4/8/12px |
| Between form fields | 16/24px |
| Information groups | 16/24/32px |
| Content panel padding | 16/24px |
| Page gutter | Desktop 24px, narrow screens 16px |
| Marketing sections | Desktop 96px, narrow 48px |

6px is a fixed recipe for icon gaps and radii; that doesn't mean every pixel value belongs in the spacing table. Borders, glyph metrics, image ratios, and optical alignment don't need to be forced onto multiples of 4. The 2px step exists for micro gaps only — it is an allowance, not a rhythm step.

## Three density tiers

| Mode | Min row height | Min control height | UI text | Fits |
|---|---:|---:|---|---|
| compact | 28px | 28px | Latin 13px, Chinese 14px | Pointer/keyboard-driven navigation and data tools |
| default | 36px | 32px | 13–15px | Ordinary desktop work UI |
| comfortable | 44px | 44px | 15–16px | Forms, mobile, touch |

- Values are per-row minimums, not fixed heights that forbid content from growing.
- Pick default when no density requirement exists; pick comfortable for touch-critical actions.
- Never mix 28, 36, and 44px among sibling rows of the same role; rows with a different content role — expanded detail rows, nested note rows — are the exception.
- A compact sidebar, default main area, and comfortable settings form can coexist — they have different jobs.
- Don't fake 44px by stacking overlapping invisible hit areas over 28px rows; actually increase row spacing or switch to a touch layout.

## Productivity app shell

```text
┌─────────────────────┬────────────────────────────────────────────┐
│ Nav 240             │ App header min 44 (location row)           │
│ Groups, row align   ├────────────────────────────────────────────┤
│                     │ View toolbar (only with view controls)     │
│                     ├──────────────────────────┬─────────────────┤
│                     │ Main work area           │ Inspector 320   │
│                     │ Lists / tables / content │ (optional)      │
│                     │                          │                 │
└─────────────────────┴──────────────────────────┴─────────────────┘
```

- Left navigation defaults to 240px, adjustable 224–280px; long localized names should wrap or widen the column first.
- The location row answers "where am I"; the view toolbar answers "how is it shown, how is it filtered". Only split into two rows when the view toolbar carries its own controls (filter/sort/group) that don't fit one row at the narrowest supported width.
- The app header (location row plus optional view toolbar) is at least 44px tall; increase it for dense tooling, wrapping text, or touch.
- The inspector defaults to 320px; when the remaining main area gets too small, switch to a drawer instead of squeezing main content into a sliver.
- Lists, tables, and kanban default to full work-area width; settings and long-form text may cap width inside the main area.
- Same-class actions stay in the same place across pages; the existence of an overflow menu is not a license to relocate actions arbitrarily.
- Browsers don't fake desktop window buttons; desktop containers keep system drag regions, window buttons, and safe areas.

## Marketing layout

- Standard content container: `1200px`; large product imagery may use a `1344px` wide showcase container.
- Text blocks don't stretch indefinitely with the container; hero subtitles stay within reading width.
- Desktop nav defaults to 72px, 56–64px on narrow screens; when sticky, it must not cover focus or anchor headings.
- A hero is a large headline + concise value statement + one set of actions + real product visuals; don't force dashboards or hollow 3D scenes just to match the style.
- Left alignment suits product explanations; centering suits short copy with a single conversion. Choose per content — there is no rule that "Linear is always left-aligned".
- Organize grids by content; don't force every feature into identical cards.
- Ambient glow, perspective, blur, and video are all optional; the page must still work with them off.

## Responsive behavior

These are starting breakpoints, not an official device taxonomy; collapse earlier when main content no longer fits.

| Available width | Default behavior |
|---|---|
| ≥1280px | Nav + main area; show the inspector when space allows |
| 960–1279px | Nav + main; inspector becomes a drawer |
| 640–959px | Nav becomes a drawer; single-column main; secondary tools move into the overflow menu |
| <640px | Touch layout, 16px gutter, primary actions directly reachable; modals may go full-screen |

- Width is only a signal; also weigh language, zoom, input method, and window height.
- Don't shrink text after zoom just to "keep the desktop layout".
- On narrow screens, tables may become summary list + detail; data that must be compared horizontally may keep a locally scrollable table instead of losing semantics to cards.
- Local horizontal scrolling of tables, timelines, and canvases must not drag ordinary copy and page titles out of view.
- Use logical properties for edges, padding, and alignment; check icon, breadcrumb, and overlay direction in RTL.
- On mobile, check safe areas, the on-screen keyboard, landscape, and short viewports so bottom buttons aren't covered.

## Radii, borders, elevation, and materials

### Radius scale

| Token | Value | Default use |
|---|---:|---|
| xs | 4px | kbd, small tags, checkbox visuals |
| sm | 6px | Menu items, small controls |
| md | 8px | Buttons, inputs, nav rows |
| lg | 12px | Panels, menu containers, dialogs |
| xl | 16px | Large showcase containers, mobile sheets |
| full | 9999px | Avatars, pills, status dots |

- Buttons default to 8px; marketing CTAs, sign-in flows, and compact pill controls may use full.
- [M] Buttons on the Linear desktop client's offline fallback page use `5px`, not pills — confirming that pills are for marketing/sign-in/compact contexts while system-level functional buttons follow the rectangular radius scale.
- Uniformity is not "everything 12px"; what's uniform is the role relationships.
- Prefer concentric nesting: inner radius ≈ `max(outer − gap, 0)`; it's an optical guideline, not an equation every nested element must satisfy.
- Identical inner and outer radii with a visible gap usually look bloated — check for it.

### Borders and shadows

| Role | Surface | Border | Shadow |
|---|---|---|---|
| Canvas / body content | canvas | none by default | none |
| Panels / cards | surface | default when a boundary is needed | none by default |
| Menus / tooltips / toasts | raised | strong | `0 8px 24px rgba(0,0,0,.28)` dark-mode reference |
| Modals | raised | strong | `0 16px 48px rgba(0,0,0,.40)` dark-mode reference |
| Controls needing recognition | control | control | none by default; focus adds a clear ring |

- Structural lines default to 1 CSS px; don't confuse it with physical pixels.
- Don't force 0.5px globally; hairlines vanish or blur across DPRs, zoom levels, and browsers.
- Overlay shadows may be single- or multi-layer; judge by clarity, low noise, and performance — no mandatory "magic compound formula".
- Don't add inner highlights, outer glows, or drop shadows to static cards; without a clear purpose, none.
- The scrim is its own background layer; don't replace modal semantics and focus management with a giant `100vmax` box-shadow.

### Materials, imagery, and glow

- Product work surfaces are opaque by default; menus are opaque so they stay readable over any backdrop.
- Sticky marketing nav may use light backdrop blur, with an opaque fallback, and contrast checked over real content.
- iOS's in-house glass materials are not a web-wide hard requirement.
- Marketing pages allow one controlled ambient glow or an informative gradient — one carrying information (data, imagery), not gradient buttons (AP-1); "must emit purple light" is not a rule.
- Don't adopt "supposedly official hero-gradient parameters" as an identity marker without a specific version and reproducible evidence.
- Cursor-following spotlight is off by default; it's a showcase effect, not part of the working Linear UI.
- Product imagery shows real information structure and states — not meaningless fake code, random statistics, or blurry screenshots.
