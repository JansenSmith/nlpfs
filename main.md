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
- `project-list.md` — per-project roster of all NLPFS children (state, role, description); updated on scaffold and lifecycle transitions
- `archive/` — retired session artifacts and plan vessels; synthesis complete before archiving; human-readable record only; not indexed, not linted
- `completed.md` — append-only log of completed work
- `sources/` — immutable ingested sources, converted to Markdown
- `sources/karpathy-llm-wiki.md` — Karpathy LLM Wiki pattern (immutable source)
- `sources/caveman.md` — Caveman terse-output skill: always-on snippet, install, benchmarks, skills (immutable source)
- `research-pkm-llm.md` — synthesized research: PKM science, LLM failure modes, Claude-specific patterns
- `assessment.md` — factory assessment: critique, consistency issues, priority fixes
- `process-doc-prompts.md` — student-facing LLM prompt: interview rules for co-creating process docs
- `commit-approval-hardening.md` — incident report + prevention options for the propose-and-wait commit guard (2026-05-15 violation in `art`); supports the [hi/lo] nonce-in-marker improvement

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

See `desirements.md#upgrading-existing-projects` for full process. Summary: content commit → stamp hash → `cp _filesys.md <name>/_filesys.md` for active and dormant → commit each child as `"upgraded _filesys.md (<H> <msg>)"`, propose all together.

### New Project main.md Template

`template.md` for active. `template-reference.md` for reference.

## Status

Active. Compliance audit complete (21 projects at time of audit; 26 total as of 2026-05-07). Improvements actively accumulating for downstream filesys push. @desirements.md load intentionally retained — factory is a meta-project that should know why it does things.

## Next Steps

_(empty)_

## Improvements

See `improvements.md`.
