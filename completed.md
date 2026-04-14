# Completed

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
