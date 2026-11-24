---
name: shifty-tween-engine
description: Shifty — a small, fast TypeScript tweening engine that interpolates plain objects and numbers embedded in strings, with per-property easing and a promise-based API. Use when you need a minimal, renderer-agnostic tween core inside a custom render loop (canvas, WebGL, headless frame rendering) without a full animation framework.
---

# Shifty

Authored by Van from [jeremyckahn/shifty](https://github.com/jeremyckahn/shifty) (1.6K stars, MIT) — a compact tweening engine (v3) that animates plain JavaScript objects. It knows nothing about the DOM: you feed it `from`/`to` states and it calls your render callback with interpolated values, which makes it a clean fit for custom canvas/WebGL loops and headless per-frame rendering.

## Core API

```ts
import { tween, Tweenable } from 'shifty';

const { tweenable } = await tween({
  from: { x: 0, opacity: 0 },
  to:   { x: 320, opacity: 1 },
  duration: 1000,
  easing: 'easeInOutQuad',       // or per-property: { x: 'easeOutExpo', opacity: 'linear' }
  render: ({ x, opacity }) => { /* draw one frame */ }
});

// Reusable instance with chainable control:
const t = new Tweenable();
await t.tween({ from: { x: 0 }, to: { x: 10 } });
t.pause(); t.resume(); t.stop();
```

- **String interpolation**: values may be strings containing numbers (e.g. `"translateX(0px)"` → `"translateX(320px)"`, RGB hex colors) — Shifty tweens every numeric token inside the string. This is its signature trick.
- **Easing**: `Tweenable.easing` holds the formula set (renamed from `Tweenable.formulas` in v3); custom cubic-bezier curves supported.
- **Promises**: `tween()` resolves when the animation completes, so sequences compose with `await` instead of callback pyramids.
- **Environment-agnostic**: runs in browsers, Node, and workers — anywhere you can supply a frame loop.

## When to reach for it (vs. the rest of our stack)

- **Inside a custom render loop** (our Remotion frame rendering, canvas scenes, WebGL) where Anime/Motion would fight the loop instead of feeding it.
- **Tiny footprint matters** — embedding one tween core into a self-contained HTML file (e.g. a make.fun game entry that must be fully offline with vendored libs only).
- Prefer **Anime.js** for DOM choreography/timelines, **Ola** for values chasing a moving target with velocity continuity, **Motion** for React component animation.

## Guidance

- Keep the render callback pure: compute state outside, draw inside.
- Per-property easing is the polish lever — position on an expo curve while opacity stays linear reads far more "designed" than one easing for everything.
