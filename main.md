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
- `completed.md` — append-only log of completed work
- `sources/` — directory of immutable ingested sources, converted to Markdown
- `sources/karpathy-llm-wiki.md` — Karpathy LLM Wiki pattern (immutable source)

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

In production. Karpathy integration complete — `desirements.md`, `_filesys.md`, and `template.md` updated with ingest/lint/sources patterns and `@_filesys.md` import. Still needs builder assessment before declaring v1.

## Next Steps

_(empty)_

## Improvements

- **[hi/hi] Builder assessment** — builder does a full read-through of `desirements.md` and `_filesys.md` and gives a verdict: what's missing, what's wrong, what's ready. Required before factory can be declared v1. Blocks the compliance audit.

- **[hi/lo] Audit existing projects for compliance** — once the scaffold factory and `_filesys.md` are finalized, audit all existing project subfolders and bring them into compliance: rename `CLAUDE.md` → `main.md` + symlink, migrate `todo.md` into `main.md ## Next Steps`, migrate memory folder contents into project files, adopt `## Improvements` backlog structure, apply heading length conventions. Each project is its own task.

- **[hi/lo] Child project @-imports** — all existing child project main.md files use unreliable "At session start, read `_filesys.md`" instructions; switch each to `@_filesys.md` during compliance audit.

- **[lo/lo] Memory folder audit** — audit all existing project memory folders (`~/.claude/projects/*/memory/`); migrate any content to the relevant project files and clear the folders. Blocked on factory v1.

- **[lo/lo] Search tooling** — when a project grows large enough that the LLM can't load all files, consider qmd (hybrid BM25 + vector, CLI + MCP) or a DIY script. See `sources/karpathy-llm-wiki.md#optional-cli-tools`.
