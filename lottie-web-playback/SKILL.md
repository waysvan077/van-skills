---
name: lottie-web-playback
description: Lottie-web — play After Effects animations exported as JSON (Bodymovin) natively on the web with SVG, Canvas, or HTML renderers. Use when a scene needs polished, designer-grade vector animation (icons, loaders, characters, transitions) that already exists as a Lottie file, or must be scrubbable/frame-exact inside web content or a video pipeline.
---

# Lottie Web

Authored by Van from [airbnb/lottie-web](https://github.com/airbnb/lottie-web) (32.5K stars, MIT) — the original Lottie player. Animations are authored in After Effects, exported as JSON via the Bodymovin plugin, and rendered at runtime as real vector graphics — resolution-independent, tiny compared to video, and fully controllable from code.

## Core API

```js
import lottie from 'lottie-web';

const anim = lottie.loadAnimation({
  container: document.getElementById('stage'),
  renderer: 'svg',              // 'svg' | 'canvas' | 'html'
  loop: true,
  autoplay: true,
  path: 'animation.json'        // or animationData: {...} inline
});

anim.play(); anim.pause(); anim.stop();
anim.goToAndStop(45, true);     // frame-exact scrubbing (value, isFrame)
anim.goToAndPlay(10, true);
anim.setSpeed(1.5); anim.setDirection(-1);
anim.playSegments([0, 30], true); // play only a frame range
anim.addEventListener('complete', onDone);
anim.addEventListener('enterFrame', e => { /* e.currentTime */ });
```

- **Renderers**: SVG (crisp, DOM-inspectable, best for UI), Canvas (better for heavy compositions and frame capture), HTML (rare).
- **Frame-exact control** (`goToAndStop`) makes Lottie scrubbable by an external clock — the key property for syncing it to narration or capturing it into video.
- **Expressions/dynamic props**: layer colors/text can be overridden at runtime via `getKeyPath` + property callbacks in the SVG renderer.

## When to reach for it (vs. the rest of our stack)

- **Designer-grade assets that already exist as Lottie JSON** — animated icons, loaders, mascots, transitions (check the asset's license before commercial use; LottieFiles assets vary).
- **Interactive web scenes** where animation must respond to hover/click/scroll with real vector quality at any size.
- **Inside video**: `@remotion/lottie` embeds a Lottie asset in a Remotion composition and drives it by the composition frame — the cleanest bridge into our video pipeline.
- Not an authoring tool: Lottie *plays* animations made elsewhere. For authoring motion from code, use Anime.js / Motion / Shifty / Ola; for math diagrams, Manim.

## Guidance

- Keep compositions modest: deeply nested precomps and effects unsupported by the player are the usual cause of "looks different on web".
- Prefer `animationData` inline for self-contained/offline pages; external `path` adds a network fetch.
- For capture-to-video, drive frames deterministically with `goToAndStop(frame, true)` instead of wall-clock playback.
