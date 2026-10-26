---
title: Transcript-Driven Editing (Talking-Head Cuts)
section: 02_VIDEO_EDITING/editing-techniques
type: recipe
confidence: creator-tested
sources: [herk-edit-kit]
updated: 2026-10-26
---

# Transcript-driven editing

How to cut talking-head footage using a word-level transcript, so the edit is tight without sounding chopped.

## Workflow

1. **Transcribe** with word-level timestamps (a cloud transcription service or a local Whisper-type model).
2. **Remove retakes.** Keep the last clean take of each line.
3. **Cut dead air between phrases.** Do not cut inside a phrase.
4. **Plan the story** (hook, proof, items, payoff, call to action) before any visuals.
5. **Source B-roll**, one visual per noun phrase.
6. **Build the composition**, anchoring each visual to the spoken word it shows.
7. **Mix** voice, music, effects.
8. **Render a draft, check it, fix, render final.**

## Speech cutting rules

- **A phrase is what you say in one breath.** Cut between phrases at real silence. Never cut inside one.
- **Keep natural beats.** The short pause after a comma or after "First," makes speech sound human. Pauses between phrases stay under about 0.2 s; beats after a comma or list marker may run 0.2–0.4 s and should stay.
- **Fillers:** remove a filler only when there is silence on both sides. If it runs straight into the next word, cutting it clips the words around it. Keep it.
- **Never speed up the voice** to tighten the edit.
- **Word endings:** transcripts often mark a word's end a little before the consonant fully dies (about 40–80 ms in some tools). Cut after a word has fully decayed.
- **Two takes join only where both sides are silent.** If they run together, play one take whole.
- **Fewer cuts, all between phrases, beats many clever micro-cuts.** One case study found the cleaner version ran several seconds longer and sounded better.

## Sync safety

- Encoding each segment separately and concatenating can drift lip-sync by about a frame at some joins. Select exact frame numbers in a single pass instead.
- After cutting, compare every cut frame against the raw footage. A matching frame count alone does not prove the right frames were chosen.
- Download the full source and cut locally. Seeking over a network for short clips can take far longer than copying the file.

## Hook first

A raw recording caps the edit. If the take has no hook and no call to action, no editing fixes that. Script the hook before recording.

## Raw reel (light style)

No B-roll, no music. Cut retakes, stutters, and dead air only. One word on screen at a time, small, lower middle, synced to the exact word. When the speaker names a tool and points, pop its logo where they point.
