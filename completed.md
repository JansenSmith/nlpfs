# Completed

## [2026-04-16] [WORK] Filesys versioning + distribution

Added `**Filesys version:**` line to `_filesys.md` (short factory commit hash). Documented two-step version stamp process and child commit format in `desirements.md` upgrading section. Distributed to all 21 child projects.
Files touched: `_filesys.md`, `desirements.md`, all 21 × `<project>/_filesys.md`.

## [2026-04-16] [LINT] Session lint

Orphans: none. Stale: `main.md#upgrading-existing-projects` (old commit format, no version stamp — contradicts desirements); `## Status` says 20 child projects, actual is 21.

## [2026-04-15] [WORK] Per-child claudeMdExcludes settings

Created `.claude/settings.json` in all 19 child projects excluding `projects/CLAUDE.md` from loading as a parent. Prevents factory context from bleeding into child sessions. No ancestor-specific mechanism exists in Claude Code — blanket per-child exclusion is the only option. Scaffolding protocol updated to create this file for new projects automatically.
Files touched: `main.md`, `completed.md`; 19 × `<project>/.claude/settings.json` (machine-local, gitignored).

## [2026-04-14] [LINT] Factory lint

Orphans: none (sources/ clean; archive/ plan files correct). Stale improvements removed: 4 done [hi/hi] before-v1 items + "Before v1" section header. @desirements.md removal improvement dismissed — intentional retention. Status section updated to reflect current state.
Files touched: `main.md`, `completed.md`.

## [2026-04-14] [WORK] Compliance audit — all 20 child projects

Full compliance audit executed across all 20 child projects. Disposals: research/, census-data/. New scaffolds: cadoodle, clauding, floof. Reference compliance: audio, discord, ff7, imagemagick, orca. Dormant: proton, pomodoro (moved from resources/pomodoro/). Active compliance: aaacme, ai-teaching, dnd, home_assistant, malcom, omarchy (renamed from updates/), vex_sort, art. Each project received: main.md + CLAUDE.md symlink + _filesys.md + @_filesys.md header, Status declared, Index complete, Behaviors with role + caveman, git initialized.
Files touched: all 20 child project directories.

## [2026-04-14] [WORK] Pre-audit factory cleanup

Archive policy established: synthesize-before-archive rule, archive/ unindexed and untracked. Gitignore scaffolding step added to scaffolding protocol (blanket `*/` + `!sources/`; assess domain subdirs). Karpathy search tooling rationale expanded in [lo/lo] improvement. _filesys.md session notes clarified.
Files touched: `main.md`, `_filesys.md`, `desirements.md`.

## [2026-04-14] [WORK] Before-v1 fixes and caveman refactor

Implemented all before-v1 fixes except removing @desirements.md (deferred for assessment). Git protocol added to scaffolding and _filesys.md. Cold-start protocol added. @-import limitation documented. Project lifecycle states + retirement/upgrade protocols added. Structured completed.md entries with type prefixes. Desirements alignment pass (startup claim, cross-refs, summary items 16-18). Caveman compression on all propagating files (_filesys.md, desirements.md, main.md, templates). Assessment.md internal consistency headings converted to proper ### headings; cross-references updated to file.md#anchor syntax throughout.
Files touched: `main.md`, `desirements.md`, `_filesys.md`, `template.md`, `template-reference.md`, `assessment.md`, `completed.md`.

## [2026-04-14] [INGEST] Caveman integration

Ingested JuliusBrussee/caveman README. Caveman cuts ~65% output tokens via terse behavioral rules; March 2026 paper confirms brief responses improve accuracy. Always-on snippet added to `template.md` and `template-reference.md` as default configurable behavior. Source saved as `sources/caveman.md`.
Files touched: `sources/caveman.md` (new), `template.md`, `template-reference.md`, `main.md`, `completed.md`.

## [2026-04-13] [WORK] Factory assessment

Researched PKM science, LLM failure modes, and Claude-specific patterns. Audited all 20 child project subdirs. Wrote `research-pkm-llm.md` (synthesized research) and `assessment.md` (critique: 6 critical issues, 5 internal consistency issues, 14 priority fixes). Updated `main.md` Improvements with all priority fixes; converted assessment internal consistency items to proper headings; updated cross-references to `file.md#anchor` syntax.
Files touched: `research-pkm-llm.md` (new), `assessment.md` (new), `main.md`, `completed.md`.

## [2026-04-13] [WORK] Karpathy integration

Updated `desirements.md`, `_filesys.md`, `template.md`, `main.md` per `plan-karpathy-integration.md`.
Added ingest/lint/sources patterns, `@_filesys.md` + `@desirements.md` imports, bidirectional cross-references, 36-hour lint trigger at session start.
`## Project Structure` renamed to `## Index` across all files.
Source: `sources/karpathy-llm-wiki.md`.
Files touched: `desirements.md`, `_filesys.md`, `template.md`, `main.md`.

## [2026-04-23] [WORK] Process doc student prompt

Spidered all 25 child projects via Index sections; collected 12 process docs into `/tmp/process-docs/`.
Created `process-doc-prompts.md` — caveman-style LLM interview prompt for student process doc co-creation.
Files touched: `process-doc-prompts.md` (new), `main.md`.

## [2026-04-23] [LINT] Prep-for-exit lint

Files on disk match Index. No orphans, no stale refs, no contradictions.
Status updated: audit count (21) vs current count (25) clarified; improvements-for-filesys-pushdown work noted as active.
