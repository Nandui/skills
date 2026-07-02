# Design Foundations

The deeper rules behind the token table in SKILL.md. Values are given in
Tailwind utility names where possible, with raw CSS equivalents so the
guidance works in any stack.

## Contents

1. [Typography](#typography)
2. [Color](#color)
3. [Dark mode](#dark-mode)
4. [Spacing & alignment](#spacing--alignment)
5. [Depth: borders, shadows, radius](#depth-borders-shadows-radius)
6. [Motion](#motion)
7. [Interactive states](#interactive-states)
8. [Content & imagery](#content--imagery)

## Typography

**Scale.** Use a constrained modular scale and never step outside it.
Tailwind's default is well-chosen: 12 / 14 / 16 / 18 / 20 / 24 / 30 / 36 /
48 / 60px (`text-xs` … `text-6xl`). Needing a size between two steps is a
hierarchy problem, not a font-size problem.

**Body size.** 16px (`text-base`) for content sites and marketing; 14px
(`text-sm`) for dense applications (dashboards, admin tools) where users scan
rather than read. Never below 12px for anything users must read.

**Line height.** ~1.5 for body (`leading-normal`/`leading-relaxed`), tighter
as text gets bigger: headings at 1.1–1.2 (`leading-tight`). Large text with
body line-height looks like a word processor, not a product.

**Letter spacing.** Slightly negative on large headings (`tracking-tight` at
30px+) — large text naturally spaces itself out. Slightly positive on
ALL-CAPS labels (`tracking-wide uppercase text-xs font-medium`), which are
otherwise cramped.

**Line length.** 65–75 characters max for prose (`max-w-prose` or `max-w-2xl`).
Full-width paragraphs on desktop are the single most common readability bug.

**Weights.** Two or three per project: normal (400) for body, medium (500)
for emphasis/labels/buttons, semibold (600) for headings. Bold (700+) rarely;
light (<400) almost never on screens. Hierarchy = size + weight + color
together, so you rarely need extremes of any one.

**Hierarchy without size.** Before making text bigger, try: heavier weight,
darker color, or more surrounding space. Before making text smaller, try:
muted color (`text-zinc-500`-style). Pages with 2 sizes + 2 colors often beat
pages with 6 sizes.

**Fonts.** System stack (`font-sans` default) is always safe. One good
variable font (Inter, Geist, Source Sans, IBM Plex) elevates instantly —
self-host or use `font-display: swap`. Two families max: one UI/body + one
display or mono. **Tabular numbers** (`tabular-nums`) anywhere numbers stack:
tables, timers, prices, stats.

## Color

**Structure: one neutral family + one accent + semantics.**

- **Neutrals do ~90% of the work** — backgrounds, borders, text. Pick one
  family and stay in it: `slate` (cool/blue-ish), `zinc`/`gray` (neutral),
  `stone` (warm). Mixing gray families reads as slightly "off" without the
  viewer knowing why.
- **One accent** for primary actions, active states, links, focus rings. The
  brand color if there is one. Not indigo-by-default — indigo/violet is the
  strongest "Tailwind demo" tell; choose deliberately even if you land nearby.
- **Semantic colors** (green success, amber warning, red destructive) appear
  *only* when they mean something. A UI where green is decoration can't use
  green for success.

**Text on light backgrounds.** Near-black, not black: `text-zinc-900` (or
`slate-900`) on white/`zinc-50`. Three text levels are enough:
primary `…-900`, secondary `…-600`, disabled/hint `…-400`. Body paragraphs in
`…-400`-level gray are unreadable — muted is for metadata, not content.

**Contrast.** WCAG AA: 4.5:1 normal text, 3:1 large text (18px+/14px bold+)
and UI component boundaries. In Tailwind terms: on white, `…-500` grays fail
for body text; use `…-600`+. Check accent-on-white for buttons: many brand
colors need white text and a darkened hover, not the reverse.

**Backgrounds.** Off-white app backgrounds (`bg-zinc-50`/`bg-slate-50`) with
white cards create effortless surface separation. Pure-white-on-pure-white
needs borders everywhere to compensate.

**Tinted neutrals.** For a branded feel, pick the gray family nearest the
brand hue (blue brand → slate; warm brand → stone) rather than tinting
manually.

## Dark mode

Dark mode is a token swap, not an inversion:

- **Backgrounds**: dark gray, never pure black — base `zinc-950`, surfaces
  `zinc-900`, raised elements `zinc-800`. Elevation = *lighter* surface, since
  shadows are nearly invisible on dark.
- **Text**: off-white `zinc-50`/`zinc-100` primary, `zinc-400` muted. Pure
  white on dark glows harshly at body sizes.
- **Desaturate accents** one step (e.g. `blue-600` light → `blue-500` dark) —
  saturated colors vibrate on dark backgrounds.
- **Borders** get *more* important (shadows gone): `zinc-800` borders on
  `zinc-900` surfaces.

Mechanics: define semantic tokens (`--background`, `--surface`, `--border`,
`--muted`, `--foreground`, `--accent`) and swap values under `.dark` or
`prefers-color-scheme`. If components reference palette steps directly
(`bg-white`, `text-zinc-900`), dark mode means touching every file — this is
why semantic tokens exist. shadcn/ui's token names (`background`, `foreground`,
`card`, `muted`, `border`, `ring`…) are a solid convention to copy even
without shadcn.

## Spacing & alignment

**The 4px grid.** Every margin, padding, and gap is a multiple of 4px —
Tailwind's scale enforces this. Arbitrary values (`p-[13px]`) are almost
always a smell.

**Proximity is meaning.** Related items sit close; unrelated items sit far.
Concretely: space *within* a group ≤ half the space *between* groups. A label
belongs to its input (`gap-1.5`), form fields to each other (`gap-4`/`gap-6`),
sections to themselves (`gap-10`+). When grouping is ambiguous, users read
structure wrong before they read a single word.

**Consistent interior padding.** Pick one card/section padding (`p-4` dense,
`p-6` standard) and use it everywhere. Mixed `p-3`/`p-5`/`p-6` across sibling
cards is the most common consistency bug in real codebases.

**Generous outer spacing.** Section separation on pages: 40–96px
(`py-10`–`py-24`). When a layout feels cramped or cheap, double the
between-section space before touching anything else.

**Alignment.** Fewer alignment lines = calmer page. Left-align text and
headings in LTR interfaces; center only short, standalone elements (empty
states, confirmation dialogs, hero headlines ≤2 lines). Right-align numbers
in tables. Never center paragraphs.

**Use gap, not margins between siblings.** `flex`/`grid` + `gap-*` keeps
spacing owned by the container; margin-driven spacing leaks and breaks when
items reorder.

## Depth: borders, shadows, radius

**Pick one separator per surface: border, background shift, or shadow.**
Card on gray background → white bg alone may be enough. Card on white →
`border border-zinc-200` *or* `shadow-sm`, not both plus a bg change.

**Shadow scale by elevation, not decoration:**
- Resting cards: nothing or `shadow-sm`
- Raised interactive (dropdowns open, popovers): `shadow-md`/`shadow-lg`
- Floating overlays (dialogs, command palettes): `shadow-xl` + backdrop

`shadow-lg` on static content cards is a dated tell. Shadows say "this
floats above" — static content doesn't float.

**Borders**: 1px, low contrast (`border-zinc-200` light / `border-zinc-800`
dark). Borders at `…-300`+ on everything makes a wireframe, not a UI.
Divide lists with `divide-y divide-zinc-200` instead of per-item borders.

**Radius**: one scale, consistently — e.g. `rounded-md` (6px) inputs/buttons,
`rounded-lg` (8px) cards, `rounded-xl` (12px) modals/large surfaces,
`rounded-full` pills/avatars. **Nested radius rule**: inner radius = outer
radius − padding (a `rounded-xl` card with `p-2` contains `rounded-lg`
children), otherwise corners visibly clash. Uniform `rounded-2xl` on
everything is the current AI-output signature.

## Motion

Motion explains causality (this opened because you clicked that); it is not
decoration.

- **Durations**: 100–150ms micro-interactions (hover, toggles), 150–250ms
  panels/dropdowns/modals, 300ms+ only for large spatial moves. Over 400ms
  feels broken.
- **Easing**: `ease-out` for entering (fast start, settle), `ease-in` for
  exiting, `ease-in-out` for in-place morphs.
- **Transition specific properties** — `transition-colors`,
  `transition-opacity`, `transition-transform` — not `transition-all`, which
  animates layout accidentally and janks.
- **Animate cheap properties**: transform and opacity (GPU); avoid animating
  width/height/top/left where possible.
- **Respect `prefers-reduced-motion`**: gate non-essential animation with
  `motion-safe:`/`motion-reduce:` or a media query. Vestibular disorders are
  common; this is a one-line courtesy.
- Entering elements: subtle fade + 4–8px translate or 0.98 scale — not
  bounces, not spins, not staggered cascades on every list.

## Interactive states

Every interactive element ships with all of these, or it's unfinished:

| State | Pattern |
|---|---|
| Hover | One step: darken bg (`hover:bg-…-700` on filled; `hover:bg-zinc-100` on ghost), or underline links. Subtle — hover confirms, it doesn't celebrate. |
| Focus | `focus-visible:ring-2 focus-visible:ring-offset-2` in the accent color. `focus-visible` (not `focus`) keeps rings off mouse clicks while preserving keyboard visibility. Never `outline-none` without a replacement. |
| Active | Slightly darker than hover, or `active:scale-[0.98]` on buttons — the press should feel physical. |
| Disabled | `disabled:opacity-50 disabled:pointer-events-none` + `cursor-not-allowed` where useful. Keep disabled text ≥ 3:1 contrast when the label must stay readable. |
| Loading | Buttons: spinner replaces or joins label, width stable (don't let the button resize), element disabled during flight. Content: skeletons (see components.md). |
| Error | Red border + red helper text below + icon; never color alone. Announce with `aria-invalid` and `aria-describedby`. |

## Content & imagery

- **Design with real content**: actual names ("Aleksandra Wiśniewska", not
  "John Doe"), realistic numbers (4,382 not 42), long emails, empty lists.
  If the layout survives real data, it works; lorem hides every overflow bug.
- **Icons**: one library (Lucide, Heroicons, Phosphor), one stroke weight,
  sized 16px inline with text / 20px in buttons and nav. Icon + label beats
  icon alone everywhere except universally known glyphs (×, search, ⚙ is
  already borderline).
- **Avatars/images**: fixed aspect ratios (`aspect-square`, `aspect-video`) +
  `object-cover` so layouts don't reflow; always a fallback (initials on a
  colored bg for avatars).
- **Numbers**: `tabular-nums`, thousands separators, right-aligned in
  columns, unit shown once per column header not per cell.
