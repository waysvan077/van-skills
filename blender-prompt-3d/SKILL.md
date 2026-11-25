---
name: blender-prompt-3d
description: One-shot prompt → 3D render — drive Blender headlessly with a procedural bpy Python script to build a branded object (car, product, mascot scene) from a text brief, apply image decals and a dominant brand color, and render a presentable still (or turntable) without any manual modelling. Use when the user wants a 3D object/scene "like that Blender one-shot demo", a branded 3D render, or a 3D asset for content.
---

# Blender Prompt 3D — one-shot prompt to branded render

Distilled from the "Gemini Neon" GT-R demo Ways shared (satyaXBT, 2026-11-25): an LLM writes ONE Python script, Blender executes it headlessly, out comes a finished 3D render. No manual modelling, no external model downloads — everything procedural. First run: the VAN-Unit 07 pink car (2026-11-25, in flight when this skill was saved — update this line with the verdict).

## Setup on this VM

- apt is broken here; use the **official standalone tarball**: download Blender 4.2 LTS (or newer stable 4.x) Linux x64 from https://download.blender.org/release/ into `~/workspace/tools/blender/`, extract with `tar --no-same-owner`.
- Verify headless before building: `blender --version` and a trivial `blender -b --python -c "import bpy"` run.
- VM reality: ~1.5 shared CPU / 6 GB RAM. Keep geometry modest (tens of objects, low subdiv), render Eevee first. A 4K Cycles render like the demo is NOT realistic here — 1280×720 Eevee is the working default.

## The build pattern

1. **One self-contained bpy script** (e.g. `~/workspace/tools/blender-car/build_car.py`): clear the default scene, build the object from primitives with bevels/shaping, assign materials, add decals, set scene + camera + lights, render. One script = reproducible + editable for round 2.
2. **Object construction that reads well at a glance:** main mass (beveled boxes / shaped meshes), secondary masses (cabin, bumpers, skirts), dark glass, wheels (tire + rim as separate cylinders), emissive headlights/taillights, small details (spoiler, mirrors) only after the silhouette works.
3. **Dominant brand color:** one hero paint material (glossy, low roughness) covering most of the body; ONE accent color sparingly (stripes, calipers, rim light gels). For VAN-Unit 07: hot pink `#FF4FA3` + neon-green accents.
4. **Image decals:** the brand portrait/logo goes on as thin planes with an image texture placed hugging the target panels (both side doors + hood for a car), offset a few mm off the surface to avoid z-fighting, rotated to face outward. Check in the render that the decal is visible, upright, and not mirrored.
5. **Scene:** dark studio backdrop, ground plane (slightly glossy), two colored area/rim lights matching the brand palette, camera at a 3/4 hero angle that shows the main decal.
6. **Render:** `BLENDER_EEVEE_NEXT`, 1280×720 PNG. Fallback: Cycles at 64 samples + denoise if Eevee fails on the VM.

## Verify before delivering (non-negotiable)

- Read the rendered image back and check: (a) the object reads as the intended thing, (b) dominant color is actually dominant, (c) the decal is visible and correct, (d) not a black/empty/blown-out frame.
- If it fails a check, edit the script and re-render — one or two iterations is normal and expected; a presentable stylized result beats an unfinished realistic attempt.
- Report honestly what is procedural/stylized vs realistic. Never pass off a still image generation as a Blender render or vice versa.

## Extensions

- **More angles / turntable:** re-render the same script with the camera on a circular path; stitch frames with ffmpeg for a turntable clip.
- **Video:** assemble renders + generated backdrops in ffmpeg/Remotion with camera moves (push-in, pan, slide) and quick beat cuts; soundtrack must be ORIGINAL (style emulation is fine — e.g. a synthesized drift-phonk loop at ~140 BPM: kick, hats, sub bass, cowbell-style lead) — never the copyrighted track a style references.
- **Asset reuse:** the same script pattern works for products, game props, and mascot scenes — swap the construction section, keep setup/decal/scene/render sections.

## Pitfalls

- Downloading model packs or asset libraries defeats the point (and the demo's claim); stay procedural.
- Decals placed exactly ON the surface z-fight; planes floating too far look pasted. A few mm offset + slight conform to the panel angle is the sweet spot.
- Eevee material/texture defaults differ from Cycles — check the actual render, not the script's intentions.
- Long renders on this VM: run them detached/backgrounded and poll the output file, never block the chat.
