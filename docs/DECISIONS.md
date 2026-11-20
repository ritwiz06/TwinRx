# Decisions

Append only. Numbered D-001, D-002, and so on. Each entry states the decision and the reason.

## D-001 (2026-11-20): What git ignores

Decision: git ignores `.env`, `.cache/`, the contents of `data/raw/` and `data/locked/`, and large binary formats inside `runs/*/outputs/` (matrices, arrays, archives, slide images, model weights, video). Small text outputs in runs (csv, json, txt, md, log) are committed.

Reason: git cannot ignore by file size, so we ignore by format. Keeping small text outputs in git means every reported number can be traced to a committed run. Raw data is rebuilt from committed download scripts and checksums.
