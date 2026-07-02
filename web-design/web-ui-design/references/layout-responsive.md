# Layout & Responsive Design

Page-level structure and how it adapts across desktop, tablet, and mobile.

## Contents

1. [Page anatomy](#page-anatomy)
2. [App shells](#app-shells)
3. [Content layouts](#content-layouts)
4. [Breakpoint strategy](#breakpoint-strategy)
5. [What actually changes per breakpoint](#what-actually-changes-per-breakpoint)
6. [Touch & mobile ergonomics](#touch--mobile-ergonomics)
7. [Marketing pages](#marketing-pages)

## Page anatomy

Every screen answers three questions in order: **where am I** (nav state,
page title), **what can I do** (primary action), **what's here** (content).

- Page header: title (`text-2xl font-semibold` apps / `text-3xl`+ content
  sites), optional one-line muted description, primary action button
  top-right. One `<h1>` per page.
- Constrain content width: `max-w-7xl mx-auto` for full app pages,
  `max-w-4xl` for settings/detail pages, `max-w-2xl` for forms and prose.
  Edge-to-edge content on wide monitors is a top-3 "unfinished" tell.
- Consistent page gutter: `px-4 sm:px-6 lg:px-8` is the standard rhythm.

## App shells

**Sidebar shell** (default for multi-section apps):

```html
<div class="flex min-h-screen bg-zinc-50">
  <aside class="hidden md:flex w-64 flex-col border-r border-zinc-200 bg-white">
    <!-- logo, nav, user menu -->
  </aside>
  <div class="flex flex-1 flex-col">
    <header class="flex h-14 items-center gap-4 border-b border-zinc-200 bg-white px-4 md:px-6">
      <button class="md:hidden" aria-label="Open menu"><!-- hamburger --></button>
      <!-- breadcrumb / search / user -->
    </header>
    <main class="flex-1 p-4 md:p-6 lg:p-8">
      <div class="mx-auto max-w-7xl"><!-- page --></div>
    </main>
  </div>
</div>
```

On mobile the sidebar becomes an overlay drawer (same nav markup in a
`<dialog>`/drawer component) triggered by the hamburger — never squeeze a
sidebar to icons-only as the *only* mobile strategy.

**Top-nav shell** (few sections, content-focused): `h-14`–`h-16` header with
`border-b`, content below in a `max-w-*` container. Mobile: links collapse
into a menu; keep the primary CTA visible outside the menu when possible.

**Bottom tab bar** (mobile app-like, ≤5 destinations): `fixed bottom-0 h-16`,
icon + 10–11px label per tab, active in accent, `pb-[env(safe-area-inset-bottom)]`
for iOS. Pairs with a top bar for page title/actions.

## Content layouts

- **Card/stat grids**: `grid gap-4 sm:grid-cols-2 lg:grid-cols-4` (stats),
  `md:grid-cols-2 xl:grid-cols-3` (content cards). Equal-height cards via
  grid, not manual heights.
- **List + detail**: desktop `grid-cols-[320px_1fr]` (list left, detail
  right); mobile becomes two screens — list, then detail with a back button.
  Don't render both panes squeezed side-by-side on a phone.
- **Settings**: single `max-w-2xl`–`3xl` column of sections; each section =
  heading + description left of (lg) or above (mobile) its fields
  (`lg:grid-cols-[240px_1fr]`), `divide-y` between sections.
- **Two-thirds + sidebar** (`lg:grid-cols-3`, main `col-span-2`): dashboards
  and detail pages with secondary info; sidebar stacks below main on mobile
  (source order: main first).
- **Full-bleed tables**: table area can exceed the prose max-width; let data
  breathe while keeping the page header aligned to the standard container.

## Breakpoint strategy

**Mobile-first**: unprefixed utilities are the mobile design; `sm:`(640)
`md:`(768) `lg:`(1024) `xl:`(1280) add from there. Desktop-first thinking
("shrink until it fits") produces cramped phones; mobile-first produces
progressive enhancement.

In practice most screens need exactly **two or three moments**:
- ~`md:` — drawer nav → persistent sidebar; single column → 2 columns
- ~`lg:`/`xl:` — full multi-column density; wider gutters

Don't scatter all five prefixes across every element. If a component needs
four breakpoint variants, simplify the component.

**Test at 375px, 768px, 1280px** (add 1536px+ if users have big monitors —
check nothing stretches absurdly). Resize through the *in-between* widths
too; layouts break between the named stops, e.g. 700px or 1100px.

## What actually changes per breakpoint

Real responsive design changes behavior, not just column counts:

| Element | Mobile (base) | Desktop (md/lg+) |
|---|---|---|
| Navigation | Drawer or bottom tabs | Persistent sidebar / top links |
| Grids | 1 column (2 for small cards) | 2–4 columns |
| Tables | Stacked cards, or h-scroll with pinned first column | Full table |
| Page padding | `p-4` | `p-6`/`p-8` |
| Page title | `text-xl`/`2xl` | `text-2xl`/`3xl` |
| Dialogs | Full-screen or bottom sheet | Centered `max-w-md/lg` |
| Primary action | Full-width button, or fixed bottom bar | Inline top-right |
| Form buttons | Full-width stacked | Inline right-aligned |
| Multi-step / list-detail | Sequential screens | Side-by-side panes |
| Hover-revealed actions | Always visible (⋯ menu) | May reveal on hover |

Type scale steps down ~1 notch on mobile for display sizes (`text-4xl
md:text-5xl` heroes); body text stays 14–16px everywhere.

## Touch & mobile ergonomics

- **Touch targets ≥44×44px**: `h-11` buttons or smaller visuals with extended
  hit area (padding / pseudo-element). Adjacent targets get ≥8px gaps —
  fat-finger misfires on cramped icon rows are a top mobile complaint.
- **No hover on touch.** Anything revealed on hover (row actions, tooltips,
  submenus) needs a tap path: visible ⋯ menu, long-press alternative, or
  always-visible controls. `@media (hover: hover)` gates hover-only styling.
- **Thumb zone**: primary actions live in the bottom half of the screen when
  possible (bottom bars, FAB-style primary buttons); top corners are the
  hardest reach on large phones.
- Inputs: font-size ≥16px (iOS zooms otherwise), correct `inputmode`, labels
  that don't rely on side-by-side layout.
- Momentum scrolling containers: `overflow-x-auto` + `-webkit-overflow-scrolling`
  handled by modern browsers, but add `snap-x snap-mandatory` for carousels.
- Safe areas: `pb-[env(safe-area-inset-bottom)]` on fixed bottom elements,
  `pt-[env(safe-area-inset-top)]` on fixed headers in PWA/standalone.
- Viewport units: prefer `min-h-dvh` over `min-h-screen`/`100vh` — `100vh`
  overflows behind mobile browser chrome.

## Marketing pages

Structure that converts and doesn't read as template:

- **Hero**: clear value headline (≤10 words, `text-4xl md:text-6xl
  font-semibold tracking-tight`), one-sentence sub in muted text, one primary
  CTA (+ optional ghost secondary), proof element (product screenshot, logos,
  metric). Left-aligned heroes with product visuals right are as effective as
  centered ones and less template-y.
- **Visual pacing**: alternate section rhythm — full-width, contained,
  two-column, quote — rather than N identical centered blocks. Vary
  backgrounds subtly (white / `zinc-50` / one dark section).
- **Feature sections**: lead with the user outcome, show the actual product
  (screenshot in a browser frame beats abstract icons). If using icon
  trios, one accent color, consistent stroke icons — no rainbow gradient
  circles.
- **Social proof**: real logos in grayscale (`opacity-60`), specific
  testimonials (name, role, photo) over star ratings.
- **Footer**: link columns by topic, legal line, that's it. Fine for it to be
  boring.
- Typography earns more here: tighter tracking on display sizes, generous
  `py-16`–`py-24` sections, `max-w-2xl` on all body copy.
