---
name: ola-smooth-values
description: Smooth value interpolation for animation — make numbers, positions, camera moves, and HUD values glide toward their targets with velocity continuity instead of jumping or easing robotically. Based on the Ola library (franciscop/ola, MIT). Use when animating anything that chases a changing target in canvas, DOM, or Remotion-adjacent web scenes — pointers, cameras, counters, chart values, character movement.
---

# Ola — Smooth Values

Authored by Van from the MIT-licensed library [franciscop/ola](https://github.com/franciscop/ola) (1.8K stars, single dependency-free file, ~3 KB). This skill captures its technique and API so the pattern can be applied even without vendoring the file.

## The core idea

Naive smoothing (`current += (target - current) * 0.1`) and standard easings both break when the target changes mid-flight: the value's velocity jumps, which the eye reads as a twitch. Ola instead interpolates along a **cubic Hermite curve** built from the value's *current position and current velocity*, so retargeting mid-animation is seamless — the value keeps its momentum and curves into the new target.

Concretely, for a segment from `x0` with velocity `v0` over duration `t1`, position at time `t` is `a·t³ + b·t² + c·t + d` with `a = (v0·t1 + 2·x0)/t1³`, `b = −(2·v0·t1 + 3·x0)/t1²`, `c = v0`, `d = x0` (where `x0` is the remaining distance). On `set()`, the current position and velocity are sampled and become the start of the next segment.

## API (vendored library)

```js
import Ola from "./ola.js"; // vendor the single file; MIT license, keep attribution

const pos = Ola({ x: 0, y: 0 }, 300); // values + default transition time in ms

pos.x = 100;        // set a new target — reading pos.x now glides toward it
pos.set({ x: 50, y: 20 }, 500); // retarget several values with a custom time
pos.$x;             // the target value (100)
pos._x.speed;       // current velocity of x (also .from, .to, .current)
```

Reading the property always returns the value interpolated at *now*, so a render loop just reads `pos.x` each frame — no manual tick/update call.

## When to use it in our work

- **Camera and parallax moves** in canvas/HTML scenes that must react to changing targets without jerking.
- **HUD numbers and chart values** that count toward a changing figure (balances, prices, scores) — the glide reads as premium, the jump reads as broken.
- **Pointer/character following** where the target moves continuously.
- Anywhere a value chases a target that can change before arrival — that mid-flight retarget is exactly the case lerp and CSS transitions handle worst.

## Guidance

- Default transition ~300 ms feels responsive; longer (600–1000 ms) for cameras and large layout moves.
- Pair with real motion, not stills: this technique exists to make movement feel alive and physical — velocity continuity is the difference between "animated" and "a slideshow with zoom".
- The library is ESM, framework-free, and works in browsers and Node. For React there is no special binding — read values inside your render/rAF loop.
