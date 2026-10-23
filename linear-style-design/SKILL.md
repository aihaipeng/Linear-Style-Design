---
name: linear-style-design
description: Linear-style design specification for any frontend architecture (web, SSR, native apps, design tools). Use when the user asks for a "Linear style" or "like Linear", needs to create or rework UIs, marketing pages, design systems, component libraries, or theme tokens, or wants to audit existing interfaces for conformance with the Linear design language.
license: MIT
metadata:
  version: "1.0.0"
---

# Linear Style Design

This is a cross-architecture Linear-style design specification based on official articles, measurements of public pages, and WCAG contrast verification. For the evidence boundaries behind each rule, see `references/principles-and-evidence.md`.

## Determine the intent first

**Intent A: Generate/implement new interfaces or components** (the user wants a Linear-style UI, page, component library, or theme)

1. Read "Core design DNA" and "Most-used defaults" below.
2. Plan before building: a compact plan — context type and density, the handful of core token values, type roles, and an ASCII layout sketch. Critique the plan once: anything you would produce the same way for any similar brief is a default rather than a choice — cut or relabel that part so the plan states only actual choices, before writing code.
3. Copy `assets/linear-tokens.css` as the token starting point and adapt it to your brand color and fonts (after replacing the brand color, re-check contrast for all states; see `references/color.md`). Persist the adapted tokens and the context/density choice in the project; every later UI task reads them first instead of re-deriving from this skill (see `references/implementation.md`).
4. Read the reference file for each topic involved (see the directory below) — only the parts the current task needs.
5. Follow the three-layer structure and the minimal implementation order in `references/implementation.md` (steps 1–2 there are already done above; continue from its step 3); don't introduce unnecessary build tooling or dependencies just to "look like Linear".
6. Verify on the render, not the code alone: screenshot or open the result at the widths and render targets the stack offers (browser windows, device simulators, design canvases), run the passes in `references/review-checklist.md` ("Critique the render, not the code") plus the micro-details sweep in `references/details.md`, fix, and repeat — at most three rounds — then self-check the Definition of Done.

**Intent B: Review/diagnose whether an existing interface matches the Linear style**

1. Read `references/review-checklist.md` directly: the anti-pattern table, the render-critique passes and details sweep, the five review questions, and the output structure. Judge screenshots or a live render of the real interface wherever the stack can produce one — reviewing code alone reads intentions into the result.
2. When you find a specific issue, cross-check the relevant topic file (color → `color.md`, typography → `typography.md`, layout/spacing → `layout.md`, component states → `components.md`, motion/accessibility → `motion-a11y.md`, micro-details → `details.md`) to verify the rule; never assert from memory.
3. Report using the "output structure" at the end of `review-checklist.md`, separating real problems from variations this spec allows — not every difference from a Linear screenshot is a defect; see "Three context types" below.

The two intents can alternate within one task (diagnose first, then fix); consult files back and forth as needed.

## Core design DNA (applies to both intents)

**One-line definition:** Linear Style is task-centric — near-neutral surfaces, clear and restrained typography, stable information density, precise alignment, and fast, predictable interaction reduce cognitive load during sustained work. It is not "black background + purple glow + Inter", and it is not lowering contrast across the page.

| Dimension | Requirement |
|---|---|
| Attention | Content before the app shell; navigation and toolbars never continuously compete for attention |
| Color | Near-neutral colors as the base; one brand accent family; semantic and data colors kept separate |
| Hierarchy | Establish relationships primarily through alignment, spacing, type size, moderate weight, and lightness |
| Structure | Grouping and whitespace before dividers; don't slice all information into cards |
| Density | Choose density by input method and task; compact desktop ≠ shrunk-down mobile |
| Shape | Small-to-medium corner radii; no outer shadow on ordinary surfaces; overlays get overlay shadows |
| Feedback | Fast local response, unambiguous states, stable layout, recoverable failures |
| Operation | Keyboard and pointer both complete tasks; shortcuts are accelerators, not the only path |
| Accessibility | Essential information stays legible; focus, states, and control boundaries never disappear for "calm" |
| Consistency | Components consume semantic tokens; theme changes don't require rewriting components |

For the judgment methods behind these principles, see `references/principles-and-evidence.md`.

## Three context types — don't conflate them

The same principles resolve to different concrete values per context; never apply the compactness of a productivity tool screenshot directly to marketing or reading contexts:

| Context | Primary goal | Inherit | Don't copy verbatim |
|---|---|---|---|
| Productivity tool UI | Sustained, high-frequency, accurate task completion | Compact layout, subdued shell, stable lists, keyboard efficiency | Marketing-scale type, ambient glow, long entrance animations |
| Marketing / brand pages | Explain value, showcase the product, drive action | Typographic discipline, real product imagery, restrained color | 28px rows and 13px dense body text |
| Docs / settings / mobile reading | Understanding information, accurate input | Clear structure, semantic color, readable body text | Compressing text and touch targets to look "Linear" |

Before any implementation or review, identify which context the task belongs to — it determines which set of defaults applies. Context is decided per page or area, not per app: a settings form inside a productivity app follows the reading-context defaults for its body text and comfortable density (see `references/layout.md`).

## Most-used defaults

The highest-frequency values; no need to open the reference files for these:

| Item | Default | Context variation |
|---|---|---|
| Dark canvas / surface / raised (overlays) | `#09090a` / `#121213` / `#1b1b1c` | "Overlays" = menus, dialogs, tooltips — token `bg.raised`; the whole set may shift to cool grays; never mix two neutral grays piecemeal |
| Light canvas / surface / raised (overlays) | `#f9f9fa` / `#ffffff` / `#ffffff` | Light is not a value-by-value inversion of dark |
| Brand accent | `#5e6ad2` | Replaceable; after replacing, re-check contrast for all states |
| Primary action button | Productivity UI defaults to inverted; sign-in/brand flows may use the accent | Stay consistent within one action group — all primary buttons in one flow look alike |
| Fonts | Inter Variable (Latin) + local script fallbacks; monospace stack for code/IDs | CJK requires measuring glyph metrics and line height |
| Weights | 400 / 500 / 600 (reading / UI / emphasis) | Website-faithful mode (marketing pages explicitly asked to replicate linear.app's look) may use 400/510/590 if the variable font is actually loaded |
| UI text | Desktop 13px, Chinese 14px; body 15px | Reading areas 16px |
| Spacing scale | 4px base scale (4/8/12/16/20/24/32/40/48/64/80/96/128) | 2px for micro gaps only — an allowance, not a scale step |
| Control height | Default 32px, compact 28px, comfortable (touch) ≥44px | Don't mix within one list of rows with the same content role |
| Sidebar / app header | 240px / min 44px | Adjust for content and language |
| Corner radius | Buttons/inputs 8px, menu items 6px, panels/dialogs 12px; pill only for marketing CTAs/sign-in/compact controls | Don't unify globally into one value |
| Motion | Hover 100ms, menus 160ms, dialogs 200ms; easing `cubic-bezier(.25,.46,.45,.94)` | High-frequency hover and command menus may use 0ms |
| Text contrast | Normal text ≥4.5:1, essential graphics/control boundaries ≥3:1 | Check the final composited color, not just the literal rgba |

The full token table (with CSS variable names) is in `assets/linear-tokens.css`; copy it directly into your project.

## Reference file directory

Read on demand; not all at once:

| File | Contents | When to read |
|---|---|---|
| `references/principles-and-evidence.md` | Design philosophy and judgment methods, context-type boundaries, evidence labels, known ambiguities | When you need the "why", or must explain a rule's basis to the user |
| `references/color.md` | Full semantic color tokens, surface rules, primary action/accent colors, system feedback colors, theming strategy | For color, theme, or light/dark work |
| `references/typography.md` | Font roles, weight decisions, the full type scale, tracking, line length | For typography, sizes, or mixed Latin/CJK typesetting |
| `references/layout.md` | Spacing scale, density tiers, app shell layout, marketing layout, responsive breakpoints, radius/border/shadow/material | For page structure, layout, responsiveness, radii and shadows |
| `references/components.md` | Component state contract; full specs for buttons/inputs/navigation/lists/cards/menus/dialogs/widgets | When implementing or reviewing concrete components |
| `references/motion-a11y.md` | Motion timing, loading feedback, optimistic-update boundaries, keyboard shortcuts, contrast and focus, sizing and zoom, W3C sources | For motion, loading, keyboard interaction, or accessibility review |
| `references/implementation.md` | Three-layer token architecture, per-framework mapping, single source of truth, minimal implementation order, CSS reference conventions | When setting up a project token system or deciding the technical approach |
| `references/details.md` | Micro-detail counter-examples: geometry/nesting, optical alignment, text behavior, scroll affordances, states, micro-consistency | During the fix rounds of render critique, and for polish work |
| `references/review-checklist.md` | Anti-pattern table (Fail / Needs reason), five review questions, six render-critique passes, details sweep, Definition of Done, verdict rules, review report output structure | The main entry point for the review/diagnosis workflow (Intent B) |

## Important boundaries

- This is not the official Linear design system: no replication of their logo, wordmark, or proprietary graphics, and never present it as the "official Linear standard".
- Unless labeled "Official"/"Measured", all values are choices made by this specification, not claims that "all of Linear is necessarily like this"; when something differs from a screenshot, first determine whether it's an allowed context variation.
