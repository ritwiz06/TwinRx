# TwinRx: working contract for Claude Code

## What this project is

TwinRx shows both sides of a cancer drug at once. On one side, a living simulation of a real patient's tumor, started from the cells in their biopsy slide (Slide-to-Sim). On the other, a 3D body that lights up wherever healthy cells carry the drug's targets (Drug Body Map). A window gauge compares the two.

The full plan lives in the shared plan doc. This file is the short version you must follow.

## Who you are working with

Ritik is a CS master's student and a beginner to intermediate ML engineer who wants to learn while building.

- **Teach before using any new concept.** Give plain language, why it is needed here, one analogy, and one alternative. One concept at a time. No quizzes.
- **Small steps.** No large code dumps. Write a small piece, run it, show the real output, then continue.
- **Style.** Natural, direct writing. Never use double hyphens anywhere: code comments, docs, commit messages, or chat.

## Current phase and scope

Pre-pitch prototype, November 2, 2026 to January 3, 2027. Colorectal cancer only.

| Weeks | Focus |
| --- | --- |
| 1 to 3 | Drug Body Map: 50 drugs, single-cell hit map, 3D body view, tissue-level baseline |
| 4 to 6 | Slide-to-Sim: TCGA slide regions, cell finder, simple tumor twin, MSI check |
| 7 to 8 | Join both sides through the drug, window gauge, Ask why, final numbers, video |

Out of scope until after the pitch: Sol, whole slides, other cancers, survival analysis, public release, and any outreach.

## Hard rules

1. **Never claim success without evidence.** After any change, run it and show the output. For UI work, show a screenshot or describe exactly what rendered. "Done" means verified, not written.
2. **Every number traces to a run.** Each run writes `runs/<run_id>/` with the command, config, git commit, outputs, and a log. Every reported number gets a WORKLOG entry pointing to its run.
3. **Locked drugs stay locked.** The 25 drugs in `configs/drugs_locked.yaml` are for the final test only. Their side-effect answers live in `data/locked/` and are read only by `scripts/score_locked.py`, run once in week 8. Never open, print, summarize, or score them before then.
4. **Simulation rules are visible.** Every rule and parameter lives in `configs/twin_rules.yaml` with either a source or the word "assumption".
5. **Language models see summaries only.** Never send patient-level rows to any model, even public TCGA data. Cache every model call by content hash in `.cache/llm/`. Pin model IDs in `configs/models.yaml`.
6. **Research sandbox language.** Any screen or text says this is a research sandbox, not medical advice. Never predict an individual patient's outcome.
7. **Public data only.** Record each dataset's license in DECISIONS. Raw data is git-ignored; commit download scripts and checksums instead.
8. **Generality.** A new cancer type or drug list must need only a config change. If code edits are needed, treat it as a generality bug and fix it generally.
9. **Non-destructive.** Never delete data or runs. Move old material to `archive/`.
10. **Report weak results plainly**, including where the Body Map fails.
11. **Stop on failure.** If a step fails (a key, a network call, a license question), stop and explain. Never work around it silently.

## Repo layout

```text
twinrx/
  CLAUDE.md
  docs/            PROJECT_STATUS, WORKLOG, DECISIONS, CONCEPTS, ROADMAP, HANDOFF, ENVIRONMENT, PITCH
  configs/         cancer type, drugs_tuning.yaml, drugs_locked.yaml, twin_rules.yaml, models.yaml
  data/raw/        downloads (git-ignored)
  data/locked/     locked drugs' answers (git-ignored, read by one script only)
  src/common/      run logging, cached LLM client, paths
  src/drugs/       ChEMBL targets, OnSIDES side effects, organ grouping
  src/bodymap/     single-cell hit map, tissue baseline, scoring
  src/slides/      TCGA download, region cutting, cell finder
  src/twin/        simulation rules and export for the app
  src/link/        tumor RNA scaling, window gauge
  src/ask/         Ask why: retrieval over stored evidence, cited answers
  scripts/         entry points, including score_locked.py
  runs/<run_id>/   one folder per run
  app/             React and Three.js
  tests/           fast unit tests
  archive/         retired material, never deleted
```

## Docs to keep current

- **PROJECT_STATUS:** where things stand right now, in under a page.
- **WORKLOG:** append-only, dated, one entry per session, with run IDs.
- **DECISIONS:** append-only, numbered D-001, D-002, and so on, each with the reason.
- **CONCEPTS:** one beginner-friendly entry per concept taught.
- **ENVIRONMENT:** machine, OS, RAM, GPU or Apple MPS, disk, Python and key package versions.
- **HANDOFF:** what a fresh session needs to continue.

## Model access

ASU Voyager LLM gateway, OpenAI-compatible, base URL `https://openai.rc.asu.edu/v1`. The key lives in `.env` as `VOYAGER_API_KEY` and is never committed. Copy model IDs exactly as the Voyager portal lists them.

## How each task runs

1. Restate the goal in one or two sentences.
2. Teach any new concept first.
3. Take the smallest useful step.
4. Run it and show the real output.
5. Update WORKLOG and PROJECT_STATUS.
6. Stop at each checkpoint and wait for Ritik's review.
