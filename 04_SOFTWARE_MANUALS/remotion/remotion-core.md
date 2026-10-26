---
title: Remotion Core Manual
section: 04_SOFTWARE_MANUALS/remotion
type: reference
confidence: verify-first
sources: [own-synthesis]
updated: 2026-10-26
---

# Remotion core manual

Remotion renders video from React components. A video is a function of the current frame.

> Verify against the official docs (remotion.dev) for your installed version. APIs below are the long-stable core.

## Start

```bash
npx create-video@latest
npx remotion studio          # live preview
npx remotion render <CompositionId> out/video.mp4
npx remotion still <CompositionId> out/frame.png --frame=30
```

## The core idea

All motion must be derived from the frame number. **CSS animations and transitions will not render correctly.** Compute every value from `useCurrentFrame()`.

## Registering a composition

```tsx
import { Composition } from "remotion";
import { MyVideo } from "./MyVideo";

export const RemotionRoot = () => (
  <Composition
    id="MyVideo"
    component={MyVideo}
    durationInFrames={300}
    fps={30}
    width={1080}
    height={1920}
  />
);
```

`registerRoot(RemotionRoot)` goes in the entry file. Each composition needs a unique `id`.

## Essential building blocks

| Item | Use |
|---|---|
| `useCurrentFrame()` | Current frame (relative inside a `<Sequence>`) |
| `useVideoConfig()` | `fps`, `width`, `height`, `durationInFrames` |
| `interpolate(frame, inputRange, outputRange, options)` | Map frame to a value. **Clamp** with `extrapolateLeft/Right: "clamp"` |
| `spring({ frame, fps, config })` | Physics-based 0→1 curve. `damping: 200` gives no bounce |
| `<AbsoluteFill>` | Full-frame layer |
| `<Sequence from durationInFrames>` | Time-shift and limit children. Frames restart at 0 inside |
| `<Series>` / `<Series.Sequence>` | Play children one after another |
| `<OffthreadVideo src>` | Embed video in render (preferred over the plain video tag for rendering) |
| `<Audio src volume>` | Add audio; `volume` can be a function of frame |
| `<Img src>` | Image that waits for load before rendering |
| `staticFile("name.mp4")` | Reference files in `public/` |
| `calculateMetadata` | Set duration or size from data or the length of a clip |

## Pattern: fade and rise in

```tsx
const frame = useCurrentFrame();
const opacity = interpolate(frame, [0, 12], [0, 1], {
  extrapolateLeft: "clamp",
  extrapolateRight: "clamp",
});
const y = interpolate(frame, [0, 12], [30, 0], {
  extrapolateLeft: "clamp",
  extrapolateRight: "clamp",
});
return <div style={{ opacity, transform: `translateY(${y}px)` }}>Hello</div>;
```

## Pattern: word-by-word reveal

Compute each word's start frame from its transcript timestamp (`Math.round(word.start * fps)`) and drive its opacity and transform from `frame - startFrame`. This keeps text tied to the audio.

## Transitions package

`@remotion/transitions` provides `TransitionSeries` with presentations such as `fade()`, `slide()`, `wipe()`, and `flip()`, and timing helpers like `linearTiming` and `springTiming`. Transitions overlap the two scenes, which shortens total duration. Account for that when you plan timing.

## Fonts and assets

- `@remotion/google-fonts` loads Google fonts in a render-safe way. For client work, prefer local fonts you have a licence for.
- Put media in `public/`. Use `staticFile()`.
- Wait for fonts before rendering (the package handles this).

## Rendering tips

- `--concurrency` controls parallel frames; lower it if memory is tight.
- Output codec and quality options are set in CLI flags or the config file (`remotion.config.ts`).
- For transparency, render to ProRes with alpha or a WebM with alpha, or PNG sequence.
- Render a still of the key frame first to check type and layout before a long render.

## Licensing

Remotion's licence has company-size conditions. As of our last check, small teams (up to a few people) and individuals can use it free, and larger for-profit companies need a company licence. **Check the current terms on remotion.dev/license before using it for client work.** Record the answer in `05_RESEARCH_NOTES/`.

## Gotchas

- No `Math.random()` without a seed. Use the `random("seed")` helper that ships with Remotion.
- Do not use `useEffect` timing for animation. Use the frame.
- Heavy effects (large blur, big shadows) slow renders. Pre-render if needed.
- Videos in the project must have a constant frame rate for exact sync. Convert variable frame rate sources first (see the ffmpeg cookbook).
