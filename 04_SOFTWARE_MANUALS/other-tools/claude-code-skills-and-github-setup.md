---
title: Claude Code Skills and GitHub Setup (How This Knowledge Base Plugs In)
section: 04_SOFTWARE_MANUALS/other-tools
type: recipe
confidence: verify-first
sources: [herk-edit-kit, saas-motion-kit, own-synthesis]
updated: 2026-10-26
---

# Claude Code skills and GitHub setup

How the two reference kits structure an agent-run video studio, and how to mirror that here. Verify specifics against current Claude Code docs.

## What the reference kits do

| Piece | Purpose |
|---|---|
| `CLAUDE.md` and `AGENTS.md` at the repo root | Always-loaded project instructions: rules, commands, conventions |
| `.claude/skills/<skill-name>/SKILL.md` | One skill per job (edit-video, short-form-edit, motion-showreel...). Invoked as `/skill-name` |
| Mirror for other agents (`.agents/skills`, `.codex`) | The Herk kit keeps skills in sync for both Claude Code and Codex |
| `docs/` | Human-readable playbooks. Every note becomes a rule |
| `templates/` | Brief, storyboard, reference ledger templates |
| `tools/` or `scripts/` | Small scripts: variety audit, silence cutting, preflight checks |
| `.github/workflows/` | An action that runs the audit on every push |
| `.gitignore` | Keeps raw footage, renders, and others' reference clips out of the repo |
| `.env.example` | Names the keys needed; real keys stay untracked |
| "Use this template" repo setting | Lets you start a new client project with everything in place |

## Recommended layout for this ecosystem

```
video-production-ecosystem/
├── CLAUDE.md                     # short: points to the KB and lists non-negotiables
├── .claude/skills/
│   ├── edit-talking-head/SKILL.md
│   ├── short-form-reel/SKILL.md
│   ├── motion-promo/SKILL.md
│   └── qc-and-deliver/SKILL.md
├── 08_KNOWLEDGE_BASE/            # this folder (text knowledge)
├── 09_RESOURCES/                 # later: LUTs, SFX, fonts, sample footage (Git LFS or external)
├── 10_PROJECTS/                  # per-client, git-ignored or separate repos
└── templates/
```

(Numbers 09 and 10 are suggestions. Name them as you like.)

## Skill design rules

1. **One skill, one job.** The description must say when to use it, in plain words, so the agent can pick it.
2. **Keep `SKILL.md` short.** It holds the steps and gates. Put knowledge in this base and point to it by path.
3. **Progressive loading.** The skill names exact files to read at each step (for example: "At step 4 read `tone-matrix.md` and `seam-design.md`"). The agent never loads the whole base.
4. **Gates.** The skill stops and asks for approval at brief, theme, storyboard, draft, sound, and final.
5. **Checks as scripts.** Anything measurable (loudness, caption contrast, variety repeats, lip sync frames) becomes a script, not a prompt.
6. **Every note becomes a rule.** After feedback, update the skill or the KB file. See `05_RESEARCH_NOTES/lessons-learned/note-to-rule-loop.md`.

## SKILL.md skeleton

```markdown
---
name: short-form-reel
description: Turn a talking-head recording into a 9:16 reel with hook, captions, B-roll and sound. Use when the user gives a raw recording and asks for a reel or Short.
---

# Short-form reel

## Before you start
Read: 08_KNOWLEDGE_BASE/02_VIDEO_EDITING/storytelling-and-pacing/short-form-playbook.md

## Steps
1. Transcribe (word-level). 
2. Cut retakes and dead air. Read: editing-techniques/transcript-driven-editing.md
3. Plan story. GATE: show the plan, wait for approval.
4. Source B-roll. Read: editing-techniques/b-roll-rules.md
5. Captions. Read: captions-and-titles/caption-style-rules.md
6. Mix. Read: 03_AUDIO_ENGINEERING/mixing-and-mastering/mix-hierarchy-and-levels.md
7. QC. Read: editing-techniques/qc-checklist.md. GATE: final watch-through.

## Never
- Invent numbers or results.
- Cut inside a phrase.
- Skip the QC checklist.
```

## GitHub practices

- Use a **private** repo for client-sensitive material and a public one only for generic knowledge.
- **Git LFS** for large binary resources (`.gitattributes` with patterns for `.cube`, `.wav`, `.mp3`, `.mp4`, `.psd`, fonts if large). Or keep binaries in cloud storage and store links in `knowledge-index.csv`.
- Ignore render outputs, raw media, and third-party reference clips.
- Commit messages that describe the rule changed ("captions: never over the mouth in first 2.5 s").
- Tag releases when a rule set is stable.
- Run the index builder in a pre-commit step or a GitHub Action so `knowledge-index.csv` never goes stale.
- Never commit API keys.

## Using this with a Claude Max plan

Claude Code can run skills and read this repo directly. Usage limits depend on your plan and change over time. Heavy rendering and checking runs on your machine, not on the plan, so keep scripts local and let the model handle planning, writing, and judgement. Check Anthropic's current plan documentation for limits.

## Licensing hygiene when learning from other kits

Before copying anything from a public kit, check its licence. Take ideas and measurements, rewrite in your words, credit the source, and never copy assets. See `05_RESEARCH_NOTES/technique-breakdowns/reference-credit-policy.md`.
