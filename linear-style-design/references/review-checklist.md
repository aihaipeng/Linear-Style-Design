# Acceptance, Anti-Patterns, and the Delivery Checklist

Treat this file as the core checklist for the "review an existing interface against Linear Style" workflow. For specific color/typography/component values, cross-check `color.md`, `typography.md`, `layout.md`, and `components.md`; for micro-detail counter-examples, cross-check `details.md`.

## Not looking Linear is usually not a missing purple glow

Levels: **Fail** = a defect in any context — report it as an issue. **Needs reason** = legitimate in some contexts — flag it, and it stands only with a stated reason; a Needs-reason finding that stands is recorded as a `linear-exception` comment where it lives (see "Verdict, caps, and exceptions" below) — the report alone is not enough.

| ID | Anti-pattern | Level | Problem | Replace with |
|---|---|---|---|---|
| AP-1 | Purple headlines everywhere, gradient buttons, colored borders | Fail | No attention hierarchy | A neutral base + one action focus |
| AP-2 | Everything wrapped in cards | Needs reason | Fragmented structure | Rows, groups, alignment; cards only for standalone objects, panels only for work areas (`components.md`) |
| AP-3 | All cards 24px radius with big shadows | Fail | Undifferentiated component roles | Radius by role (4–16px scale in `layout.md`); ordinary panels unshadowed |
| AP-4 | 12px low-contrast captions mistaken for refinement | Fail | Illegible, unusable | Conforming muted + caption/body roles from `typography.md` |
| AP-5 | Every heading 800 bold | Fail | Hierarchy reduced to shouting | Size, position, moderate 500/600, spacing |
| AP-6 | Forcing 510 on every language as the "signature weight" | Fail | Ignores fonts and platforms | Map to actually available weights and test |
| AP-7 | Only one colored element per screen | Fail | Focus/states go invisible | Brand contained; essential states intact |
| AP-8 | Scroll-in for every section, hover motion on every card | Needs reason | High-frequency interference | Fast local feedback; few purposeful motions |
| AP-9 | Optimistic "success" for everything async | Fail | Data and permission risk | Choose confirm/pending/rollback by risk |
| AP-10 | Every table becomes phone cards | Needs reason | Comparison lost | Summary view or locally scrollable table |
| AP-11 | Forced Cmd+K, shortcuts, and dynamic-theme bundle | Needs reason | Features added for style's sake | Add only when the task truly needs it |
| AP-12 | Unverified framework configs copied blind | Needs reason | Cross-stack in name, broken in practice | Stable semantic contract + per-version adaptation |
| AP-13 | Dialog header and secondary controls dominate while the task-critical control is cramped | Fail | Space contradicts the task | One task per dialog; the primary control gets the space (`components.md`) |
| AP-14 | A sized control whose label or icon sits off-center, hugging an edge | Fail | Browser-default sloppiness at the most-seen detail | Controls own their centering: inline-flex + `line-height: 1` on single-line text (`components.md`) |
| AP-15 | Long values or options clip mid-character or blow out the row, full text unreachable | Fail | Data unreadable exactly where it matters | Ellipsis + reachable full text; min/max widths (`components.md`) |

Removing anti-patterns is not enough. A page with every one of them deleted and nothing put in its place is an empty page with good manners — avoidance is itself a recognizable look. Fill the gap with real structure: alignment, grouping, spacing rhythm, stable density (`layout.md`), and content that only this product would have.

## Five questions for visual review

Run these as summary questions over the six render passes below (Q1 ≈ passes 1–2, Q5 ≈ pass 4); Q3–Q4 also need real content and state inspection, which the static passes don't cover.

1. Can the user locate the current object and the next step at a glance?
2. Does the structure get clearer when you remove a border, a color block, or a card layer?
3. Does the density fit real languages and input methods — not just English screenshots?
4. Do static, selected, focused, and error states still belong to one visual system?
5. Does the information structure still hold with glow, transitions, and product imagery removed?

## Critique the render, not the code

Code review reads intentions into the result — you (or the author) wrote it. A picture only shows what is there. Whenever the stack can produce one — a browser window, a device simulator, an exported design image — judge that, and treat code-only review as provisional. Make each pass separately, on the render:

1. **Thumbnail.** Shrink a screenshot until text is unreadable: the first thing you notice must be the current work object or primary information, not shell chrome, badges, or decoration (see `principles-and-evidence.md`).
2. **Five seconds.** Could a stranger say what the screen is for and what the next action is? A title that only makes sense after scrolling or opening a menu is not doing its job.
3. **Squint.** Blur until text is bars: the page should reduce to two or three masses with clear size differences. Many similar medium masses means nothing leads.
4. **Desaturate, cover the logo.** In grayscale, hierarchy must still hold (semantic and state colors excepted). With the name hidden, the screen should still show this product's task structure — not a template that would accept any logo.
5. **Edges.** Follow the left edge of every content block: a composed layout reuses two or three x positions on purpose; six means elements were placed one at a time.
6. **Both themes, three widths.** Look at light and dark, and at the narrow/regular/wide views the stack targets — many findings only exist at the edges.

Fix the top findings, re-render, then run the passes and the details sweep again — at most three rounds. Stop early when a round finds nothing a stranger would notice; if round three still has real problems, say so in the report instead of rounding up.

## Details sweep

Each round, after the six render passes, sweep once for micro-details — at actual size, then at 200% zoom — in fixed order: geometry and nesting → optical alignment → text behavior → scroll and overflow affordances → states → micro-consistency. Every entry is a counter-example with its fix in `details.md`. Sweep findings use the same levels as anti-patterns: matching an AP or a hard floor is Fail; the rest are Needs reason; purely cosmetic polish may be listed as an advisory without affecting the verdict.

## Definition of Done

**Visual & system:**

- [ ] Context, density, and theme scope chosen; not every page defaulted into a dense back office.
- [ ] Shared semantic tokens are the single source of truth; no stray non-semantic hex in components.
- [ ] Brand, system feedback, workflow state, and chart colors keep distinct responsibilities.
- [ ] Primary/secondary actions are clear; focus and states aren't hidden to satisfy "one colored element".
- [ ] Ordinary panels have no outer shadow by default; borders and radii are consistent by role.
- [ ] Mixed Latin/CJK, numerals, long titles, and long nav names don't break alignment.

**Interaction & accessibility:**

- [ ] Default / hover / pressed / selected / focus / disabled / pending / error covered where applicable.
- [ ] Text contrast verified on real composited backgrounds, including placeholders, errors, button hovers.
- [ ] Essential control boundaries and graphical states meet non-text contrast requirements.
- [ ] Keyboard completes main flows; focus never lost or covered; dialogs return focus sensibly on close.
- [ ] Character shortcuts (if implemented) can be disabled/remapped/focus-scoped; IME doesn't misfire.
- [ ] Touch doesn't depend on hover/right-click; target sizes and spacing pass; hit areas don't overlap.
- [ ] 200% zoom and 320px-equivalent reflow verified; necessary 2D areas scroll locally.
- [ ] Still operable under reduced motion and high-contrast/forced colors.
- [ ] Slow requests, failures, no-permission, and empty results are distinguishable; user input never lost.
- [ ] Optimistic updates have correct recovery; high-risk actions never fake success.

**Regression & delivery:**

- [ ] At least narrow, regular, and wide screens covered; short viewports and on-screen keyboards per target platform; render critiqued per "Critique the render, not the code" (up to three fix rounds, details sweep included).
- [ ] If dual themes are supported, all key component and overlay states checked in both.
- [ ] Visual walkthrough done with real content, not just short English and idealized data.
- [ ] Key interactions keep automated or repeatable manual regression steps.
- [ ] Baseline screenshots, approved exceptions, and reasons recorded; exceptions haven't grown into a second system.
- [ ] No misuse of Linear brand assets; this spec is never called the official Linear standard.

## Verdict, caps, and exceptions

Every review ends with exactly one verdict:

- **Conforms** — no Fail-level anti-pattern; every Needs-reason item is absent; all hard floors hold (text and non-text contrast, keyboard path, visible focus, reflow); checked on a render, not code alone.
- **Conforms with exceptions** — as above, with Needs-reason items standing and recorded as `linear-exception` comments. Exceptions can only cover Needs-reason findings — Fail-level anti-patterns and hard floors are never exceptable.
- **Needs work** — anything else, including any Needs-reason item present without a stated reason.

The Definition of Done drives delivery (Intent A); the verdict summarizes a review (Intent B). A conforming verdict presumes every DoD item relevant to this review passes; delivery-process items (baseline screenshots, regression steps) are out of scope for a review verdict.

Caps override the overall impression: any text- or non-text-contrast failure, invisible focus, missing keyboard path, or reflow failure forces "Needs work" however polished the rest is; a review performed on code alone never earns "Conforms". If your first verdict on your own work is "Conforms", re-check it as a reviewer who did not build it.

Exceptions are recorded where they live — `/* linear-exception: AP-8 — scroll-in on every marketing section */` or the platform's comment equivalent — so later reviews re-challenge them instead of rediscovering or silently inheriting them.

## Output structure for review reports

When reviewing an existing interface, report with this structure instead of reading the checklist aloud:

```
# [Page/Component] Linear Style Review

## Conclusion
Verdict: <Conforms / Conforms with exceptions / Needs work>
One or two sentences: the biggest problem

## Issues found (ordered by severity)
1. [Issue] — location — rule (AP-n from the table above, or file + section in color.md / typography.md / etc.) — suggested fix
2. ...

## What already conforms (optional, brief)

## Non-issues (optional)
Note what looks "un-Linear" but is an allowed context variation, to prevent over-correction
```
