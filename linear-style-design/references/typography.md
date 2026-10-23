# Fonts and Typography

## Font roles

| Role | Default | Rule |
|---|---|---|
| Latin UI and body | Inter Variable | One family is enough; no need to buy or load a Display family |
| Display headings | Same Inter Variable | Inter Display is allowed but not a cross-architecture dependency |
| Chinese / Japanese / Korean | A high-quality system or project font for the language | Measure glyph metrics, punctuation, and line height; Inter does not cover this body text |
| Code, IDs, shortcuts | `ui-monospace` monospace stack | Berkeley Mono is optional and separately licensed; not a required dependency |
| Brand-styled emphasis headings (e.g., system-level error/empty pages) | May swap Inter for a monospace family | [M] The Linear desktop client's offline fallback page titles use `Berkeley Mono`; a restrained exception — don't switch body headings to mono by default |
| Table numerals | UI font + tabular numbers | Don't convert whole tables to Mono just to align digits |

See `assets/linear-tokens.css` (`--ls-font-sans` / `--ls-font-mono`) for font stacks. Font names must match the family inside the actually loaded files; don't treat fallback names as proof of loading.

## Weight decisions

- Product baseline: **400 / 500 / 600** for reading / UI medium / emphasis.
- Website-faithful mode (marketing pages explicitly asked to replicate linear.app's look) may use **400 / 510 / 590**, but only with a variable font that actually supports those values.
- 450 is allowed for UI body text when a font runs thin on Windows; it must not spread into a fourth standard for all components.
- Regular UI headings don't default to 700/800; reach for size, position, color, and spacing first.
- Body `strong`, technical content, and readability-driven bold may use 700. Style rules never cancel semantic emphasis.
- Control weight with regular `font-weight`; never pin `font-variation-settings: "wght" 400` on an ancestor, overriding descendants.
- Non-variable fonts fall back to the nearest available step of 400/500/600; synthesized 510 is not a passing standard.

## Unified type scale

Sizes in px assume a 16px root; web implementations should prefer the rem equivalents (see `assets/linear-tokens.css`). Default UI never shrinks body text on narrow screens.

| Role | Size / line-height | Weight | Latin tracking | Typical use |
|---|---|---:|---|---|
| micro | 11 / 16 | 500 | 0 | Shortcut hints; never for core information |
| caption | 12 / 18 | 400–500 | 0 | Timestamps, auxiliary notes |
| ui | 13 / 20 | 400–500 | 0 | Desktop navigation, lists, buttons |
| ui-cjk | 14 / 21 | 400–500 | 0 | Default Chinese-UI step |
| body | 15 / 24 | 400 | 0 | Product body text, marketing copy |
| reading | 16 / 26 | 400 | 0 | Docs, settings forms, long-form text, mobile input |
| lead | 18 / 28 | 400 | `-0.01em` | Marketing lead-ins; not dense list body |
| heading-sm | 20 / 28 | 600 | `-0.012em` | Panel and dialog titles |
| heading-md | 24 / 32 | 600 | `-0.012em` | Page titles |
| heading-lg | 32 / 40 | 600 | `-0.02em` | Large section titles |
| display-sm | 48 / 52 | 500 | `-0.022em` | Marketing section titles |
| display | 64 / 64 | 500 | `-0.022em` | Desktop hero |

**CJK adjustments:** Chinese body text defaults to `letter-spacing: normal`; large headings default to at least 1.15 line-height — don't apply the Latin 64/64 tight ratio indiscriminately. In mixed-script text, check the baselines of punctuation, Latin IDs, and digits.

## Tracking and OpenType

- Large headings may use slight negative tracking; small sizes don't tighten by default, to avoid squeezing CJK and low-DPI text.
- `-0.011em` is an optional fitted value, not a requirement for all body text.
- Enable `cv01/ss03` only when the chosen font truly supports them and the result looks right; Latin-only is fine.
- `zero` is for technical data by default; avoid altering digit appearance in ordinary text.
- Group labels and table headers are not forced to all-caps; default to sentence case, and normal mixed casing in Chinese.
- Don't use "suddenly purple, italic, or gradient words" as the default heading emphasis.

## Line length and reading rhythm

- Latin long-form text defaults to ~`60–75ch`; stay under roughly 80 characters.
- Chinese reading columns default to ~`32–40` full-width characters; don't mistake `70ch` for 70 hanzi.
- Long text is left-aligned without forced justification; headings align left per context — centering is not a violation.
- Heading-to-description spacing 8–12px; paragraph spacing 12–16px; section spacing 24–40px.
- Headings may use balanced wrapping, but not hardcoded `<br>` breaks tied to one desktop width.
- List summaries may truncate, but full content must be reachable via details, expand, or another actionable path — never hover tooltips alone.
- Errors, critical terms, form labels, and primary buttons must never be irrecoverably truncated for visual tidiness.
