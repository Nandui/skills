---
name: web-ui-design
description: >-
  Design, build, or redesign modern, polished web app interfaces grounded in
  current UI/UX practice — from a single component to a full page to an entire
  project. Use whenever the user asks to design, style, restyle, modernize,
  polish, or improve the look and feel of any web UI; says an interface looks
  dated, ugly, unprofessional, or generic; or asks for a landing page,
  dashboard, settings screen, form, data table, navigation, or app shell.
  Works with Tailwind CSS, Base UI, shadcn/ui, or plain HTML/CSS in any
  framework, and covers desktop, tablet, and mobile. Trigger even when the
  user never says "design" — "make this page look better", "build a pricing
  page", "the signup flow feels clunky" all qualify.
---

# Web UI Design

Produce interfaces that look professionally designed, not generated. The
difference is rarely talent — it is **consistency, hierarchy, and restraint**:
a small set of deliberate choices (one type scale, one spacing rhythm, one
accent color, one radius scale) applied everywhere without exception. Most
"dated" or "AI-looking" UIs fail not because any single element is bad, but
because every element makes its own choices.

Paths in this file are relative to this skill directory.

## Step 0 — Establish scope and mode before touching anything

Answer two questions from the user's request and the codebase:

1. **Scope** — a component, a page, or the whole project? Match your blast
   radius to it. A "make this button nicer" request does not justify a
   project-wide restyle; a "modernize our app" request does justify defining
   shared tokens first.
2. **Mode** — designing **from scratch**, or **redesigning something that
   exists**? These have opposite first moves:
   - From scratch → interview the user (below). You cannot design well for a
     product you don't understand.
   - Redesign → audit the existing UI first (below). You cannot fix what you
     haven't diagnosed, and users have invisible constraints (brand, legacy
     screens, stakeholder attachments) that an audit conversation surfaces.

Never skip straight to writing markup on a project-scope request. The token
decisions you make in the first ten minutes determine whether the result
holds together.

## Mode A — From scratch: interview first

Ask **one round of 3–6 questions**, only the ones the user hasn't already
answered. More rounds feel like interrogation; zero rounds produce generic
output that fits nobody. Cover whichever of these are still unknown:

- **Product & audience** — what is it, who uses it, in what situation
  (at a desk? on a phone in a hurry?). This drives density and device priority.
- **Personality** — offer spectra, not open-ended prompts: playful ↔ serious,
  warm ↔ technical, minimal ↔ expressive. Two or three words are enough to
  anchor every later decision.
- **References** — 1–2 existing apps or sites whose look they admire. This is
  the highest-signal question; a reference resolves a hundred small choices.
- **Brand constraints** — existing colors, logo, fonts that must survive.
- **Density** — spacious marketing feel vs. dense data-tool feel.
- **Dark mode** — needed now, later, or never? (Building tokens dark-mode-ready
  costs nothing now and a rewrite later.)

If the user is unavailable (batch run, one-shot request) or has already given
enough direction: **state your assumptions explicitly at the top of your
response** ("Assuming: internal tool, information-dense, desktop-first,
neutral-professional tone") and proceed. Stated assumptions invite cheap
corrections; silent assumptions produce expensive rework.

## Mode B — Redesign: audit before proposing anything

Read the actual code (and render/screenshot it if the project runs — see
Verification below). Then inventory, in a scratch file or your head:

- Distinct **font sizes and weights** in use
- Distinct **colors** — count the grays especially; 7+ near-identical grays is
  the classic symptom of no system
- Distinct **spacing values**, **border radii**, **shadows**
- Interactive **states** present vs. missing (hover, focus-visible, disabled,
  loading, empty, error)
- **Responsive behavior** — what actually changes at each breakpoint

Follow `references/redesign-audit.md` for the full process and a
findings → fix mapping. Then, before implementing:

1. Present the 3–5 highest-impact findings in plain language, ranked by
   visual payoff (usually: inconsistent spacing, weak hierarchy, missing
   states, low contrast, dated depth effects).
2. Ask what's in scope, what must not change (brand, layouts users rely on),
   and whether behavior changes are acceptable or looks-only.
3. Only then propose and implement.

Preserve all existing functionality unless removal is explicitly agreed —
every field, link, and action in the original must exist in the redesign.

## Foundations — define tokens before components

Whatever the scope, ground the work in a small token set. For component-scope
work in an existing project, **derive tokens from what's already there**
(read the Tailwind config, CSS custom properties, or neighboring components)
rather than inventing new ones — a beautiful component that ignores its
surroundings makes the page worse.

Decide once, then never deviate:

| Token | Modern default (adjust to interview/audit) |
|---|---|
| Type scale | Tailwind's default (12/14/16/18/20/24/30/36/48px); body 16px, dense apps 14px |
| Font | System stack or one variable font (e.g. Inter, Geist); max 2 families |
| Neutrals | One gray family (slate = cool, zinc = neutral, stone = warm) doing ~90% of the UI |
| Accent | One brand/action color; semantic green/amber/red used only for meaning |
| Spacing | 4px grid; consistent component padding (pick p-4 *or* p-6, not both) |
| Radius | One scale: e.g. `rounded-md` controls, `rounded-lg` cards, `rounded-xl` overlays |
| Shadows | `shadow-sm` for raised, larger soft shadows only for floating (menus, dialogs) |

Express tokens as semantic names (`background`, `surface`, `border`, `muted`,
`accent`) — in Tailwind v4 via `@theme` CSS variables, in v3 via
`theme.extend`, in plain CSS via custom properties. Semantic names are what
make dark mode a token swap instead of a rewrite.

Deep guidance with concrete values: `references/foundations.md` — read it
when making token decisions or when output needs polish it isn't getting.

## Core craft rules

The condensed version of what separates designed from generated. Full
treatment in `references/foundations.md`.

- **Hierarchy through contrast of weight and color, not just size.** A page
  should scan in three passes: what is this / what can I do / details. Muted
  text color (`text-muted-foreground`-style) for secondary info beats
  shrinking it.
- **Whitespace is the cheapest quality signal.** Group related items tightly,
  separate unrelated ones generously (proximity principle). When a layout
  feels off, the fix is more often spacing than decoration.
- **Borders OR background shift OR shadow to separate surfaces — not all
  three.** Modern depth is quiet: 1px low-contrast borders, subtle shadows.
- **Every interactive element gets hover, focus-visible, active, and disabled
  states.** Missing states are the fastest tell of unfinished work. Never
  remove focus outlines without an equal-or-better replacement
  (`focus-visible:ring-2 ring-offset-2`).
- **Real content over lorem.** Design with realistic names, numbers, and edge
  cases (long names, zero states, 4-digit quantities). Fake-perfect content
  hides layout bugs.
- **Restraint over novelty.** One accent color. One or two font weights per
  hierarchy level. Icons from one set at one size (16 or 20px inline). If an
  element could be removed without losing meaning, remove it.

### Anti-patterns — the "generated" look

Actively avoid these; they are the strongest signals of template/AI output:

- Indigo/purple-gradient-on-everything with `shadow-lg rounded-2xl` on every
  card (the default-Tailwind-demo look)
- Emoji as icons or in headings; 🚀-style marketing copy
- Three-equal-feature-cards-with-gradient-icons hero sections
- Centered text everywhere, especially long centered paragraphs
- Cards nested in cards nested in cards; borders around everything
- Pure `#000` on pure `#fff`; tiny gray text for whole paragraphs
- Every section identical in width, rhythm, and alignment (no visual pacing)
- Dated tells: heavy drop shadows, bevels, glossy buttons, stock gradients

## Responsive — design mobile-first, verify at three widths

Base styles are the mobile layout; add complexity at `sm:`/`md:`/`lg:`.
Breakpoints should change *behavior*, not just squeeze:

- Navigation morphs (sidebar → drawer or bottom bar; top-nav links → menu)
- Grids collapse (3–4 columns → 1–2); tables become cards or scroll
  horizontally with a pinned first column
- Padding steps down (`p-4` mobile → `p-6`/`p-8` desktop); page-level type
  steps down slightly
- Touch targets ≥ 44px on mobile; no critical action hidden behind hover
  (touch has no hover)

Verify at **375px, 768px, and 1280px** minimum. Patterns and layout recipes:
`references/layout-responsive.md`.

## Components — don't rebuild solved problems

Behavior-heavy components (dialogs, dropdowns, comboboxes, tooltips, tabs)
have years of accessibility work baked into headless libraries. Rebuilding
them by hand produces keyboard traps and screen-reader bugs.

- **React projects**: Base UI (headless primitives you style with Tailwind) or
  shadcn/ui (copy-in styled components on the same idea). If the project
  already uses one, extend it in its own idiom.
- **Non-React / plain HTML**: reach for native elements first — `<dialog>`,
  `<details>`, `<select>`, `popover` attribute — then progressive enhancement.

Per-component patterns (buttons, forms, tables, navigation, empty/loading
states) and library specifics: `references/components.md`.

## Accessibility is part of "looks professional"

Non-negotiable baseline, because inaccessible UI is broken UI for a real
fraction of users — and because contrast, focus states, and semantic
structure visibly improve the design for everyone:

- Semantic HTML (`<button>` not clickable `<div>`; landmarks; one `<h1>`,
  ordered heading levels)
- WCAG AA contrast: 4.5:1 body text, 3:1 large text and UI components
- Labels on every input (visible label — placeholder is not a label)
- Full keyboard path: everything reachable, visible focus, Escape closes
  overlays, focus trapped in modals and returned on close
- Color never the only carrier of meaning (pair with icon/text)

## Verification — render it, look at it, then judge it

Design work is not done when the code compiles; it's done when it looks right.

1. **Render the result.** Run the dev server or open the HTML file. If a
   project run-skill or browser tooling is available, take screenshots at
   375px, 768px, and 1280px. Look at the screenshots.
2. **Check states**: tab through interactive elements (focus rings visible?),
   check hover, disabled, and the empty/zero-data case.
3. **Squint test**: does hierarchy survive blurring — do the important things
   still stand out?
4. **Consistency sweep**: grep your own output for off-scale values — a stray
   `p-5` among `p-4`s, a fourth gray, a second radius. Fix them.

If rendering is impossible in the environment, re-read the final code against
the checklist above and say plainly that visual verification wasn't possible.

## Reference files

| File | Read when |
|---|---|
| `references/foundations.md` | Making token/typography/color/spacing/motion decisions; output looks "off" and you need the deeper rules |
| `references/components.md` | Building or restyling specific components; choosing/using Base UI or shadcn/ui |
| `references/layout-responsive.md` | Page-level layout, app shells, breakpoint strategy, touch/mobile patterns |
| `references/redesign-audit.md` | Any redesign — full audit process and findings → fix mapping |
