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
- `archive/` — retired session artifacts and plan vessels; synthesis complete before archiving; human-readable record only; not indexed, not linted
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
- Archive is synthesis-complete. Nothing moves to `archive/` until all value has been distributed to project files. `archive/` contents are not LLM-accessible and will not be surfaced.
- `archive/` contents are intentionally unindexed — do not flag as orphans during lint.

### Scaffolding Protocol

1. Ask for project name if not given.
2. Ask how to refer to person if not obvious.
3. Ask project type if not obvious: **active** (tracked work) or **reference** (lookup content). Default active.
4. Create `<name>/` directory.
5. Copy appropriate template to `<name>/main.md` — `template.md` for active, `template-reference.md` for reference. Fill in name, goal, role.
6. Run `ln -s main.md <name>/CLAUDE.md`.
7. Copy `_filesys.md` into `<name>/_filesys.md`.
8. Create `<name>/.gitignore`: default ignores all subdirectories (`*/`) except `sources/` (`!sources/`, `!sources/**`). Assess two things: (a) any existing subdirectories with content worth tracking; (b) whether the project's domain naturally calls for subdirectories (e.g. characters/, models/, assets/) — discuss with builder and add `!<dir>/` + `!<dir>/**` exceptions for any agreed dirs.
9. Create `<name>/.claude/settings.json` with parent exclusion:
   ```json
   {
     "claudeMdExcludes": [
       "/home/jansen/Documents/projects/CLAUDE.md"
     ]
   }
   ```
   This prevents the factory CLAUDE.md from loading in child sessions. Machine-local; gitignored by `*/`.
10. Run `git init <name>/`. Stage scaffolded files (`main.md`, `_filesys.md`, `CLAUDE.md`, `.gitignore`), propose `"init commit"`, wait for explicit approval. Uses local git identity.
11. Create additional files only if immediate content exists. No stubs.

### Upgrading Existing Projects

`_filesys.md` updated → `cp _filesys.md <name>/_filesys.md`, propose `"upgraded _filesys.md"` per project, wait for approval before committing. Active and dormant always get upgrades; reference at judgement.

### New Project main.md Template

`template.md` for active. `template-reference.md` for reference.

## Status

Active. Core design validated; compliance audit complete across all 20 child projects. Before-v1 fixes implemented. @desirements.md load intentionally retained — factory is a meta-project that should know why it does things.

## Next Steps

_(empty)_

## Improvements

**High importance:**

- **[hi/lo] Define "vibes" concretely** — What's Next Protocol: "improvement most adjacent to builder's apparent current focus per `completed.md`; or, if no recent focus, shortest unblocked item regardless of domain." See `assessment.md#6-vibes-not-a-protocol`.

- **[hi/lo] Backlog ceiling** — cap at ~12 items; triage required before adding when full; add creation dates for aging. See `assessment.md#3-no-backlog-decay`, `assessment.md#6-vibes-not-a-protocol`.

- **[hi/lo] LLM vs builder content convention** — `>` blockquote for LLM-generated additions; behavioral rule against silently overwriting builder prose. See `assessment.md#5-context-poisoning-unaddressed`.

**Eventually:**

- **[lo/lo] Memory folder audit** — audit `~/.claude/projects/*/memory/`; migrate content to project files and clear folders. Blocked on factory v1.

- **[lo/lo] desirements↔_filesys sync protocol** — checklist or trigger for propagating changes between files. See `assessment.md#4-desirementsfilesys-sync-burden`.

- **[lo/lo] Source freshness convention** — when does immutable source need re-checking? See `assessment.md#eventually`.

- **[lo/lo] Search tooling** — scale-triggered: needed when project grows beyond what fits in context window. Recommended tools: qmd (hybrid BM25 + vector, CLI + MCP server) or DIY search script. See `sources/karpathy-llm-wiki.md#optional-cli-tools`.
