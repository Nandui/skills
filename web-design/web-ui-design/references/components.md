# Component Patterns

Per-component guidance plus how to use headless/component libraries well.
Examples use Tailwind utilities; translate to plain CSS when the project
doesn't use Tailwind.

## Contents

1. [Choosing a component strategy](#choosing-a-component-strategy)
2. [Base UI](#base-ui)
3. [shadcn/ui](#shadcnui)
4. [Buttons](#buttons)
5. [Forms](#forms)
6. [Tables & data display](#tables--data-display)
7. [Navigation](#navigation)
8. [Overlays: dialogs, dropdowns, tooltips](#overlays-dialogs-dropdowns-tooltips)
9. [Cards](#cards)
10. [Feedback: empty, loading, error](#feedback-empty-loading-error)

## Choosing a component strategy

Decision order:

1. **Project already has a component system** (shadcn/ui folder, MUI, existing
   design system) → extend it in its own idiom. A foreign-styled component is
   worse than a slightly dated consistent one.
2. **React, no system yet** → shadcn/ui when the project wants ready styled
   components it owns and can edit; Base UI when the team wants full styling
   control over headless primitives. Radix UI and Headless UI occupy the same
   headless niche — same principles apply.
3. **Not React / plain HTML** → native elements first (`<dialog>`,
   `<details>`, `<select>`, the `popover` attribute, `<input type=date>`),
   styled with CSS. They're keyboard- and screen-reader-correct for free.

The rule behind the rule: **behavior-heavy components (focus trapping,
typeahead, dismissal, positioning) are solved problems** — hand-rolled
versions reliably ship keyboard traps and ARIA bugs. Spend your effort on
styling, not re-deriving behavior.

## Base UI

Headless React primitives (`@base-ui-components/react`) from the Radix/MUI
lineage. Unstyled; you bring Tailwind classes.

```jsx
import { Dialog } from '@base-ui-components/react/dialog';

<Dialog.Root>
  <Dialog.Trigger className="…button classes…">Open</Dialog.Trigger>
  <Dialog.Portal>
    <Dialog.Backdrop className="fixed inset-0 bg-black/40 backdrop-blur-sm" />
    <Dialog.Popup className="fixed top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2
                             w-full max-w-md rounded-xl bg-white p-6 shadow-xl">
      <Dialog.Title className="text-lg font-semibold">Title</Dialog.Title>
      <Dialog.Description className="mt-1 text-sm text-zinc-600">…</Dialog.Description>
      …
    </Dialog.Popup>
  </Dialog.Portal>
</Dialog.Root>
```

Working notes:
- Components are namespaced parts (`Root/Trigger/Portal/Popup/…`); style each
  part; state comes via data attributes (`data-[open]`, `data-[disabled]`)
  which pair with Tailwind: `data-[open]:opacity-100`.
- `render` prop swaps the underlying element (`<Dialog.Trigger render={<MyButton/>}>`)
  instead of nesting buttons in buttons.
- Portal-based popups escape `overflow:hidden` ancestors — if a dropdown
  clips, check that a Portal part is present.
- Base UI ships zero styles: any visual inconsistency is your class lists,
  so keep shared class strings in one place (a `cva`/constants file).

## shadcn/ui

Not a dependency — a CLI that copies styled components (built on
Radix/Base-style primitives + Tailwind) into the project, which then owns
them. `npx shadcn@latest add button dialog table`.

Working notes:
- Components land in `components/ui/`; edit them freely — that's the point.
- Theming lives in semantic CSS variables (`--background`, `--foreground`,
  `--card`, `--muted`, `--border`, `--ring`, `--primary`…). Restyle by
  changing tokens first, component files second. Dark mode is already wired
  through these tokens.
- Respect its `cva` variant pattern when adding variants (`variant="ghost"`,
  `size="sm"`) instead of one-off className overrides at call sites.
- The default look is clean but recognizable — adjusting radius, accent, and
  font gets you a distinct feel while keeping the machinery.

## Buttons

- **One primary per view.** Primary = filled accent. Everything else steps
  down: secondary = `border border-zinc-300 bg-white hover:bg-zinc-50`,
  ghost = `hover:bg-zinc-100` no border, destructive = filled/outline red
  reserved for irreversible actions.
- **Consistent heights**: `h-9` (36px) default, `h-8` compact, `h-10`
  marketing. Padding `px-4` default; text `text-sm font-medium`. Same height
  for adjacent buttons and inputs — misaligned control heights look broken.
- Icon + label: 16px icon, `gap-2`. Icon-only buttons need `aria-label` and
  a square hit area (`size-9`).
- Label = verb ("Save changes", "Invite member"), not "OK"/"Submit"/"Yes".
- Destructive confirmations: the confirm button repeats the verb ("Delete
  project"), never "Are you sure? [Yes] [No]".

## Forms

The highest-stakes component class — bad forms cost users real work.

- **Label above input** (not placeholder-as-label: it vanishes on type and
  fails screen readers). `text-sm font-medium` labels, `gap-1.5` to the input.
- Inputs: `h-9`/`h-10`, `rounded-md border-zinc-300`, focus ring in accent,
  16px font on mobile (below 16px iOS zooms the viewport on focus).
- **Single column.** Multi-column forms cause skipped fields; pair only
  tightly-coupled short fields (city/zip, first/last name).
- Help text below the input in muted `text-sm`; error text replaces or joins
  it in red with an icon, plus red border on the field. Set `aria-invalid`
  and link the message with `aria-describedby`.
- Mark **optional** fields ("Optional" in muted text next to the label) —
  marking required with asterisks makes everything shout.
- Group with `<fieldset>`/section headings every 4–6 fields; long forms
  become steps.
- Buttons: primary action right-aligned at the bottom (or full-width on
  mobile), cancel as ghost to its left. Disable only during submission —
  disabled-until-valid hides *why* the form won't submit.
- Correct `type=`/`inputmode`/`autocomplete` attributes — free UX (email
  keyboards, password managers, autofill).

## Tables & data display

- Alignment: text left, numbers right with `tabular-nums`, dates consistent.
- Header: `text-xs font-medium uppercase tracking-wide text-zinc-500`,
  `border-b`; sticky (`sticky top-0 bg-…`) past ~10 rows.
- Rows: `divide-y divide-zinc-200`, generous cell padding (`px-4 py-3`),
  `hover:bg-zinc-50` when rows are interactive. Zebra stripes only for very
  wide rows where the eye needs a guide.
- Numbers-heavy tables: `text-sm`; primary cell (name) can be `font-medium`
  while metadata cells stay muted.
- **Mobile**: tables don't shrink — transform to stacked cards (each row a
  card with label:value pairs) or horizontal scroll with pinned first column
  (`overflow-x-auto` + sticky column). Choose per table, don't let it squish.
- Row actions: trailing icon-button menu (⋯) rather than a button per action
  per row.
- Stat blocks: label in muted small text *above*, value large
  (`text-3xl font-semibold tabular-nums`), delta with color + arrow icon
  (color never alone).

## Navigation

- **App with ≥5 sections → left sidebar** (`w-60`–`w-64`): nav items
  `h-9 rounded-md px-3 text-sm font-medium gap-2` with 16–20px icons; active
  = accent-tinted bg + accent text (`bg-…-50 text-…-700`), inactive muted
  with `hover:bg-zinc-100`. Group with muted `text-xs uppercase` section
  labels.
- **Marketing / few sections → top nav**, `h-14`–`h-16`, logo left, links
  center/left, actions right, `border-b` or subtle shadow on scroll.
- **Mobile**: sidebar becomes a drawer behind a hamburger (top-left) or — for
  ≤5 top-level destinations in app-like products — a bottom tab bar
  (`h-16`, icon + tiny label, safe-area padding).
- Always show current location: active nav state + page title; breadcrumbs
  only for hierarchies ≥3 deep.

## Overlays: dialogs, dropdowns, tooltips

- Use a library or `<dialog>`; the hard parts (focus trap, escape/outside
  dismissal, positioning, scroll lock) must come from somewhere proven.
- **Dialogs**: max-w-md/lg, `rounded-xl p-6 shadow-xl`, backdrop
  `bg-black/40` (optionally `backdrop-blur-sm`); title `text-lg font-semibold`,
  actions bottom-right. Content longer than ~60vh → make it a page, not a
  modal. Confirmation dialogs name the action and object ("Delete 'Q3
  report'?").
- **Dropdown menus**: `min-w-40 rounded-lg border bg-white p-1 shadow-lg`;
  items `rounded-md px-2 py-1.5 text-sm` with hover bg; destructive item in
  red, separated by a divider.
- **Tooltips**: labels/shortcuts only (things that fit in one line); never
  put content or actions in tooltips — touch devices can't hover. Delay
  ~300–500ms.
- Overlay animation: fade + slight scale (0.98→1) or 4–8px translate,
  150–200ms out / 100–150ms in.

## Cards

- Card = one coherent unit of content, not a universal wrapper. If everything
  is a card, nothing is — flat sections with headings often beat card grids.
- Structure: optional media (`aspect-video object-cover`), body `p-4`/`p-6`
  with title (`font-semibold`) + muted description, optional footer separated
  by `border-t` for meta/actions.
- Separation: on gray page bg, white bg alone; on white, `border` *or*
  `shadow-sm` (see foundations). Whole-card clickable → wrap in `<a>`,
  `hover:border-zinc-300` or slight shadow lift, and don't nest other links
  inside.
- **No cards inside cards.** Inner grouping uses spacing and dividers.

## Feedback: empty, loading, error

The states that separate finished products from demos — design all three for
every data surface:

- **Empty (first use)**: centered, max-w-sm: muted icon or small
  illustration, one-line headline ("No projects yet"), one sentence of
  guidance, primary action ("Create project"). An empty table header row with
  nothing under it is not an empty state.
- **Empty (filtered)**: different message — "No results for 'x'" + clear-filters
  action. Don't show the first-use state when filters are the cause.
- **Loading**: skeletons that mirror the real layout (`animate-pulse` blocks
  matching card/row shapes) for content; spinners only for button-level
  actions. Keep layout dimensions stable so nothing jumps when data lands.
  Loads under ~300ms need no indicator — a flash of skeleton is worse than
  nothing.
- **Error**: say what failed and what to do ("Couldn't load members — Retry")
  inline where the content would be; toasts for transient action failures.
  Never a dead-end red box without a next step.
