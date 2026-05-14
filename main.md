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
- `process-doc-prompts.md` — student-facing LLM prompt: interview rules for co-creating process docs

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

**High importance:**

- **[hi/lo] Adjacent-style communication recognition** — builder often communicates intent via *adjacent* things they would expect to hear, not literal dictation. Example: framing a methodology rule by speaking it aloud in builder's own personal style ("when I'm ready, I'll let you know"). Treat as illustrative signal of gist, not quoted template to mimic. Two implications: (1) don't render builder's voice verbatim into docs; describe the rule directly. (2) recognize quoted illustrative phrases as adjacent communication, not literal text to adopt.

- **[hi/lo] Don't cite Claude** — use NLPFS terminology only: "LLM" for the agent, "builder" or the project-specific role term (e.g. "wizard") for the human. Product name "Claude" leaks substrate, couples to implementer, breaks if agent changes.

- **[hi/lo] Define "vibes" concretely** — What's Next Protocol: "improvement most adjacent to builder's apparent current focus per `completed.md`; or, if no recent focus, shortest unblocked item regardless of domain." See `assessment.md#6-vibes-not-a-protocol`.

- **[hi/lo] Backlog ceiling** — cap at ~12 items; triage required before adding when full; add creation dates for aging. See `assessment.md#3-no-backlog-decay`, `assessment.md#6-vibes-not-a-protocol`.

- **[hi/lo] LLM vs builder content convention** — `>` blockquote for LLM-generated additions; behavioral rule against silently overwriting builder prose. See `assessment.md#5-context-poisoning-unaddressed`.

- **[hi/lo] Source file content convention** — `_filesys.md` Ingest section says "convert to Markdown" but doesn't distinguish between ingest (faithful transcription of original) and synthesize (distribute to project files). LLMs collapse these into one step and write synthesis into the source file. Fix: clarify in `_filesys.md` Ingest that `sources/<name>.md` must be a faithful markdown rendering of the original; synthesis distributes separately to project files. These are two distinct operations. Temporary local version in `art/main.md` Behaviors — remove after pushing downstream.

- **[lo/hi] Commit message guidance** — current style guidance (lowercase, past tense, no period) doesn't address substance. LLMs default to listing touched files, which duplicates the diff. Add to `_filesys.md` Git section: "describe what changed and why — not which files were touched; files are visible in the diff." Temporary local version in `art/main.md` Behaviors — remove after pushing downstream.

- **[hi/lo] "show X" opens markdown in vmd** — when builder says "show X" and X resolves to a `.md` file, run `Bash(vmd <resolved-path> &)`. Resolution should be contextual: use project Index, Next Steps, and session context to map fuzzy references (e.g. "show me the draft" → nearest draft `.md`). Add as universal behavior in `_filesys.md` Behaviors. Requires `vmd` installed (`npm install -g vmd`).

- **[lo/hi] vmd auto-refreshes on save** — `_filesys.md` Behaviors should note that vmd auto-refreshes when the underlying `.md` file is written; LLM does not need to re-run `vmd <file> &` after edits. Opening a second instance adds clutter. Only open vmd when the user asks to see the file; after that, edits appear automatically.

- **[hi/lo] Document vmd in scaffold template** — child projects should know `vmd` is the CLI markdown viewer (`vmd <file.md>`, auto-refreshes on save). Add a note to `_filesys.md` Behaviors or the scaffold template so child LLMs don't need to be told every session. See `../vmd/main.md` for patches and details.

- **[hi/lo] Document markdown footnotes in _filesys.md** — vmd supports standard markdown footnotes: inline `[^1]`, definition `[^1]: text`. Place definitions immediately after the section they annotate. Add as a note in `_filesys.md` Behaviors alongside the vmd behavior. See `../vmd/main.md#footnote-support` for install, patch, and usage example.

- **[hi/lo] Scaffold clipboard behavior** — when asking the user to run a command in an external terminal, pipe it to `wl-copy` so it lands in their clipboard. Consider adding to `_filesys.md` Behaviors as a universal pattern (Wayland-specific; may need platform guard).

- **[hi/lo] Citation validation protocol** — AI role: guide human to source (URL + what to look for), assess passages human provides, flag whether claim appears supported. Human role: open source independently, read it, provide relevant passages to AI, make final determination. AI marks status field only after human confirms. Add as universal behavior in `_filesys.md` or scaffold template Behaviors.

- **[hi/lo] Scaffold default permissions audit** — some tool permissions (e.g. `wl-copy`, web search) recur across many projects and may be worth including in the scaffold `.claude/settings.json` by default. Review which permissions appear in most project settings and evaluate case-by-case whether they belong in the template. Surfaced from omarchy project where wl-copy clipboard use is a per-session pattern.

- **[hi/lo] Commit proposal required — add to _filesys.md Git** — factory Behaviors (line 33) already specifies: always show proposed commit message + file list, wait for explicit approval before `git commit` or `git push`. Not in `_filesys.md`, so child sessions don't inherit it. Add to `_filesys.md` Git section. Failure mode: LLM inferred approval from general closure language without surfacing the proposal step.

- **[lo/hi] Session-scoped commit staging** — on commit proposal, auto-stage only files the LLM touched in the current session. Any other modified files in `git status` get mentioned explicitly ("also modified, not touched this session — stage manually if intended") but not staged. Add to `_filesys.md` Git section. Reduces crossed commits when multiple sessions work the same repo; does not help when two sessions touch the same file — flag that case explicitly so builder can resolve.

- **[hi/lo] Disable Claude in `~`** — remove the Claude session instance for the home directory (history at `~/.claude/projects/-home-jansen/`) and specifically disable Claude from running with `~` as cwd. Home isn't a project and shouldn't be a default Claude entry point — accidental sessions there accumulate cruft and have no project context. Ad-hoc sessions in other random folders are fine; this is just about blocking `~` itself. Decide mechanism (settings, hook, or wrapper) and execute.

**Eventually:**

- **[lo/lo] Memory folder audit** — audit `~/.claude/projects/*/memory/`; migrate content to project files and clear folders. Blocked on factory v1.

- **[lo/lo] desirements↔_filesys sync protocol** — checklist or trigger for propagating changes between files. See `assessment.md#4-desirementsfilesys-sync-burden`.

- **[lo/lo] Source freshness convention** — when does immutable source need re-checking? See `assessment.md#eventually`.

- **[lo/lo] completed.md efficiency** — append-only logs should stay oldest-first (natural, simple `echo >>`). Cold-start protocol reads completed.md to find last entry date — with oldest-first, use `tail -30 completed.md` instead of reading the whole file. Add to `_filesys.md`: cold-start reads `tail -30 completed.md`; completed logs default oldest-first.

- **[lo/lo] Search tooling** — scale-triggered: needed when project grows beyond what fits in context window. Recommended tools: qmd (hybrid BM25 + vector, CLI + MCP server) or DIY search script. See `sources/karpathy-llm-wiki.md#optional-cli-tools`.

- **[lo/lo] Behavioral hooks for commit workflow** — behaviors in `main.md` aren't reliably self-applied at commit time. Two hooks: (1) `PreToolUse` on Bash — intercept `git commit` to enforce propose-first before commit fires; (2) `PostToolUse` on Bash — inject summary + next-step transition reminder after commit completes. Pattern-matching on `git commit` in Bash command string is imprecise — a dedicated git tool would allow cleaner, more targeted hooks (worth advocating upstream). Investigate via `update-config` skill; generalize as pattern in `_filesys.md` Behaviors once validated.
