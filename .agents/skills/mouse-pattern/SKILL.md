---
name: mouse-pattern
description: Use whenever you add, change or debug the mouse-reactive dot background on the WarpDancer site (the full-screen canvas with id bg). Contains the full working code and the values that tune it.
---

# Mouse-reactive background pattern

Do not invent your own version. Copy the snippet below and adjust only the CONFIG constants.

## HTML

Put this as the first element inside `<body>`:

```html
<canvas id="bg"></canvas>
```

## CSS

```css
html, body { background: var(--bg); margin: 0; }
#bg { position: fixed; inset: 0; width: 100%; height: 100%; z-index: 0; pointer-events: none; }
main, header, footer, section { position: relative; z-index: 1; }
```

The canvas sits behind the content. All content wrappers need `position: relative; z-index: 1`.

## CONFIG (top of the script, tunable by the owner)

```js
const DOT_SPACING = 28;      // pixels between dots
const DOT_SIZE = 1.6;        // base radius in pixels
const MOUSE_RADIUS = 140;    // how far the cursor reaches
const MOUSE_STRENGTH = 2.2;  // how hard dots are pushed away
const DRIFT_SPEED = 0.0004;  // speed of the automatic motion when no mouse
```

## JS (copy as is)

```js
const canvas = document.getElementById('bg');
const ctx = canvas.getContext('2d');
const accent = getComputedStyle(document.documentElement).getPropertyValue('--accent').trim() || '#00FF88';
const reduceMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches;

let w = 0, h = 0, dots = [];
const mouse = { x: -9999, y: -9999, active: false };

function build() {
  const dpr = Math.min(window.devicePixelRatio || 1, 2);
  w = window.innerWidth;
  h = window.innerHeight;
  canvas.width = w * dpr;
  canvas.height = h * dpr;
  ctx.setTransform(dpr, 0, 0, dpr, 0, 0);

  let spacing = DOT_SPACING;
  while ((w / spacing) * (h / spacing) > 3000) spacing += 2; // cap the dot count

  dots = [];
  for (let y = spacing / 2; y < h; y += spacing) {
    for (let x = spacing / 2; x < w; x += spacing) {
      dots.push({ hx: x, hy: y, x: x, y: y, vx: 0, vy: 0 });
    }
  }
}

function frame(time) {
  // No real mouse: a virtual cursor drifts across the screen.
  if (!mouse.active) {
    mouse.x = w / 2 + Math.cos(time * DRIFT_SPEED * 3) * w * 0.35;
    mouse.y = h / 2 + Math.sin(time * DRIFT_SPEED * 5) * h * 0.30;
  }

  ctx.clearRect(0, 0, w, h);
  ctx.fillStyle = accent;

  for (const d of dots) {
    const dx = d.x - mouse.x;
    const dy = d.y - mouse.y;
    const dist = Math.sqrt(dx * dx + dy * dy) || 1;
    let near = 0;

    if (dist < MOUSE_RADIUS) {
      near = 1 - dist / MOUSE_RADIUS;
      d.vx += (dx / dist) * near * MOUSE_STRENGTH;
      d.vy += (dy / dist) * near * MOUSE_STRENGTH;
    }

    // spring back to the home position, with damping
    d.vx += (d.hx - d.x) * 0.06;
    d.vy += (d.hy - d.y) * 0.06;
    d.vx *= 0.82;
    d.vy *= 0.82;
    d.x += d.vx;
    d.y += d.vy;

    ctx.globalAlpha = 0.18 + 0.82 * near;
    ctx.beginPath();
    ctx.arc(d.x, d.y, DOT_SIZE * (1 + near * 1.5), 0, Math.PI * 2);
    ctx.fill();
  }

  ctx.globalAlpha = 1;
  requestAnimationFrame(frame);
}

function drawStatic() {
  ctx.clearRect(0, 0, w, h);
  ctx.fillStyle = accent;
  ctx.globalAlpha = 0.18;
  for (const d of dots) {
    ctx.beginPath();
    ctx.arc(d.hx, d.hy, DOT_SIZE, 0, Math.PI * 2);
    ctx.fill();
  }
  ctx.globalAlpha = 1;
}

window.addEventListener('pointermove', (e) => {
  mouse.x = e.clientX;
  mouse.y = e.clientY;
  mouse.active = true;
});
document.addEventListener('mouseleave', () => { mouse.active = false; });
window.addEventListener('pointerup', (e) => { if (e.pointerType === 'touch') mouse.active = false; });
window.addEventListener('resize', () => { build(); if (reduceMotion) drawStatic(); });

build();
if (reduceMotion) drawStatic();
else requestAnimationFrame(frame);
```

## Rules

- Keep the dot cap at about 3000. Do not remove the `while` loop.
- The pattern uses the accent color only. No second color.
- Do not add libraries.
- To change the look, edit CONFIG values. Examples: larger `DOT_SPACING` for a calmer look, larger `MOUSE_RADIUS` for a wider reaction.

## Adding it to the page

The full script is long. Add it in small edits, not one big edit: first the HTML tag and the CSS, then the setup code (down to `build()`), then `frame()` and `drawStatic()`, then the event listeners and the start lines. Copy the code exactly.

## Common changes (use small edits, never rewrite the script)

- Calmer pattern: raise `DOT_SPACING` (for example 36) or lower `DOT_SIZE`.
- Denser pattern: lower `DOT_SPACING` (the script already caps the dot count at about 3000).
- Wider reaction: raise `MOUSE_RADIUS`. Stronger push: raise `MOUSE_STRENGTH`.
- Faster idle movement: raise `DRIFT_SPEED`.
- Pull the dots toward the cursor instead of pushing them away: in `frame()`, change the two lines `d.vx += (dx / dist) * near * MOUSE_STRENGTH;` and `d.vy += (dy / dist) * near * MOUSE_STRENGTH;` from `+=` to `-=`.
- Short lines instead of dots: in `frame()`, replace the `ctx.arc(...)` drawing with `ctx.moveTo(d.x, d.y)` and `ctx.lineTo(d.x + 6, d.y)`, then `ctx.stroke()` instead of `ctx.fill()`, and set `ctx.strokeStyle = accent`.
- The pattern must keep using only the accent color, and the content must stay above the canvas.

## Check

- Move the mouse: dots near it move away and grow brighter, then settle back.
- Leave the window: the pattern keeps moving slowly by itself.
- Resize the window: the grid fills the new size.
- Text stays readable over the pattern (dots are at most 18 percent opaque away from the cursor).
