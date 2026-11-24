---
name: media-editing-skill
description: Skill for editing photo, video, and sound/audio using local tools and AI agent workflows. Use when working on media pipelines, Muse-style video generation, MakeFun/Make.fun projects, short-form content, captions, color, motion graphics, or any task involving ffmpeg, Whisper, Remotion, or motion design. Triggers on video edit, photo edit, audio clean, caption, reframe, silence remove, lip sync post, motion design, or media export.
---

# Media Editing Skill (Photo + Video + Sound)

This skill gives your agent practical, local-first capabilities for photo, video, and audio editing. It is built from the most useful Agent Skills and GitHub resources discussed on X (late 2026) around Muse, MakeFun.ai style pipelines, Remotion, FFmpeg, and motion design.

Use it for:
- Cleaning and cutting footage
- Adding captions / subtitles / karaoke
- Reframing for TikTok / Reels / Shorts (9:16)
- Audio loudness, denoise, music ducking
- Motion graphics overlays
- Character consistency and cinematic prompts (Muse style)
- Exporting platform-ready files

## Core Principles

1. Always probe the file first (ffprobe / probe.py).
2. Prefer lossless operations when possible (stream copy).
3. Never overwrite the source — write new files.
4. Verify output (duration, resolution, loudness, contact sheet).
5. Keep everything local unless the user explicitly wants a cloud API (Muse / MakeFun).

## Recommended Skills & Repos to Install Alongside

Install these for full power (run the install commands in your agent environment):

**Must-have (video editing core)**
- https://github.com/kajisho5/ffmpeg-skill  
  Install: `npx ffmpeg-skill` or `npx ffmpeg-skill --all`  
  42 local FFmpeg tools + MCP. Cut, join, silence remove, captions, reframe, loudness, platform export.

**Motion & animation**
- https://github.com/LottieFiles/motion-design-skill  
  Disney 12 principles, easing, timing, choreography for agents.
- https://github.com/remotion-dev/skills  
  Official Remotion agent skills for code-driven video.
- https://github.com/remotion-dev/remotion  
  React-based video engine.
- https://github.com/motion-canvas/motion-canvas  
  TypeScript animation for explainers.

**Muse / AI video generation helpers (character consistency, film crew)**
- https://github.com/LuoJiangYong/muse-video-skill  
  Virtual film crew (director, writer, DP, art, VFX, sound) + 38 technique cases.
- https://github.com/masonmosen/muse-video-prompt-skill  
  Cinematic prompts + character lock for single shots.
- https://github.com/sonlovinbot-team/Muse-animated-short-film  
  Full short-film pipeline (character → script → narration → edit).
- https://github.com/useapi/muse-api  
  Third-party Muse API bridge (check terms & privacy before use).

**Audio / speech**
- https://github.com/m-bain/whisperX  
  Accurate word-level timestamps + alignment (great for captions).
- https://github.com/librosa/librosa  
  Beat detection, audio analysis for motion-sync.

**Other useful**
- https://github.com/FujiwaraChoki/supoclip  
  Open-source short-clip / highlight style tool.
- https://github.com/n0an/ffmpeg-skill  
  Alternative categorized FFmpeg recipes.

**Photo / image**
- Use Muse / MakeFun image tools or local tools (ImageMagick, Pillow, or agent vision) for restore, colorize, face swap prep, consistency references.
- For layered editing prefer Seedream / similar models via MakeFun when available.

## Quick Workflows

### 1. Basic video clean-up + vertical export
```
Probe the file → remove silence / filler → cut best segments → reframe 9:16 → normalize loudness to -14 LUFS → burn captions → export for Reels/TikTok → verify.
```

### 2. Add professional motion / titles
Use Remotion or motion-design-skill principles + ffmpeg-skill overlays.  
Measure source with ffmpeg-skill first so graphics land on exact frames.

### 3. Muse / AI-generated clip consistency
1. Lock character with muse-video-prompt-skill or muse-video-skill.
2. Generate multiple shots.
3. Use ffmpeg-skill to join + color match + audio mix.
4. Add sound design (Openverse free SFX or generated).

### 4. Audio-first edit (podcast / interview style)
WhisperX → transcript is the timeline → cut by words → remove filler → level audio → optional BGM with ducking → export.

### 5. Photo to talking video / avatar
Generate or edit still with Muse/MakeFun → image-to-video or talking-photo → post-process with ffmpeg-skill (reframe, caption, loudness).

## FFmpeg Golden Rules (always apply)

- Use `-y` for overwrite of output only.
- Prefer `-c copy` when no filter is needed.
- For filters: re-encode with good preset (`-preset medium -crf 18` for quality).
- Audio: `-af loudnorm=I=-14:TP=-1.5:LRA=11` for streaming platforms.
- Vertical: scale + crop or pad carefully; keep subject centered when possible.
- Always check with `ffprobe` after write.

## Platform Delivery Targets (quick reference)

- YouTube: 1920x1080 or 4K, H.264/H.265, AAC, ~-14 LUFS
- TikTok / Reels / Shorts: 1080x1920, H.264, AAC 128k+, max duration varies
- X / Twitter: 1920x1080 preferred, short clips
- Podcast: MP3 320k or WAV, -16 LUFS mono/stereo

## How to Use This Skill

When the user asks for any media edit:
1. Identify input files and desired output (platform, length, aspect, captions?).
2. Probe first.
3. Plan the chain of operations.
4. Execute with the installed tools (prefer ffmpeg-skill scripts if present).
5. Verify and show contact sheet / metrics.
6. Offer next improvements (color, motion, sound design).

If a specialized skill (ffmpeg-skill, muse-video-skill, etc.) is installed, load and prefer it. This skill acts as the orchestrator and knowledge base.

## Installation for User

1. Copy the whole `media-editing-skill` folder into your agent’s skills directory  
   (e.g. `~/.claude/skills/`, Cursor skills folder, or equivalent).
2. Install the must-have companions:
   ```
   npx ffmpeg-skill
   ```
3. Optionally clone the Muse helpers:
   ```
   git clone https://github.com/LuoJiangYong/muse-video-skill.git
   # place into skills folder
   ```
4. Restart or reload the agent so it sees the new skill.

This single skill + the listed repos gives a complete local + AI-generation media editing toolkit suitable for Make.fun bounties, Muse pipelines, MakeFun.ai workflows, or any agent that needs to ship polished photo/video/sound.