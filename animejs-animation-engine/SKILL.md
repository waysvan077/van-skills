---
name: animejs-animation-engine
description: Anime.js V4 animation engine for the web — timelines, stagger, springs, SVG and text animation for DOM/canvas scenes. Use when building web-based animated scenes, title sequences, interactive pages, or HTML layers that need choreographed, genuinely moving animation rather than CSS transitions.
---

# Anime.js V4

Authored by Van from [juliangarnier/anime](https://github.com/juliangarnier/anime) (74.9K stars, MIT) — a lightweight JavaScript animation engine. V4 is modular ESM; there is no UMD `anime()` global anymore (that was V3).

## Core API

```js
import { animate, createTimeline, stagger, createScope } from 'animejs';

animate('.square', {
  x: 320,
  rotate: { from: -180 },          // per-property keyframes/from-to supported
  duration: 1250,
  delay: stagger(65, { from: 'center' }),
  ease: 'inOutQuint',
  loop: true,
  alternate: true
});
```

- **`animate(targets, params)`** — targets are selectors, elements, or plain objects (numbers on any object can be animated, which makes it usable as a general tween engine for canvas values).
- **`createTimeline()`** — sequence animations with position parameters; the tool for choreographed scenes and title sequences where beats must land in order.
- **`stagger(value, opts)`** — distribute delays/values across targets; `from: 'center' | 'last' | index`, grid mode for tile/letter fields.
- **Eases** — named curves (`outExpo`, `inOutQuint`, …), cubic-bezier strings, and **spring** (`createSpring`) for physical, interruptible motion.
- **Modules** — `animejs/svg` (motion path, line drawing, morphing), `animejs/text` (split text for per-letter animation), `animejs/draggable`, `animejs/scope` (`createScope` binds animations to a React component lifecycle and cleans them up), `animejs/utils`.

## When to reach for it (vs. the rest of our stack)

- **DOM/HTML scenes**: hero animations, animated infographics, interactive pages — Anime is lighter and more direct than Motion for vanilla JS.
- **Text and SVG choreography**: letter-stagger title reveals and path drawing are one-liners here.
- **Inside Remotion HTML layers or standalone web scenes** rendered to video: drive elements with Anime timelines, capture per-frame or record the page.
- Prefer **Motion** for React component trees (layout animations, gestures, AnimatePresence-style exits) and **Ola** for single values chasing a moving target with velocity continuity.

## Guidance

- Choreograph with timelines + stagger, not scattered `setTimeout`s — one timeline is scrubbable, reversible, and frame-accurate.
- Animate transform/opacity properties for 60 fps; animating layout properties (width/top) is the classic jank source.
- Real motion is the point: combine translation, rotation, and scale with spring or expo easings so scenes feel physical, never a slow zoom on a still image.
