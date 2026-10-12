# Tawananyasha Nyamapfene: Portfolio (Assignment 2: Styling & Responsive Layout)

A responsive portfolio page for a Software Engineering student focused on cybersecurity.
Assignment 1's semantic HTML5 structure (`header`, `nav`, `main`, `section`, `article`,
`aside`, `footer`) is unchanged. This assignment adds CSS and Tailwind classes only.

## Files

| File         | Purpose                                                              |
|--------------|----------------------------------------------------------------------|
| `index.html` | Semantic markup + Tailwind utility classes (header/nav, contact form) |
| `style.css`  | Hand-written, mobile-first CSS for everything else                   |
| `README.md`  | This document                                                        |

## How to run

Open `index.html` in a browser. Tailwind is loaded from its Play CDN
(`cdn.tailwindcss.com`, pinned to 3.4.17), so an internet connection is needed.

## Tailwind vs custom CSS: the split and why

The split is by section, so no element is styled by both layers.

| Section                        | Styled with     | Reason                                                                 |
|--------------------------------|-----------------|------------------------------------------------------------------------|
| `<header>` and `<nav>`         | **Tailwind**    | Mostly spacing, colour and a flex row: ideal for utilities.            |
| Contact `<form>` and its fields| **Tailwind**    | Many repeated small states (focus rings, borders) are quick as utilities. |
| Page layout, About, Portfolio, Skills, Contact card, footer | **style.css** | Uses design tokens (CSS variables), grid placement and the custom media queries. |

Details that keep the two layers from clashing:

- Colours in `style.css` (`--color-*` variables) use the same hex values as the Tailwind
  `slate` and `sky` colours used in the HTML, so the page looks consistent.
- Tailwind's reset removes default margins, list bullets and heading sizes. Because of that,
  `style.css` sets those explicitly and scopes its selectors (for example `main h2`)
  so they are not overridden.
- The `<form>` element itself is only styled by Tailwind. The surrounding `#contact` card
  (padding, border, background) comes from `style.css`.

## CSS techniques used

**Box model (Part A).** `box-sizing: border-box` everywhere. Spacing comes from a token
scale (`--space-1` to `--space-5`). Cards use consistent padding, 1px borders and
rounded corners instead of default browser spacing.

**Visual hierarchy (Part A).** A single type scale (`--text-sm` to `--text-xl`, with a
larger `h2` from 768px up). Dark slate header, white cards on a light background, and
sky blue as the one accent colour for headings underlines, links and buttons.

**Flexbox (Part B).**
- Nav bar (Tailwind): `flex`, `flex-wrap`, `items-center`, `justify-center`, `gap-*`.
- Skills chips (`style.css`): `display: flex`, `flex-wrap`, `align-items`, `justify-content`, `gap`.
- Footer (`style.css`): column on phones, row with `justify-content: space-between` from 768px.

**CSS Grid (Part C).**
- Page layout: one column on mobile; at 1024px `grid-template-columns: minmax(0, 2fr) minmax(0, 1fr)`
  with Skills in the right column (sticky) and Contact spanning the full width.
- Portfolio cards: `repeat(auto-fit, minmax(min(100%, 16rem), 1fr))` with `gap`.
- Contact form (Tailwind): `grid-cols-1` on phones, `md:grid-cols-2` from 768px.

## Responsive design (Part D)

- Base styles are written for phones (~375px) first.
- Larger screens are added with `min-width` queries only:
  - `@media (min-width: 48rem)`: tablet (768px): more padding, larger headings, footer becomes a row.
  - `@media (min-width: 64rem)`: desktop (1024px): two-column page grid.
  - `@media (prefers-reduced-motion: reduce)`: turns off smooth scrolling.
- `img, svg, video { max-width: 100%; height: auto }` is set so any media added later
  scales with its container. Text wraps rather than shrinking.
- Tailwind responsive variants used: `md:px-8`, `md:py-12`, `md:text-5xl`, `md:text-xl`,
  `md:gap-4` (header/nav) and `md:grid-cols-2`, `md:col-span-2`, `md:justify-self-start`
  (contact form).

## Testing (Part F)

Tested in the browser device toolbar (DevTools responsive mode) at:

| Width   | Device type | Result                                                                 |
|---------|-------------|------------------------------------------------------------------------|
| 375px   | Mobile      | Single column; nav wraps cleanly; no horizontal scrollbar; form is one column. |
| 768px   | Tablet      | Larger headings and padding; portfolio shows two cards; form is two columns.   |
| 1280px  | Desktop     | Two-column layout with sticky Skills panel; Contact spans full width.          |

At all three widths there is no horizontal scrolling, no overlapping text and the nav links work.

Code quality: HTML and CSS were checked with the W3C validators, consistently indented,
and contain no placeholder or Lorem Ipsum text.

## Responsive Notes

**Breakpoints tested:** 375px (mobile), 768px (tablet) and 1280px (desktop), using the
browser's device toolbar. The CSS breakpoints are `min-width: 48rem` (768px) and
`min-width: 64rem` (1024px), so 1280px uses the desktop layout.

**Flexbox vs. Grid, and why:**
- **Flexbox** is used for one-dimensional rows where items wrap and share space: the nav links,
  the skills chips and the footer. Items flow in a single direction, and `flex-wrap` and `gap`
  handle narrow screens.
- **Grid** is used where rows and columns both matter: the overall page layout (two columns on
  desktop, with Skills spanning two rows), the portfolio card grid (`repeat(auto-fit, minmax())`)
  and the contact form (1 column on mobile, 2 columns from `md:`).

**Sections that use Tailwind utilities:**
- Header (title and tagline)
- Navigation bar
- Contact form (layout, fields and submit button)

Everything else (page layout, About, Portfolio, Skills, Contact card shell and footer) uses `style.css`.
