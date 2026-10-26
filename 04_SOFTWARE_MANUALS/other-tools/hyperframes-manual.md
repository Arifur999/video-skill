---
title: HyperFrames Manual (HTML + GSAP Video)
section: 04_SOFTWARE_MANUALS/other-tools
type: reference
confidence: verify-first
sources: [saas-motion-kit, herk-edit-kit]
updated: 2026-10-26
---

# HyperFrames manual

HyperFrames (by HeyGen) lets an agent write video as HTML + GSAP and render it to MP4. Facts below come from how the two reference kits use it. **We have not read the HyperFrames docs directly.** Verify against `github.com/heygen-com/hyperframes` before relying on any detail.

## Requirements (per the kits)

- Node.js 22 or newer
- ffmpeg and ffprobe
- A headless Chrome (HyperFrames downloads its own)
- An agent that can use skills (Claude Code or Codex)

## Install the HyperFrames skills

```bash
npx skills add heygen-com/hyperframes
```

## Everyday commands (as used in the kits)

```bash
npx hyperframes lint                       # check the project
npx hyperframes preview                    # live preview in Studio
npx hyperframes render --quality draft --output renders/draft.mp4
npx hyperframes render --quality delivery --resolution landscape-4k --output renders/final-4k.mp4
npx hyperframes add <block>                # install a registry block
```

Blocks mentioned in the kits include whip-pan, cross-warp-morph, flash-through-white, swirl-vortex, cinematic-zoom, chromatic-radial-split, and grain-overlay. Registry blocks give speed. A hand-built piece gives reference quality.

## Composition anatomy (from the kits)

- A stage element with a `data-composition-id`.
- Each timed layer is an element with the `clip` class and these attributes: `data-start` (seconds), `data-duration` (seconds), `data-track-index` (layer order).
- Animation is a **paused GSAP timeline** registered at `window.__timelines["<composition-id>"]`. The key must match the composition id exactly.
- Audio elements are siblings in the root, each with an explicit `data-volume`.
- Captions live outside the master composition, as body-level siblings, on high track numbers (20 and up), so they are not hidden by sub-composition timing.

## Rules that prevent the common failures

1. **Fill the slot.** End every timeline with a no-op tween that spans the full slot duration. Otherwise a sub-composition may be hidden early and flash black.
2. **Seek-safe.** Everything is a function of time. No `Math.random()` or `Date.now()`. One paused timeline. Drive Three.js or canvas work from the seek event or a proxy tween.
3. **Bracket video.** Use a poster still before and a last-frame still after any `<video>`.
4. **Tween end times on frame boundaries.**
5. **No `gsap.defaults()`.** Declare ease and duration on every tween.
6. **Local fonts and libraries** for final renders. Public CDNs are fine for drafts.
7. **View the frames.** Lint passing is not the same as design working.

## Render workflow

1. Lint.
2. Preview and scrub.
3. Draft render; look at frames.
4. Render at 4K; downscale with Lanczos for delivery (see `03_AUDIO_ENGINEERING/loudness-and-export/delivery-specs.md`).
5. Run the QC checklist.

## Gotchas from the kits

- Fonts with non-Latin glyphs need local subsets.
- Uppercase CSS transforms change letters in some languages.
- Live WebGL can stall a headless render; use Canvas 2D fallback.
- Footage and large renders stay out of Git (use ignored folders or release assets).
- A progress bar at the bottom makes a brand film look like a screen recording. Leave it out.

## Where this fits

Use HyperFrames for motion films, product promos, and animated explainers. Use Remotion when the project is data-driven React. Use ffmpeg for everything around them.
