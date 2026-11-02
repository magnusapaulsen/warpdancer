---
name: frontend-design
description: Use in steps 1 and 2 of the workflow, for page structure, typography, layout and spacing. A short design guide for a black and white site with one accent color.
---

# Frontend design for WarpDancer

Goal: a site that looks designed by a person, not generated. Black, white, one accent, strong typography, lots of space.

## Direction

Direction is fixed: stark, technical, quiet. Black background, white text, one accent color. The confidence comes from type size, spacing and restraint, not from effects.

## Color

- `--bg: #000000`, `--text: #FFFFFF`, `--accent: #00FF88`.
- `--muted: rgba(255,255,255,0.6)` for secondary text. Borders: `rgba(255,255,255,0.15)`.
- The accent is used for: the button, links, one highlight word in the hero, the background pattern. Nothing else.
- No gradients, no shadows, no glow, no second accent.

## Typography

- Font stack: `system-ui, -apple-system, "Segoe UI", Roboto, sans-serif`. For small technical labels you may use `ui-monospace, "SF Mono", Menlo, Consolas, monospace`.
- Hero heading: `font-size: clamp(2.5rem, 7vw, 5.5rem)`, weight 700, line-height 1.05, letter-spacing -0.02em.
- Section headings: `clamp(1.6rem, 3.5vw, 2.6rem)`, weight 600.
- Body: 1.125rem, line-height 1.6, color `--text` or `--muted`.
- Body text max width: `65ch`.
- Left-align everything. No centered paragraphs.

## Layout

- Page container: `max-width: 1100px; margin: 0 auto; padding: 0 24px`.
- Vertical rhythm: sections separated by `clamp(80px, 12vw, 160px)`.
- Vary the layout between sections so the page does not feel repetitive:
  - Hero: large heading, one sentence, one button, lots of empty space. Full viewport height.
  - Why own it: a two-column layout on desktop. A small numbered label (01, 02, 03) on the left, prose on the right. Rows separated by thin borders.
  - Machines: a table with thin borders, one column per machine. On phones it becomes stacked blocks.
  - Extras: a short paragraph and a plain list, with a narrow text column.
  - Contact: a narrow single-column form.
  - Footer: one line, muted text.
- No row of three identical cards with icons.

## Components

- Button: solid `--accent` background, black text, `padding: 14px 28px`, `border-radius: 4px`, weight 600. Hover: slightly lower opacity. Visible focus outline.
- Links: `--accent`, underline on hover.
- Form fields: transparent background, 1px border `rgba(255,255,255,0.25)`, white text, `padding: 12px 14px`, border-radius 4px. Focus: border becomes `--accent`. Every field has a visible label above it.
- Table: no zebra stripes. Thin horizontal borders. Header row in muted uppercase small text.

## Responsive

- Use `<meta name="viewport" content="width=device-width, initial-scale=1">`.
- One breakpoint is enough: `@media (max-width: 720px)`. Columns stack, padding shrinks to 20px.
- No horizontal scrolling at 360px width.

## Accessibility

- White on black has full contrast. Keep muted text at 60 percent white or brighter.
- Use real elements: `header`, `main`, `section`, `h1`, `h2`, `table`, `form`, `label`, `button`.
- One `h1`. Headings in order.
- Respect `prefers-reduced-motion`.

## Never

- No "LIVE", "NEW" or "BETA" pills. No badges at all.
- No emojis, no stock-style icons, no glassmorphism, no purple.
- No fake logos, testimonials or statistics.
- No em dashes or en dashes in text or comments.

## Check

- Resize the window from 360px to 1600px. Nothing overlaps or overflows.
- Count the colors: black, white, greys from white, one accent. Nothing else.
- Squint test: the hero heading is clearly the most important thing on the page.
