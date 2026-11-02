# AGENTS.md: WarpDancer website

Read this file fully before every task. Follow it exactly. If a rule here conflicts with your habits, follow the rule.

## Project

WarpDancer sells ready-made local AI machines to businesses. The customer buys the hardware once, with an open model and an agent harness already installed, and then pays only for electricity. No subscription for the intelligence. Data stays in the building. The machine can be tuned to the company's own workflows.

Tagline: "Stop renting your intelligence. Own it."

Audience: small businesses curious about AI, and larger companies that cannot send sensitive data to outside APIs.

Your job: build a one-page marketing website as ONE file, `index.html`.

## Output rules

- One self-contained file: `index.html`. HTML, CSS and JS inline.
- No frameworks, no build step, no CDN, no external fonts, no external images, no network requests.
- Vanilla JS only. Background pattern uses a `<canvas>`.
- System font stack only.
- Work in small steps (see Workflow). Do not rewrite the whole file when only one part needs to change. Use targeted edits.

## Workflow

Do the steps in order. Finish and check one step before starting the next. If a `skills/` folder exists, read the matching skill file before the step.

1. Skeleton: HTML structure with all sections and placeholder-free real text. No styling beyond a reset.
2. Styling: colors, typography, layout. Use the CONFIG variables only.
3. Background pattern: canvas with mouse reaction (`skills/mouse-pattern/SKILL.md`).
4. Copy pass: check all text against the writing rules (`skills/copy-rules/SKILL.md`).
5. Final check: run the checklist at the bottom. Fix every failure.

After each step, say in one or two lines what you changed. Do not explain the code at length.

## Sections (in this order)

1. Hero: tagline, one sentence of explanation, one button "Book a call".
2. Why own it: three short points as plain prose, not cards with icons.
   - Cloud AI subscriptions are priced below what they cost to provide, and prices tend to rise over time, as they did with Uber and Netflix.
   - Many companies cannot send sensitive data to an outside API. Local AI keeps it in the building.
   - Open models you can run yourself are now close to the frontier, and a better model can be installed on the same hardware later.
3. The three machines (table or columns):
   - Small: about 12B model, one RTX 5090, for chat, documents and forms.
   - Medium: about 70B model, one RTX PRO 6000 Blackwell, for everyday coding and agents.
   - Large: close-to-frontier model, four to eight data center GPUs, for heavy coding on big projects.
4. Optional extras: custom fine-tuning and engineer time, billed monthly.
5. Contact: plain form with name, email, message. It does not need to send anything.
6. Footer: "WarpDancer" and the year.

Do not invent prices, speeds, benchmarks, customers, quotes or awards. If a fact is not in this file, leave it out.

## Design rules

- Background `#000000`. Main text `#FFFFFF`. Secondary text and borders: white at lower opacity only.
- Exactly ONE accent color, set once as `--accent` (default `#00FF88`). Use it for: the button, links, one highlight word in the hero, and the background pattern. Nowhere else.
- Left-aligned text. Line width at most about 65 characters. Large headings, generous whitespace.
- Sections must not all look the same. Vary the layout between sections.
- Must work on a phone screen and a wide desktop screen.

### Background pattern

- Fixed full-screen canvas behind all content, `pointer-events: none`.
- Grid of small dots or short lines. Elements near the cursor are pushed away, rotated or brightened, with smooth easing, and relax back when the cursor leaves.
- Use `requestAnimationFrame`. Handle `resize` and `devicePixelRatio`. At most about 3000 elements.
- No mouse (touch devices): the pattern drifts slowly by itself.
- If `prefers-reduced-motion` is set, draw the pattern static.

## CONFIG block

Put this at the very top of the file, clearly commented, so the owner can tune the look by editing numbers.

CSS variables: `--accent`, `--bg`, `--text`, `--muted`.

JS constants: `DOT_SPACING`, `DOT_SIZE`, `MOUSE_RADIUS`, `MOUSE_STRENGTH`, `DRIFT_SPEED`.

Never hard-code these values anywhere else in the file.

## Banned

These make a site look AI-generated. Never use them.

- Em dashes and en dashes, in text and in code comments. Use commas, periods or colons.
- Pills or badges saying "LIVE", "NEW", "BETA" or similar.
- Emojis, gradients, glassmorphism, glow shadows, purple.
- A row of three identical cards with icons.
- Fake testimonials, fake logos, fake statistics, fake awards.
- Words: revolutionize, unleash, seamless, cutting-edge, next-generation, empower, elevate, supercharge, game-changing, leverage, robust.
- Sentences starting with "Imagine". Sections ending with a rhetorical question.

## Writing style

Short, plain, direct sentences. Say what the product is and does. Concrete nouns, no hype. Write like a person explaining it to a colleague.

## Final checklist

Before saying you are done, verify each item by reading the file:

- [ ] Single file `index.html`, no external requests
- [ ] Background is exactly `#000000`, main text exactly `#FFFFFF`
- [ ] Only one accent color is used
- [ ] No em dash or en dash anywhere (search the file for the characters)
- [ ] No "LIVE" badge or similar
- [ ] No banned words
- [ ] Pattern reacts to the mouse and works after a window resize
- [ ] All six sections are present, in order
- [ ] CONFIG block is at the top and is the only place values are defined
- [ ] No invented numbers or claims

If any item fails, fix it and check again.