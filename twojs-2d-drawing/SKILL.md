---
name: twojs-2d-drawing
description: Two.js — a renderer-agnostic 2D drawing and animation API for the web. One scene graph renders to SVG, Canvas 2D, or WebGL. Use when building 2D vector scenes, generative art, diagrams, or animated illustrations that must run as web content or be captured into video frames.
---

# Two.js

Authored by Van from [jonobr1/two.js](https://github.com/jonobr1/two.js) (8.7K stars, MIT) — a two-dimensional drawing API with a single scene graph and three interchangeable renderers: SVG, Canvas 2D, and WebGL. You describe shapes once; the chosen renderer draws them.

## Core API

```js
import Two from 'two.js';

const two = new Two({ fullscreen: true, type: Two.Types.svg }).appendTo(document.body);

const circle = two.makeCircle(72, 100, 50);
circle.fill = '#FF8000';
circle.noStroke();

const rect = two.makeRectangle(200, 100, 80, 40);
const group = two.makeGroup(circle, rect);
group.translation.set(100, 50);
group.rotation = Math.PI / 4;

two.bind('update', (frameCount, timeDelta) => {
  circle.rotation += 0.01;         // animate by mutating properties per frame
}).play();
```

- **Primitives**: `makeCircle`, `makeRectangle`, `makeLine`, `makeCurve`, `makePath`, `makeText`, `makeGroup`, plus gradients and sprites.
- **Scene graph**: every shape has `translation`, `rotation`, `scale`; groups nest and transform together.
- **Animation loop**: `two.play()` / `two.pause()`, with an `update` event giving `frameCount` and `timeDelta` — mutate properties inside it.
- **Renderer swap**: `Two.Types.svg | canvas | webgl` at construction; SVG output is inspectable/copyable DOM, Canvas/WebGL suit per-frame capture.

## When to reach for it (vs. the rest of our stack)

- **Vector scenes that must also be stills**: the same scene renders an SVG poster/thumbnail and an animated sequence — handy for video + thumbnail pairs.
- **Generative/procedural 2D art** and data-driven diagrams in the browser, where hand-rolled canvas code would sprawl.
- **Frame capture into video**: render Canvas/WebGL output per frame for the Remotion/ffmpeg pipeline when a scene is easier in a scene graph than in raw canvas calls.
- Prefer **Manim** for mathematical notation/graphs with LaTeX-grade typesetting, **Anime.js/Shifty** for tweening values (Two.js has no built-in easing engine — pair it with one of them: tween a proxy object, copy values into shape properties each frame).

## Guidance

- Keep an explicit proxy state object for animated values; the update handler should only copy state → shape properties, so easing engines stay in charge of timing.
- Choose SVG for crisp diagram exports, Canvas for capture-to-video, WebGL only when shape counts get large.
