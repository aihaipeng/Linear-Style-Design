# Component Specification

## The shared component contract

Every interactive component must cover its applicable states — not just a default screenshot. A state is applicable when the component can visually or behaviorally enter it; static non-interactive elements need no hover/pressed/selected.

| State | Must convey | Default approach | Common mistake |
|---|---|---|---|
| default | What it is; whether it's operable | Clear text or icon; necessary boundary | Only looks clickable on hover |
| hover | Pointer position | Slight background/text change | Card lifting 8px; content wobbling |
| pressed | Actively pressed | Immediate fill change | Feedback waits for the network |
| focus-visible | Keyboard position | A clear 2px outline with 2px offset | Faint purple glow; no visible focus |
| selected / current | Selected object or current location | Neutral fill + check/indicator bar/distinct shape | Hue change only; indistinguishable from hover |
| disabled | Currently inoperable | Disabled semantics, restrained dimming, reason when needed | Just lower opacity yet still submits |
| pending | Request in flight | Stable size, local progress, no double submits | Full-page overlay or faked success |
| error | Failure and how to recover | Text, linked error, recovery action | Red border or a flash toast only |

**State priority:** disabled ignores interaction; pending prevents double submits; focus stays visible; selected survives hover/pressed; errors never cancel keyboard focus. States can stack.

## Buttons

| Variant | Default use | Visual |
|---|---|---|
| Primary | The main action of the current action group | Inverted fill; explicit verb copy |
| Accent | Main action in sign-in, onboarding, brand flows | `accent.fill` + `accent.onFill`; not a second primary button |
| Secondary | Secondary actions | Transparent/surface fill + default border + secondary text |
| Ghost | Toolbars, low-weight actions | No border; light fill on hover; text stays readable |
| Destructive | Destructive actions like delete or revoke | Danger text + explicit action name; a verified danger fill for strong confirmation |

- Default minimum height 32px; compact 28px; forms and comfortable (touch) 44px.
- Horizontal padding 12px; icons 16px with 8px text gap; radius 8px.
- Sized controls own their centering: `display: inline-flex; align-items: center; justify-content: center`, single-line labels at `line-height: 1` — the browser's built-in button centering is lost the moment `display` changes, and font metrics leave ghost space. Prefer padding-based heights over fixed height + magic line-height; give buttons and inputs the same vertical padding, font-size, line-height, and border-width so equal roles render equal heights.
- Asymmetric glyphs (play triangles, chevrons) get a 1–2px optical nudge; trim a few px of padding on the trailing-icon side.
- Font size 13px desktop / 14px Chinese; 44px form buttons may use 15–16px.
- Primary action names are specific — "Save changes", "Create project" — not every button labeled "Confirm" or "Submit".
- Ordinary hover does not move, scale, or pulse shadows; click feedback never waits for the network.
- Keep button width and accessible name while loading; use `aria-busy` when appropriate and block repeat submits.
- Icon-only buttons need accessible names; the glyph may be 16px, but the target must not be.
- Navigation uses links; actions use buttons; don't build the whole app from clickable divs.
- A disable reason must be reachable when it affects the task — not buried in a tooltip on a focus-proof disabled button.

## Inputs, selects, and forms

- Default min-height 32px; comfortable (touch) 44px; textareas grow with content.
- Background `bg.control`; required boundary `border.control`; text primary; placeholder muted.
- Values that can overflow — selects, combobox triggers, filenames — truncate with an ellipsis and keep the full value reachable through a visible, actionable path (details, expand, or the menu); `title`/`aria-label` are supplements, never the sole path (`typography.md`). Middle-truncate identifiers whose tail carries the identity when the row can't grow; wrap where there is room.
- Select menus: min-width ≥ trigger (and ≥ 2× control height), max-width capped (~28rem); overflowing options truncate inside the menu with full text reachable; long option lists get a max-height and scroll. A flex row needs `min-w-0` on the control or truncation never engages.
- Radius 8px; horizontal padding 12px; hints and errors sit next to the field.
- Labels are always visible in normal case; placeholders never replace them.
- Focus uses a clear ring; a border change may accompany it, but a low-opacity glow alone is not enough.
- Errors state the concrete problem and fix, and are linked to the field; user input survives failed requests.
- Explain required/format/length constraints at sensible moments; validation doesn't barrage every keystroke.
- Mobile inputs default to the 16px step; test the target browser's input zoom; never disable user zoom.
- Use content-appropriate input types, autocomplete, and keyboard hints; passwords support managers and paste.
- Keep the native picker when it does the job; custom styling doesn't mean rewriting every date/select/upload control.

**Checkboxes, radios, and switches:**

- Visual size 16–20px by default, with labels and containers enlarging the real target.
- Both the unchecked boundary and the checked check/dot must be recognizable; a 6% structural line cannot draw a nearly invisible checkbox.
- Checkboxes are for multi-select or independent choices, radios for mutually exclusive ones, switches for immediately effective settings; similar looks don't justify swapping semantics.
- Selection never rides on color fill alone; keep the check, dot, or thumb displacement.

## Navigation, tabs, and toolbars

**Sidebar:**

- Width 240px; rows min-height 28px; icons 16px; text 13/14px; group spacing 16–24px.
- Ordinary items may use muted; the current item uses primary; don't demote everything else to disabled.
- The current item uses a neutral selection fill plus a recognizable indicator bar, shape, or other non-hue cue, and is marked semantically as current.
- No colored icon tiles by default, no per-row borders; group labels aren't forced to uppercase.
- Collapsed navigation must stay comprehensible; tooltips are a supplement — mobile gets full names or expandable navigation.

**Tabs:**

- Tabs switch content panes within one view; navigating to standalone pages uses links instead of forced tab semantics.
- Height 32px; text 13/14px; selection uses either a fill shape or underline, and stays recognizable.
- Focus and selected are expressed separately; keyboard focus on a tab is not consent for expensive data loads.
- Close buttons must be discoverable under keyboard focus and touch — not hover-only.

**Toolbar:**

- Same-class actions keep stable positions across pages; hierarchy is expressed by position, spacing, and button variant.
- Show high-frequency actions by default; the rest go into a named overflow menu — not everything gets hidden.
- Sticky toolbars must not cover focused content; narrow screens may wrap or merge actions.

## Lists, tables, and kanban

- Default row min-height 36px; compact 28px; comfortable (touch) 44px; body text 13/14px.
- Organize by scanning order: status/type, identifier and title, key metadata, actions; exact fields follow the business.
- IDs, dates, and numbers never outshine titles; numeric columns use stable widths and tabular numbers.
- Numbers right-align by default; text start-aligns; columns match their headers.
- No zebra striping by default, no per-row card shadows; row separators appear only when they genuinely aid scanning.
- Hover uses a light fill; selected needs an explicit marker that hover styles cannot override.
- Sortable headers expose sort state and operable semantics; a decorative arrow alone is not enough.
- Secondary row actions may appear on hover/focus-within; touch and keyboard always have a reachable path.
- Long titles may truncate with a details/expand path; key identities, amounts, and errors never end at an ellipsis.
- Data tables preserve the header-cell relationship; ordinary tables aren't forced into complex ARIA grids for style.
- Inline buttons, links, and whole-row actions must not nest invalidly; clicking one button must not also open the row detail.
- Paginate or virtualize large lists only when needed; once virtualized, focus, screen-reader order, and search must survive.
- Drag reordering always has a non-drag alternative; drop targets are explicit, not just a floating shadow.
- Kanban keeps column semantics and readable titles; small screens may browse column-by-column or switch to a list — cards never shrink to untappable.

## Cards, panels, and inspectors

- Cards are containers for standalone objects; panels are work areas; don't turn every field into a card.
- surface by default + optional default border + 12px radius; padding 16/24px; no outer shadow on ordinary surfaces.
- Titles 16–20px medium or semibold; descriptions 13–15px secondary or muted.
- Static panels have no hover effects; only interactive objects get interaction feedback.
- When a whole card is clickable, provide a clear entry and keep inner actions' semantics and event boundaries separate.
- Inspector fields align consistently; scroll regions are clearly separated — no three tiny scroll boxes fighting over the wheel.
- No nested "matryoshka" cards repeating the same border, radius, and background.

## Menus, dropdowns, and context actions

- Container: raised + strong border + popover shadow; radius 12px; padding 4px.
- Menu items min-height 32px default, 28px compact, 44px comfortable (touch); item radius 6px.
- Icons are optional — not one per verb; shortcuts use a low-weight kbd on the right.
- Selection menus keep the check; destructive actions get their own group with text explanation.
- Defaults: 160ms in, 100ms out; high-frequency command menus may drop the transition.
- Support arrow keys, Enter, Esc, and sensible focus movement; semantics must match the actual menu or listbox behavior.
- Submenus need forgiving pointer paths; diagonal movement must not close them instantly.
- Context-menu entries need visible/keyboard/touch equivalents; right-click is never the only discovery path.
- Overlays stay in the viewport and keep pointing at the right trigger after scroll, zoom, and keyboard changes.

## Dialogs, drawers, and command menus

**Modal dialogs:**

- Default width 560px, capped at viewport minus 32px; radius 12px; padding 24px.
- Title 20/28; body 15/24; errors stay visible inside the dialog.
- Long content scrolls internally while close and key actions stay reachable; narrow screens may go full-screen.
- One task per dialog: the task-critical control gets the space; the title carries the main message; the body holds only what supports it. Header and footer persist while content scrolls; at most three footer actions, primary at the trailing edge. A dialog that needs a second dialog or a page's worth of content is a page.
- On open, move focus to the first focusable element or the dialog itself (focus the primary input when a single input is the task); the background is inert; Tab doesn't escape; on close, focus returns to the trigger or the element that follows it.
- Esc closes the topmost closable layer; unsaved data triggers a confirm-or-keep-draft flow, never silent loss.
- Destructive confirms name the object and consequence; default focus never invites an irreversible action.
- Non-modal inspectors don't set `aria-modal=true` by mistake or trap focus without reason.
- The 2px outline must not be clipped by the container's overflow; focus visibility beats tidy clipping.

**Command menu:**

- A high-value optional capability for complex productivity apps — not a component every marketing site or settings page must add.
- When enabled, provide a visible entry plus the `Cmd/Ctrl+K` hint, and check the host for shortcut conflicts.
- Default width 600px; top position around 15vh, with a capped max height that accommodates the on-screen keyboard.
- Input height 48px; result rows 36px; show command name, necessary context, and actually implemented shortcuts.
- Search, grouping, and empty-result explanations must be understandable; fuzzy search and recents come only when clearly worth it.
- Distinguish keyboard focus/active result from selection; Up/Down to choose, Enter to run, Esc to back out/close.
- Use the appropriate pattern (dialog + combobox/listbox) based on real interaction; don't slap roles onto static lists mechanically.
- Results triggering async or destructive actions still go through the usual permission, confirmation, and failure handling — coming from a command palette is no bypass.

## Widgets and system states

| Component | Default spec | Watch out for |
|---|---|---|
| Badge / Tag | Visual height 20–24px; 12px text; 4–6px or pill radius; neutral text + status dot | Enlarge the target when interactive; don't conflate category, status, and removable tags |
| Icons | 16px primary, 20px standalone actions; one icon family | Line icons by default, fills may express state; no blanket ban on filled icons |
| Tooltip | 12–13px; padding 6–8px; 8px radius; max 280px | Trigger on hover and focus; dismissible and hoverable; never the sole home of critical information |
| Toast | Max width 360px; 16px from edges; raised; 13/14px | Success toasts don't steal focus; errors may persist; actions must outlive a few seconds |
| Kbd | Mono 11–12px; visual height 20px; 4px radius | Only show real shortcuts; show platform-correct key names |
| Skeleton | Match the final layout; low-contrast static blocks, light pulse when needed | Decorations aren't announced one by one; the container announces busy; stop pulsing under reduced motion |
| Progress | Real progress when measurable; explicit in-progress when not | Never invent percentages; consider cancellation for long tasks |
| Empty | Short title + reason/note + fitting action | Distinguish first use, empty filter results, no permission, and load failure |
| Error state | Explicit failure scope + recovery action | Preserve user input; permission errors don't masquerade as empty data |

Icons, illustrations, and emoji are not blanket-banned: system action icons stay consistent, while user content, avatars, real brands, and genuinely clarifying material keep their expression.
