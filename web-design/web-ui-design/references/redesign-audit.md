# Redesign Audit Process

How to analyze an existing UI before changing it. The audit does two jobs:
it finds what's actually wrong (not what's fashionable to change), and it
produces the evidence that makes your recommendations credible to the user.

## Step 1 — See it

- Read the relevant source: templates/components, global CSS, Tailwind
  config, design-token files.
- Run it if the project runs (dev server, or just open static HTML). Take
  screenshots at 375px and 1280px. If a project run-skill or browser tooling
  exists, use it. A rendered screenshot catches what code reading misses:
  actual contrast, actual crowding, actual imbalance.
- Note the stack while reading: Tailwind version, component library, CSS
  architecture. The redesign must speak the project's existing language.

## Step 2 — Inventory the system (or its absence)

Count distinct values in use. Grep/scan for:

| Inventory | How | Healthy | Symptom of no system |
|---|---|---|---|
| Font sizes | `text-*` classes or `font-size:` | 4–6 in use | 9+, or arbitrary `text-[13px]` |
| Font weights | `font-*` | 2–3 | 4+ |
| Gray shades | `zinc/gray/slate/stone-*`, hex grays | 1 family, 4–6 steps | Mixed families, 8+ near-duplicates |
| Accent colors | non-gray color classes | 1 + semantics | Several unrelated hues |
| Spacing values | `p-* m-* gap-*` | Consistent per context | `p-3`/`p-4`/`p-5` on sibling cards |
| Radii | `rounded-*` | 1 scale (2–3 steps) | 4+ different radii |
| Shadows | `shadow-*` | Elevation-based | `shadow-lg` on static content |
| Breakpoints | `sm: md: lg:` usage | Real behavior changes | None at all, or px-perfect chaos |

Also check for **interactive states**: search for `hover:`, `focus-visible:`
(or `:hover`, `:focus-visible`), `disabled:`. Their absence is a finding.
And **feedback states**: is there any empty-state, loading, or error UI?

## Step 3 — Judge against modern practice

Walk the rendered UI (or code) against this list, noting concrete instances:

- **Hierarchy**: Can you tell in one glance what the page is and what the
  primary action is? Multiple competing bold elements? Everything same size?
- **Contrast**: body text ≥4.5:1? Muted text used for actual content?
- **Spacing**: related items grouped tighter than unrelated? Sections
  breathing or wall-to-wall? Consistent card padding?
- **Alignment**: how many alignment lines? Centered paragraphs? Ragged
  label/input relationships?
- **Density**: appropriate for the audience (data tool vs. consumer)?
- **Depth**: borders+shadows+backgrounds all fighting? Dated heavy shadows,
  gradients, bevels?
- **States**: focus visible? hover present? loading/empty/error designed?
- **Responsive**: usable at 375px? tables/nav adapt or just squish? touch
  targets ≥44px?
- **Accessibility quick pass**: semantic elements or div-soup? labels on
  inputs? heading order? keyboard path through the primary flow?
- **Consistency**: do sibling screens/components make the same choices?

## Step 4 — Rank findings by visual payoff

Typical impact order (deviate when evidence says otherwise):

1. **Spacing & alignment fixes** — biggest perceived-quality jump per line
   of code
2. **Hierarchy** — one clear primary action, muted secondary text, heading
   scale
3. **Color discipline** — collapse to one gray family + one accent;
   fix contrast failures
4. **Missing states** — focus rings, hovers, empty/loading states
5. **Depth cleanup** — remove shadow/border/gradient noise
6. **Responsive repairs** — nav, tables, touch targets
7. Typography upgrade (font swap) — high impact but do it *after* the system
   exists, or it just makes inconsistency prettier

## Step 5 — Present findings, then ask before building

Present 3–5 findings in plain language with a concrete example each
("Sibling cards use p-3, p-4 and p-5 — the grid feels subtly untidy even
though no single card looks wrong"). Screenshots with annotations if you
rendered it.

Then ask (one round):
- What's in scope — this screen, this flow, or app-wide?
- What must not change? (brand colors, layouts users depend on, anything
  stakeholders are attached to)
- Looks-only, or may behavior/markup change too (affects tests, selectors,
  analytics)?
- Timeline pressure — polish pass vs. proper token refactor?

Skipping this step and delivering a surprise full rewrite is the most common
way redesigns get rejected wholesale.

## Step 6 — Implement system-first

- Define/repair tokens first (Tailwind config, CSS variables), then sweep
  components to adopt them. Token-first means every later fix is a one-line
  change.
- **Functionality is sacred**: every field, action, link, and data point in
  the original exists in the redesign unless removal was agreed. Diff your
  end state against the original's capabilities, not its markup.
- Keep a mapping of old → new values (e.g. "all `gray-*` → `zinc-*`",
  "`p-3/p-5` → `p-4`") and apply it mechanically — mechanical consistency is
  the point.
- Work in review-able slices on real codebases: tokens → shell/nav → highest
  -traffic screen → the rest.
- Re-render and compare before/after screenshots at 375px and 1280px. Check
  the checklist in SKILL.md's Verification section before declaring done.

## Common findings → standard fixes

| Finding | Fix |
|---|---|
| 8 grays across 2 families | One family, 4–6 steps, mapped mechanically |
| No focus styles / `outline: none` | `focus-visible:ring-2 ring-offset-2` accent ring everywhere interactive |
| Placeholder-as-label forms | Visible labels above, placeholders for examples only |
| `shadow-lg` static cards | `border` or `shadow-sm`; reserve big shadows for overlays |
| Buttons of 5 heights | `h-9` scale with variants (primary/secondary/ghost/destructive) |
| Centered long text | Left-align; `max-w-prose` |
| Table squished on mobile | Stacked cards or h-scroll + pinned first column |
| Pure black text | `zinc-900`-equivalent, muted steps for secondary |
| Rainbow section colors | Neutrals + one accent; semantics only for meaning |
| No empty/loading states | Skeletons + designed empty state per data surface |
| `100vh` mobile bugs | `min-h-dvh` |
| Gradient/emoji decoration | Replace with restraint: neutral surfaces, one accent, real icons |
