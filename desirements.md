# NLP File System — Desirements

Patterns and principles observed across all projects, synthesized with external research. Source of truth for how projects here are structured and behave.

---

## What This Is

**NLP file system** — directory of Markdown files functioning as persistent LLM memory and operating context. All state on disk in human-readable files. LLM navigates, reads, writes, and maintains them across sessions.

Named equivalents:
- **Context-as-Code** — context externalized, version-controlled, persistent
- **Filesystem-first memory** — agents write artifacts to disk; remember through selective reading
- **Project Brain** — living context doc that eliminates re-explaining each session
- **Semantic File System (LSFS)** — NL queries + file content replace path-based commands; 15%+ retrieval accuracy improvement
- **Modular context** — topically coherent files split by load trigger; monolithic = anti-pattern

---

## main.md — The Project Brain

Every project has `main.md` as entry point and clearinghouse. `CLAUDE.md` is a symlink for Claude Code auto-load compatibility — `main.md` is the actual file.

**main.md always references outward. No other file references back.**

File needs to cite content in main.md → extract first. Citation need = split trigger.

### Standard Sections

```
# Project Name
## Goal
## [Domain sections — extracted to files as they grow]
## Index               ← catalog of all files; always includes main.md and CLAUDE.md symlink; splits to index.md when grown
## Behaviors           ← LLM behavioral instructions for this project (see below)
## Next Steps          ← active task list (always here; see below)
```

### Principles

- **Under ~200 lines** — curated ruthlessly. Only what LLM wouldn't get right without it.
- **References out, never duplicates** — point to where details live.
- **First-person singular** — "my", "I", never "our/we".
- **No hidden folders** — all notes are `.md` files in project directory or subdirectories.
- **Self-maintaining** — at session end ("prep to exit"), current state and next steps written back.

---

## Next Steps — Active Task List

Lives permanently in `main.md ## Next Steps`. Intentionally small. Never splits.

**Rules (GTD + Personal Kanban):**
- **3–7 items max** — WIP limit. Full → new items to improvements, not lost. Ceiling = finish or capture, not stop recording.
- **Next actions only** — concrete, unblocked, doable now. Not projects or vague goals. Can't write as physical action → improvement.
- **Right scope:** "add entry to pieces.md" — wrong scope: "improve the pipeline"
- Completed → completed.md.

**Badly-scoped items** → improvements. Improvements = holding space for any scope. When surfaced: what's next concrete action, or new project?

**Blocked items** don't belong in Next Steps. Identified → ask permission to migrate to improvements with tag. Before surfacing any new work.

**What's next? protocol** — asked what to do next, or Next Steps empty/all-blocked:
1. Check Next Steps for blocked items. Ask permission to migrate each to improvements with suggested tag. Handle all blocked first.
2. Unblocked remain → surface one. Empty → scan improvements (hi/hi first, then lo/hi or hi/lo on context and vibes, then lo/lo).
3. Pick **one** — 60% vibes, 40% priority. Single binary yes/no. Not a list.
4. Yes → create concrete next action citing via `file.md#anchor`.

**Prep for exit** — person signals wrap-up → audit everything touched; save to permanent files or session notes. Nothing touched lives only in chat.

---

## Section and File Types

New content lands in `main.md` first. Section grows large enough to stand alone → split to named file.

Split cycle:
1. Content accretes in main.md section.
2. Grows coherent and large → extract to named `.md` file.
3. Replace section in main.md with one-line reference.
4. Add file to Index.
5. New file: references out, first-person singular, no duplication.

File types:

| Type | Purpose | Examples |
|---|---|---|
| **Log** | Append-only history | `completed.md`, `session-YYYY-MM-DD.md` |
| **Backlog** | Tagged improvement queue; items pull into Next Steps | `improvements.md` or `## Improvements` in main.md |
| **Catalog** | Inventory of domain entities | `pieces.md`, `filaments.md`, `characters/` |
| **Process** | Step-by-step how-to for recurring workflow | `slicer.md`, `assembly.md` |
| **Reference** | Dense lookup material | `glossary.md`, `tools.md` — `## Glossary` starts in main.md, splits when grown |
| **Exploration** | Unstructured ideation | `ideas.md` |
| **Research** | Findings from external sources | `topic-notes.md` |
| **Formal** | Authored documents | `policy.md`, `lessons.md` |
| **Behaviors** | Accumulated behavioral corrections | `behaviors.md` |
| **Index** | Catalog of all files and sections | starts as `## Index` in main.md; splits to `index.md` when grown |
| **Sources** | Immutable raw ingested material | `sources/<name>.md` — converted to Markdown at ingest; never edited |

### Backlog

`## Improvements` in main.md or `improvements.md`. Tagged by importance × urgency:

- **[hi/hi]** — Implement proactively before/during next active work cycle.
- **[lo/hi]** — Consider for current or next work cycle.
- **[hi/lo]** — High importance, not urgent. Surface during downtime or lulls.
- **[lo/lo]** — Surface when wistful with no hi/lo items remaining.

Tag definitions belong in the backlog. When to surface: project-specific, declared in Behaviors.

**Surfacing = clarification moment.** LLM raises improvement → next concrete action? → Next Steps. Large enough for own NLP file system → new project.

### Task Flow

```
capture   →  improvements (tagged — any scope, including badly-scoped or multi-step)
surface   →  LLM raises at right moment; clarify into next action or new project
pull      →  main.md ## Next Steps (concrete, unblocked)
finish    →  completed.md (append-only log)
```

---

## Ingest

Ingest workflow:

1. Fetch or read source.
2. Convert to clean Markdown — strip HTML, add heading structure if plain text, convert PDFs. Required for anchor support.
3. Save to `sources/<name>` with immutable header:
   ```
   > **Immutable source.** Do not edit. Fetched YYYYMMDD-HHMMSS.
   > Original: <url or provenance>
   ```
4. Discuss and synthesize.
5. Distribute synthesis across all relevant files — one ingest may touch Process, Reference, Behaviors, and more simultaneously.
6. Log in `completed.md` with source name, path, all files touched.

Receiving files cite `sources/<name>#<anchor>`, not raw URL. Oversized sources → store key excerpts. Sources immutable — descriptive name, never edited after creation.

**File-back on query:** valuable synthesis (comparison, analysis, connection) → file into project, not chat. Explorations compound.

---

## Lint / Periodic Audit

Periodic self-check: orphaned files, stale anchors, contradictions, knowledge gaps.

When to run:
- **Session start**: check `completed.md` for last lint entry; absent or >36h → run lint before any work
- During prep-for-exit
- Session touches many files
- After major accumulation cycle

LLM-initiated: builder shouldn't need to ask.

---

## Cold-Start Protocol

Triggered when last entry in `completed.md` is older than 7 days. Supersedes warm-start lint.

Problem: after gap, LLM arrives with no recent context, runs continuity checklist. Sees lint timestamp but no sense of stable state. Re-entry cost must be near-zero; "what's next?" answerable in <60 seconds.

Protocol:
1. Read all files in Index.
2. Verify Index against actual files on disk — flag orphans or missing.
3. Check for drift: stale Next Steps, blocked items not marked blocked, irrelevant improvements.
4. Surface single re-orientation summary: current state, last work done, one recommended next action.
5. Proceed normally.

---

## Section Headings and Cross-References

### Heading Length

**3–5 words** after any tag prefix. Detail in body. Short headings = clean, stable anchors — heading change breaks every link.

- Good: `## [hi/hi] Oil finish rags`
- Avoid: `## [hi/hi] Acquire new rags for use during oil finish application`

**Headings, not bold text, for referenceable content.** Bold has no anchor. Heading auto-generates one: lowercase, spaces → hyphens, special chars stripped. `### My Section` → `#my-section`. Need to link → make it heading first.

### Cross-References

`file.md#anchor` syntax:

```
Cut rabbet on all stock — see `improvements.md#hihi-frame-rabbet-fit`
```

Anchors auto-generated: lowercase, spaces → hyphens, special chars stripped. `[hi/hi]` → `hihi-` in anchor.

Not clickable everywhere, but precise human-readable pointers that survive copy-paste and search.

**Bidirectional where meaningful.** A references B → check if back-ref belongs in B. Behavioral rule: when writing any reference, stop and check before moving on.

### Cross-Project References

Content accumulates in one project but belongs to a sibling project. Move it — don't duplicate.

Pattern: content in project A belongs in project B → move to B, replace in A with one-line pointer (`../B/file.md`). Update B's Index. Commit each repo separately; propose both commits together and wait for single approval.

Example: art project accumulated wall-mounting procedure. Belongs to home project. Moved to `../home/hanging.md`; art/assembly.md now points there.

When to apply: content is general to a domain outside the current project; would be useful elsewhere; would duplicate or go stale if kept in both places.

---

## Behaviors

Behavioral instructions in `main.md ## Behaviors` or `behaviors.md` once split. Tell LLM how to operate: role, startup ritual, proactive actions, conventions, corrections. Start inline; split using same split-when-coherent rule.

**All corrections go here explicitly.** Correction made → write into Behaviors immediately. Nothing behavioral accumulates silently in hidden folders.

Common behavior types:
- **Session startup** — what to read at session start
- **Role vocabulary** — how to refer to person using this project
- **Proactive surfacing** — what to surface without being asked, and when
- **Accumulation triggers** — when to append to log/catalog/backlog
- **Tool usage** — which tools for which actions
- **Archive behavior** — when session notes move to `archive/`
- **Ingest log** — every ingest appended to `completed.md` with source name, path, all files touched
- **Lint trigger** — session start: check last entry date in `completed.md`; >7 days → Cold-Start Protocol; otherwise lint if last lint >36h. Also at prep-for-exit, heavy sessions, major accumulation.
- **Cross-reference** — writing ref to another file → immediately check if back-ref belongs in target
- **Git commits** — task complete → propose commit message + file list; wait for explicit approval before committing or pushing

---

## Role Vocabulary

Each project names who the human is, declared in Behaviors. If unspecified at creation, ask: *"How should I refer to you? (artist, player, educator, user, ...)"*

---

## Session Notes

Written to `session-YYYY-MM-DD.md` in project root. After integration into permanent files, moved to `archive/`. Not read unless explicitly requested.

---

## No Stubs

No file until content exists.

---

## Project Lifecycle

Every project has a state. State determines apparatus and maintenance.

| State | Description | Apparatus |
|---|---|---|
| **active** | Being worked on; open next actions or improvements | Full: Next Steps, Improvements, Behaviors, lint, cold-start |
| **reference** | Lookup/how-to content; not a tracked project | Index + content only; no task machinery |
| **dormant** | Was active; stalled on external blocker; not abandoned | Status declares blocker; Improvements preserved; lint still runs |
| **archived** | Closed, done, or abandoned; read-only | Status declares archived; no further maintenance |

Declared in `## Status` in `main.md`.

### Retirement Protocol

**Active → dormant**: All Next Steps blocked, no unblocked work → confirm dormancy with builder. Status: `Dormant. Waiting for: [X].` Empty Next Steps. Keep Improvements.

**Active → reference**: Project revealed to be reference material (no tasks, just content). Strip Next Steps and Improvements. Status: `Reference. No task tracking.` Restructure main.md if needed.

**Active/dormant → archived**: Finished or explicitly abandoned. Status: `Archived [date]. [Reason].` No further lint or cold-start.

**Reference/dormant → active**: Work resumes or tasks emerge. Update Status. Restore apparatus if removed.

---

## Persistence Layers

| Layer | Location | What belongs here |
|---|---|---|
| **Project files** | `<project>/*.md` | Everything — domain knowledge, behaviors, tasks, logs, corrections |
| **Immutable sources** | `<project>/sources/` | Raw ingested material — converted to Markdown at ingest; never edited |
| **Global LLM config** | `~/.claude/CLAUDE.md`, settings | Behavior applying to all projects globally |
| **Project memory folder** | `~/.claude/projects/<path>/memory/` | Should remain empty. Anything here → migrate to project files. |

Memory folder = fallback; should not be used. Corrections, vocabulary, conventions → `## Behaviors` or `behaviors.md`.

---

## Scaffolding a New Project

1. Ask for project name.
2. Ask how to refer to person, if not obvious.
3. Ask project type if not obvious: **active** (tracked work) or **reference** (lookup content). Default active.
4. Create `<name>/` directory.
5. Copy appropriate template to `<name>/main.md` — `template.md` for active, `template-reference.md` for reference. Fill in name, goal, role.
6. Run `ln -s main.md <name>/CLAUDE.md`.
7. Copy `_filesys.md` into `<name>/_filesys.md`. Factory-managed; child LLMs must not edit.
8. Create `<name>/.gitignore`: default ignores all subdirectories (`*/`) except `sources/` (`!sources/`, `!sources/**`). Assess two things: (a) any existing subdirectories with content worth tracking; (b) whether the project's domain naturally calls for subdirectories (e.g. characters/, models/, assets/) — discuss with builder and add `!<dir>/` + `!<dir>/**` exceptions for any agreed dirs.
9. Run `git init <name>/`. Stage scaffolded files (`main.md`, `_filesys.md`, `CLAUDE.md`, `.gitignore`), propose `"init commit"`, wait for explicit approval. Uses local git identity.
10. Create additional files only if immediate content exists. No stubs.

Both templates include caveman mode on by default. Remove the behavior line to disable per project.

### Upgrading Existing Projects

`_filesys.md` updated → push to child projects deliberately. Never auto-pushed.

**Version stamp process:**
1. Make content changes to `_filesys.md`. Commit factory — this is the *content commit*.
2. `git rev-parse --short HEAD` → get hash H (the content commit).
3. Update `**Filesys version:**` line in `_filesys.md` to H.
4. Commit factory: `"stamped hash <H>"`.

**Distribution:**
5. `cp _filesys.md <name>/_filesys.md` for all active and dormant projects.
6. Commit each child: `"upgraded _filesys.md (<H> <content-commit-msg>)"` — H and message from the content commit (step 1–2), not the stamp. Propose all child commits together; wait for single approval.
7. Reference projects: judgement call — apply convention updates; skip task-flow-only changes.

---

## Desirements Summary

1. **Filesystem is the memory.** All durable state in `.md` files — including learned behaviors.
2. **Single entry point.** `main.md` is actual project brain. `CLAUDE.md` is symlink for Claude Code compatibility.
3. **Session startup ritual.** Startup files injected via `@filename` harness expansion — not behavioral reads. LLM sees them because harness loads at session start. @-imports don't refresh mid-session — edits take effect only at next session start.
4. **Active list is small.** 3–7 WIP-limited next actions in `main.md ## Next Steps`. Never a separate file. New items always captured to improvements.
5. **Backlog is tagged.** Improvements holds deferred work with hi/lo importance × urgency tags. Items pull into Next Steps when space opens.
6. **Start monolithic, split when coherent.** New content into `main.md` first. Extract when section stands alone or another file needs to cite it.
7. **Know the file type.** Use taxonomy to name and structure new files correctly.
8. **Short headings, precise references.** 3–5 word headings. `file.md#anchor` syntax. Headings not bold text for referenceable content — only headings generate anchors.
9. **Reference over duplication.** Files point to each other; no content repeated.
10. **Role clarity.** Each project names who the human is. Ask if unspecified.
11. **Self-maintaining.** `main.md` updated at natural milestones. Project documents itself.
12. **Sources are immutable.** Ingested material in `sources/` as clean Markdown, frozen at creation. Synthesized files cite `sources/<name>#<anchor>`, not raw URLs.
13. **Ingest is an operation.** Source enters project → convert to Markdown, save to `sources/`, synthesize across all relevant files, log in `completed.md`.
14. **File back.** Valuable query syntheses in project files, not chat. Explorations compound.
15. **Lint periodically.** Orphans, stale refs, contradictions at session start (if >36h since last), at prep-for-exit, after heavy sessions or major accumulation.
16. **Projects have lifecycle states.** active/reference/dormant/archived. Declared in `## Status`. State determines apparatus. Factory has retirement and upgrade protocols.
17. **Version-controlled from init.** Git init on scaffold. Each task completion → proposed commit + file list; explicit approval required. Never commit or push without approval.
18. **Terse by default.** Caveman mode in all new projects (~65% output token reduction). Per-project configurable — remove behavior line to disable.

---

## Literature Sources

- [GTD in 15 minutes – A Pragmatic Guide to Getting Things Done](https://hamberg.no/gtd) — next actions: concrete, unblocked, doable now
- [Your Next Actions List Template: 2 Simple Rules](https://www.shortform.com/blog/next-actions-list-template-gtd/) — scope and granularity of next actions
- [What are Work In Progress Limits in Personal Kanban](https://flow-e.com/personal-kanban/wip/) — WIP limit of 3 as starting point; pull-when-ready
- [Kanban WIP Limits: Definition, Benefits & Best Practices](https://teamhood.com/kanban-resources/kanban-wip-limits/) — ceiling means finish or capture to backlog, not stop recording
- [CLAUDE.md Best Practices – UX Planet](https://uxplanet.org/claude-md-best-practices-1ef4f861ce7c) — ~200 line ceiling, curated ruthlessly
- [Writing a good CLAUDE.md – HumanLayer](https://www.humanlayer.dev/blog/writing-a-good-claude-md) — clearinghouse pattern
- [Context-as-Code: Building a Shared Brain for AI Agents](https://medium.com/@marvin-lijma/how-to-build-a-shared-brain-for-ai-agents-the-context-as-code-pattern-873b87804f0b) — externalized, version-controlled context
- [Project Brains: Organizing Complex Initiatives](https://elezea.com/2026/02/project-brains-organizing-complex-initiatives-for-ai-assisted-work/) — living context documents
- [From Monolithic Prompts to Modular Context](https://dev.to/salt_creative/from-monolithic-prompts-to-modular-context-a-practical-architecture-for-agent-memory-1lcp) — split by topic when coherent; monolith is anti-pattern
- [From Commands to Prompts: LLM-based Semantic File System](https://openreview.net/forum?id=2G021ZqUEZ) — topically coherent files improve NL retrieval accuracy
- [Building Effective AI Agents – Anthropic](https://www.anthropic.com/research/building-effective-agents) — filesystem-first memory architecture
- [The GTD Approach to Linking Next Actions and Projects](https://gettingthingsdone.com/2020/06/the-gtd-approach-to-linking-next-actions-and-projects/) — only identify immediate next action; don't decompose all steps upfront
- [Projects vs. Next Actions – Ask MetaFilter](https://ask.metafilter.com/217774/Projects-vs-Next-Actions) — badly-scoped items belong in backlog, not next actions list
- [LLM Wiki — Andrej Karpathy](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) — persistent wiki pattern: immutable sources → LLM-maintained wiki → schema; ingest/query/lint operations; maintenance cost near-zero enables compounding knowledge bases
- `research-pkm-llm.md` — synthesized research: PKM science, LLM failure modes, Claude-specific patterns; primary evidence base for factory design decisions
