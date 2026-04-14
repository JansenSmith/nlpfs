@_filesys.md
@desirements.md
# Projects — Scaffold Factory

## Goal

Create new NLP file system projects on demand. When prompted with project description, build initial file structure per `desirements.md`.

## Index

- `main.md` — this file; project brain
- `CLAUDE.md` — symlink to main.md (Claude Code auto-load compatibility)
- `desirements.md` — design documentation and source of truth for NLP file system principles
- `_filesys.md` — operational principles template; copied into every new project. Covers self-maintenance: accumulation, splitting, surfacing, file types, task flow. Not initialization (factory's job).
- `template.md` — new active project main.md template
- `template-reference.md` — new reference project main.md template
- `archive/plan-karpathy-integration.md` — Karpathy integration plan (completed; archived)
- `archive/claude-plan-karpathy-integration.md` — Claude Code plan file from Karpathy integration session (archived)
- `completed.md` — append-only log of completed work
- `sources/` — immutable ingested sources, converted to Markdown
- `sources/karpathy-llm-wiki.md` — Karpathy LLM Wiki pattern (immutable source)
- `sources/caveman.md` — Caveman terse-output skill: always-on snippet, install, benchmarks, skills (immutable source)
- `research-pkm-llm.md` — synthesized research: PKM science, LLM failure modes, Claude-specific patterns
- `assessment.md` — factory assessment: critique, consistency issues, priority fixes

## Behaviors

- Refer to person as "builder."
- New project request → execute scaffolding protocol below.
- Consult `desirements.md` when refining scaffolding or resolving design questions.
- Design changes: capture/discuss in main.md → expand with rationale in `desirements.md` → distill operational impact into `_filesys.md`.
- `_filesys.md` in child projects is factory-managed. Child LLMs must not edit it. Updates pushed deliberately from this factory.
- Always show builder proposed commit message and wait for explicit approval before `git commit`. Same for `git push`.

### Scaffolding Protocol

1. Ask for project name if not given.
2. Ask how to refer to person if not obvious.
3. Ask project type if not obvious: **active** (tracked work) or **reference** (lookup content). Default active.
4. Create `<name>/` directory.
5. Copy appropriate template to `<name>/main.md` — `template.md` for active, `template-reference.md` for reference. Fill in name, goal, role.
6. Run `ln -s main.md <name>/CLAUDE.md`.
7. Copy `_filesys.md` into `<name>/_filesys.md`.
8. Run `git init <name>/`. Stage scaffolded files (`main.md`, `_filesys.md`, `CLAUDE.md`), propose `"init commit"`, wait for explicit approval. Uses local git identity.
9. Create additional files only if immediate content exists. No stubs.

### Upgrading Existing Projects

`_filesys.md` updated → `cp _filesys.md <name>/_filesys.md`, propose `"upgraded _filesys.md"` per project, wait for approval before committing. Active and dormant always get upgrades; reference at judgement.

### New Project main.md Template

`template.md` for active. `template-reference.md` for reference.

## Status

Assessment complete. Core design validated; 6 critical issues identified in `assessment.md`. Before v1: remove @desirements.md bloat, add project lifecycle, cold-start protocol, structured completed.md entries, document @-import limitation.

## Next Steps

_(empty)_

## Improvements

**Before v1:**

- **[hi/hi] Remove @desirements.md from main.md** — load on demand only; update Behaviors: "read `desirements.md` when design questions or scaffolding decisions arise." Cuts session overhead from 440+ to ~180 lines. See `assessment.md#1-session-overhead-violates-design`.

- **[hi/hi] Project lifecycle states** — define active/reference/dormant/archived; add retirement protocol to scaffolding; lightweight templates for reference and system doc types. See `assessment.md#2-no-project-lifecycle`.

- **[hi/hi] Cold-start protocol** — add to `_filesys.md`: triggered when last session >7 days per `completed.md`; reads all active files, verifies Index, surfaces re-orientation summary before any work. See `assessment.md#4-no-cold-start-protocol`.

- **[hi/hi] Structured completed.md entries** — retrofit `[LINT]`/`[WORK]`/`[INGEST]` type prefixes on existing entries; enforce going forward. See `assessment.md#3-completedmd-entry-structure`.

- **[hi/hi] Document @-import limitation** — add to `_filesys.md`: @-imports load at session start and do not refresh mid-session. See `assessment.md#5-import-limitation-undocumented`.

**High importance:**

- **[hi/lo] Compliance audit** — audit all 20 child projects: rename `CLAUDE.md` → `main.md` + symlink, migrate `todo.md`, adopt `## Improvements`, switch to `@_filesys.md`, address 5 undocumented projects (cadoodle, census-data, clauding, floof, research), flag pomodoro runaway API bug. Each project own task. Blocked on factory v1.

- **[hi/lo] Define "vibes" concretely** — What's Next Protocol: "improvement most adjacent to builder's apparent current focus per `completed.md`; or, if no recent focus, shortest unblocked item regardless of domain." See `assessment.md#6-vibes-not-a-protocol`.

- **[hi/lo] Backlog ceiling** — cap at ~12 items; triage required before adding when full; add creation dates for aging. See `assessment.md#3-no-backlog-decay`, `assessment.md#6-vibes-not-a-protocol`.

- **[hi/lo] LLM vs builder content convention** — `>` blockquote for LLM-generated additions; behavioral rule against silently overwriting builder prose. See `assessment.md#5-context-poisoning-unaddressed`.

**Eventually:**

- **[lo/lo] Memory folder audit** — audit `~/.claude/projects/*/memory/`; migrate content to project files and clear folders. Blocked on factory v1.

- **[lo/lo] desirements↔_filesys sync protocol** — checklist or trigger for propagating changes between files. See `assessment.md#4-desirementsfilesys-sync-burden`.

- **[lo/lo] Source freshness convention** — when does immutable source need re-checking? See `assessment.md` §Eventually 11.

- **[lo/lo] Search tooling** — project grows beyond loadable size → consider qmd (BM25 + vector) or DIY script. See `sources/karpathy-llm-wiki.md#optional-cli-tools`.
