---
title: Knowledge Base — Start Here
section: root
type: reference
confidence: established
sources: [own-synthesis]
updated: 2026-10-26
---

# 08_KNOWLEDGE_BASE

The text knowledge layer of the video production ecosystem. It holds rules, recipes, checklists and templates. It holds **no** footage, audio, LUTs, fonts or project files. Those go in the resource folders later.

## How an agent should use this

1. Read this file.
2. Open the section README for the job at hand (routing table below).
3. Read only the files that match the task. Do not load the whole base.
4. When a file says `confidence: verify-first`, check the official docs before relying on it.
5. After every project, write what you learned into `05_RESEARCH_NOTES/lessons-learned/` (see the loop in that folder).

## Routing table

| If the task is... | Start with |
|---|---|
| Plan a video from a brief | `02_VIDEO_EDITING/storytelling-and-pacing/brief-to-storyboard-method.md` |
| Choose a tone and the craft choices that follow | `02_VIDEO_EDITING/storytelling-and-pacing/tone-matrix.md` |
| Edit talking-head footage | `02_VIDEO_EDITING/editing-techniques/transcript-driven-editing.md` |
| Make a reel / Short | `02_VIDEO_EDITING/storytelling-and-pacing/short-form-playbook.md` |
| Add captions | `02_VIDEO_EDITING/captions-and-titles/caption-style-rules.md` |
| Animate text | `01_MOTION_DESIGN/typography-animation/kinetic-type-rules.md` |
| Design transitions | `01_MOTION_DESIGN/transitions-and-camera/transition-atlas.md` |
| Build a motion film in code | `04_SOFTWARE_MANUALS/other-tools/hyperframes-manual.md` or `remotion/remotion-core.md` |
| Fix or mix audio | `03_AUDIO_ENGINEERING/dialogue-cleanup/dialogue-chain.md` |
| Export / deliver | `03_AUDIO_ENGINEERING/loudness-and-export/delivery-specs.md` |
| Check work before delivery | `02_VIDEO_EDITING/editing-techniques/qc-checklist.md` |
| Run a command in ffmpeg | `04_SOFTWARE_MANUALS/ffmpeg/ffmpeg-cookbook.md` |

## File conventions

Every file starts with this frontmatter:

```yaml
---
title: Plain title
section: 01_MOTION_DESIGN/animation-principles
type: principle | rule | recipe | reference | template | log
confidence: established | creator-tested | verify-first
sources: [own-synthesis, herk-edit-kit, saas-motion-kit]
updated: YYYY-MM-DD
---
```

**Confidence levels**

- `established`: standard craft knowledge. Safe to apply.
- `creator-tested`: measured or learned by one creator in their own work (the reference kits). Treat as a strong starting point, not law. Break it on purpose, never by accident.
- `verify-first`: version-specific or number-specific (tool flags, platform specs, licences). Check the official source before relying on it.

## Writing rules for this base

- Plain, direct language. Short sentences. No filler.
- One idea per file. If a file passes about 250 lines, split it.
- Rules say *what to do* and *why*. A rule with no reason does not go in.
- Numbers always say where they come from.
- Never paste other people's text. Paraphrase, credit in `sources`, and record the credit in `SOURCES.md`.

## Index

`knowledge-index.csv` lists every file with its title, section, type, confidence and path. Rebuild it after any change:

```
python 08_KNOWLEDGE_BASE/_tools/build_index.py
```

## Open items (need your input)

- The third reference repo (`nextwork-projects/ai-video-editor-cooked`) could not be read. See `SOURCES.md`.
- Resource folders (LUTs, SFX, fonts, sample footage, reference clips, working project templates) are intentionally not here yet.
