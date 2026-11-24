---
name: manim-math-animation
description: Manim (Community Edition) — the Python framework behind 3Blue1Brown-style mathematical animations. Use when a video needs precise animated diagrams, graphs, formulas, number lines, or step-by-step visual proofs — explainers where the geometry/math itself is the content.
---

# Manim Community Edition

Authored by Van from [ManimCommunity/manim](https://github.com/ManimCommunity/manim) (42.7K stars, MIT) — a Python framework for programmatic mathematical animations, rendered scene-by-scene to video.

## Mental model

A video is a set of **Scenes**. Each Scene's `construct()` places **Mobjects** (mathematical objects: text, formulas, shapes, axes, graphs) and plays **Animations** that create, transform, or remove them over time. Manim renders the scene to an mp4 — deterministic and frame-exact, no screen recording.

```python
from manim import *

class PriceLevels(Scene):
    def construct(self):
        axes = Axes(x_range=[0, 10], y_range=[0, 100])
        curve = axes.plot(lambda x: 60 + 20 * np.sin(x), color=BLUE)
        self.play(Create(axes))
        self.play(Create(curve), run_time=2)
        label = MathTex(r"61{,}400").next_to(curve, UP)
        self.play(Write(label))
        self.wait()
```

Render: `manim render -qm scene.py PriceLevels` (`-ql` low/preview, `-qm` medium, `-qh` high; output under `media/videos/`).

## Core vocabulary

- **Mobjects**: `Text`, `MathTex` (LaTeX formulas), `Circle/Square/Line/Arrow`, `Axes`, `NumberLine`, `Graph` via `axes.plot()`, `Dot`, `DecimalNumber` (animated counters), `VMobject` for custom paths.
- **Animations**: `Create`, `Write`, `FadeIn/FadeOut`, `Transform` (morph one mobject into another — the signature Manim move), `ReplacementTransform`, `Indicate`, `MoveAlongPath`; `self.wait()` holds a beat.
- **Layout**: `.next_to()`, `.shift()`, `.scale()`, `.to_edge()`, `VGroup` for treating many mobjects as one.

## When to use it in our work

- **Explainer videos where the diagram is the argument**: price levels on a chart, how liquidation math works, probability trees, compounding curves — drawn precisely and morphed step by step as narration proceeds.
- **Education content** (math, physics, CS concepts) in the 3Blue1Brown idiom.
- Not for character/story scenes (that's the Remotion + image/video stack) and not for DOM interactivity (Anime/Motion territory).

## Environment caveats (this VM)

- Python package `manim` needs system deps (Cairo, Pango, ffmpeg — ffmpeg is present); `MathTex` additionally needs a LaTeX install, which is heavy — prefer `Text` unless formulas are essential, or verify LaTeX availability before promising MathTex output.
- Rendering is CPU-bound; keep scenes at `-qm` for drafts and expect minutes, not seconds, per scene on this machine.
- Install in a venv (`pip install manim`) under `~/workspace` so it survives VM replacement.
