@_filesys.md
@desirements.md
# Projects — Scaffold Factory

## Goal

Create new NLP file system projects on demand. When prompted with a project description, build the initial file structure according to the principles in `desirements.md`.

## Index

- `main.md` — this file; the project brain
- `CLAUDE.md` — symlink to main.md (backwards compatibility with Claude Code auto-load)
- `desirements.md` — design documentation and source of truth for NLP file system principles
- `_filesys.md` — operational principles template; copied into every new project. Covers ongoing self-maintenance: accumulation, splitting, surfacing, file types, task flow. Does not cover initialization (that is the factory's job).
- `template.md` — new project main.md template; used by the scaffolding protocol.
- `archive/plan-karpathy-integration.md` — Karpathy integration plan (completed; archived)
- `archive/claude-plan-karpathy-integration.md` — Claude Code plan file from the Karpathy integration session (archived)
- `completed.md` — append-only log of completed work
- `sources/` — directory of immutable ingested sources, converted to Markdown
- `sources/karpathy-llm-wiki.md` — Karpathy LLM Wiki pattern (immutable source)
- `research-pkm-llm.md` — synthesized research: PKM science, LLM failure modes, Claude-specific patterns
- `assessment.md` — factory assessment: critique, consistency issues, priority fixes

## Behaviors

- Refer to this person as "builder."
- When asked to create a new project, execute the scaffolding protocol below.
- Consult `desirements.md` when refining scaffolding behavior or resolving design questions.
- Design changes accumulate in this order: capture/discuss in main.md → expand with rationale in `desirements.md` → distill operational impact into `_filesys.md`.
- `_filesys.md` in child projects is factory-managed. Child project LLMs must not edit it. Updates are pushed deliberately from this factory by copying the updated `_filesys.md` into each project.
- Always show the builder a proposed commit message and wait for explicit approval before running `git commit`. Same for `git push`.

### Scaffolding Protocol

When asked to create a new project:

1. Ask for the project name if not given.
2. Ask how to refer to the person in this project if not obvious from context.
3. Create `<name>/` directory.
4. Copy `template.md` to `<name>/main.md`. Fill in the project name, goal, and role.
5. Run `ln -s main.md <name>/CLAUDE.md` to create the symlink (backwards compatibility with Claude Code auto-load).
6. Copy `_filesys.md` into `<name>/_filesys.md`.
7. Create additional files only if there is immediate content for them. No stubs.

### New Project main.md Template

See `template.md`.

## Status

Assessment complete. Core design validated; 6 critical issues identified in `assessment.md`. Before v1: remove @desirements.md bloat, add project lifecycle, cold-start protocol, structured completed.md entries, document @-import limitation.

## Next Steps

_(empty)_

## Improvements

**Before v1:**

- **[hi/hi] Remove @desirements.md from main.md** — load on demand only; update Behaviors: "read `desirements.md` when design questions or scaffolding decisions arise." Cuts session overhead from 440+ to ~180 lines. See `assessment.md#1-session-overhead-violates-design`.

- **[hi/hi] Project lifecycle states** — define active/reference/dormant/archived; add retirement protocol to scaffolding; add lightweight templates for reference and system doc project types. See `assessment.md#2-no-project-lifecycle`.

- **[hi/hi] Cold-start protocol** — add to `_filesys.md`: triggered when last session >7 days per `completed.md`; reads all active files, verifies Index, surfaces re-orientation summary before any work. See `assessment.md#4-no-cold-start-protocol`.

- **[hi/hi] Structured completed.md entries** — retrofit `[LINT]`/`[WORK]`/`[INGEST]` type prefixes on existing entries; enforce going forward. See `assessment.md#3-completedmd-entry-structure`.

- **[hi/hi] Document @-import limitation** — add to `_filesys.md`: @-imports load at session start and do not refresh mid-session. See `assessment.md#5-import-limitation-undocumented`.

**High importance:**

- **[hi/lo] Compliance audit** — audit all 20 child projects: rename `CLAUDE.md` → `main.md` + symlink, migrate `todo.md`, adopt `## Improvements`, switch to `@_filesys.md`, address 5 undocumented projects (cadoodle, census-data, clauding, floof, research), flag pomodoro runaway API bug. Each project is its own task. Blocked on factory v1.

- **[hi/lo] Define "vibes" concretely** — What's Next Protocol: "improvement most adjacent to builder's apparent current focus per `completed.md`; or, if no recent focus, shortest unblocked item regardless of domain." See `assessment.md#6-vibes-not-a-protocol`.

- **[hi/lo] Backlog ceiling** — cap at ~12 items; triage required before adding when full; add creation dates to items for aging. See `assessment.md#3-no-backlog-decay`, `assessment.md#6-vibes-not-a-protocol`.

- **[hi/lo] LLM vs builder content convention** — `>` blockquote for LLM-generated additions during a session; behavioral rule against silently overwriting builder prose. See `assessment.md#5-context-poisoning-unaddressed`.

**Eventually:**

- **[lo/lo] Memory folder audit** — audit `~/.claude/projects/*/memory/`; migrate content to project files and clear folders. Blocked on factory v1.

- **[lo/lo] desirements↔_filesys sync protocol** — checklist or trigger for propagating changes between the two files. See `assessment.md#4-desirementsfilesys-sync-burden`.

- **[lo/lo] Source freshness convention** — when does an immutable source need re-checking? See `assessment.md` §Eventually 11.

- **[lo/lo] Search tooling** — when a project grows beyond loadable size, consider qmd (hybrid BM25 + vector) or a DIY script. See `sources/karpathy-llm-wiki.md#optional-cli-tools`.
