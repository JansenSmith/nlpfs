# NLP File System — Desirements

Patterns and principles observed across all projects in this directory, synthesized with external best-practice research. This is the source of truth for how projects here are structured and how they should behave.

---

## What This Is

A **NLP file system** is a directory of Markdown files that functions as the persistent memory and operating context for an LLM. Rather than relying on chat history or model memory, all state lives on disk in human-readable files. The LLM navigates, reads, writes, and maintains these files as its primary mode of operation across sessions.

Named equivalents in the literature:
- **Context-as-Code** — context externalized, version-controlled, and persistent
- **Filesystem-first memory** — agents write artifacts to disk and remember through selective reading
- **Project Brain** — a living context document that eliminates re-explaining project details each session
- **Semantic File System (LSFS)** — NL queries and file content replace traditional path-based commands; shown to improve retrieval accuracy 15%+ over path navigation
- **Modular context** — topically coherent files split by load trigger; monolithic context is the anti-pattern

---

## main.md — The Project Brain

Every project has a `main.md` as its entry point and clearinghouse. `CLAUDE.md` is a symlink to `main.md` for backwards compatibility with Claude Code's auto-load behavior — `main.md` is the actual file.

**main.md always references outward. No other file references back to it.**

If a file needs to cite content that currently lives in main.md, that content should be extracted to its own file first — the citation need is a split trigger.

### Standard Sections

```
# Project Name
## Goal
## [Domain sections — extracted to files as they grow]
## Index               ← catalog of all files with one-line descriptions; always includes main.md and CLAUDE.md symlink; splits to index.md when entries grow beyond a flat list
## Behaviors           ← LLM behavioral instructions for this project (see below)
## Next Steps          ← active task list (always here; see below)
```

### Principles

- **Under ~200 lines** — curated ruthlessly. Only what the LLM wouldn't get right without it.
- **References out, never duplicates** — point to where details live.
- **First-person singular** — "my", "I", never "our/we".
- **No hidden folders for project notes** — all notes are `.md` files in the project directory or subdirectories.
- **Self-maintaining** — at session end ("prep to exit"), current state and next steps are written back.

---

## Next Steps — Active Task List

The active task list lives permanently in `main.md` as `## Next Steps`. It is intentionally small and never splits to a separate file.

**Rules (from GTD + Personal Kanban):**
- **3–7 items maximum** — Personal Kanban WIP limit. When the list is full, new items go to improvements rather than being lost — hitting the ceiling is a signal to finish something or capture to improvements, not to stop recording.
- **Next actions only** — each item is concrete, unblocked, and doable right now (GTD). Not projects or vague goals. If an item can't be written as a specific physical action, it's an improvement — capture it there instead.
- **Right scope:** "add entry to pieces.md" — wrong scope: "improve the pipeline"
- Completed items move to completed.md.

**Badly-scoped items** belong in improvements. GTD distinguishes next actions from larger goals that require clarification before they can be acted on. Improvements serve as that holding space — any scope. When surfaced by the LLM, the question becomes: what's the next concrete action, or does this warrant a new project?

**Blocked items** do not belong in Next Steps. When a blocked item is identified, ask permission to migrate it to improvements with an appropriate tag. This happens before surfacing any new work.

**"What's next?" protocol** — when the person asks what to do next, or Next Steps is empty or all-blocked, the LLM should:
1. Check Next Steps for blocked items. For each one, ask permission to migrate it to improvements with a suggested tag. Handle all blocked items before proceeding.
2. If unblocked items remain in Next Steps, surface one of those. If Next Steps is now empty, scan improvements (hi/hi first, then lo/hi or hi/lo based on context and vibes, then lo/lo).
3. Pick **one** — 60% vibes, 40% priority order. Present it as a single binary yes/no choice. Not a list.
4. If yes: for an improvement, create a concrete next action citing it via `file.md#anchor`.

**"Prep for exit"** — when the person signals they are wrapping up, the LLM performs an internal audit of everything touched this session and ensures each piece is saved: either accumulated into permanent project files or captured in a session notes file. Nothing touched in the session should exist only in chat history.

---

## Section and File Types

New content lands in `main.md` first. When a section grows large enough to stand alone — i.e., it could be read in isolation and make sense — split it to a named file. The split cycle:

1. Content accretes in a `main.md` section.
2. Section grows coherent and large → extract to a named `.md` file.
3. Replace the section in `main.md` with a one-line reference.
4. Add the file to Project Structure.
5. The new file follows the same principles: references out, first-person singular, no duplication.

Identify the section/file type when creating or splitting:

| Type | Purpose | Examples |
|---|---|---|
| **Log** | Append-only history | `completed.md`, `session-YYYY-MM-DD.md` |
| **Backlog** | Tagged improvement queue; items pull into Next Steps | `improvements.md` or `## Improvements` in main.md |
| **Catalog** | Inventory of domain entities | `pieces.md`, `filaments.md`, `characters/` |
| **Process** | Step-by-step how-to for a recurring workflow | `slicer.md`, `assembly.md` |
| **Reference** | Dense lookup material | `glossary.md`, `tools.md`, `materials.md` — `## Glossary` starts in main.md, splits when grown |
| **Exploration** | Unstructured ideation | `ideas.md` |
| **Research** | Findings from external sources | `topic-notes.md` |
| **Formal** | Authored documents | `policy.md`, `lessons.md` |
| **Behaviors** | Accumulated behavioral corrections | `behaviors.md` |
| **Index** | Content-oriented catalog of all files and sections | starts as `## Index` in main.md; splits to `index.md` via standard accumulation when grown |
| **Sources** | Immutable raw ingested material | `sources/<name>.md` — converted to Markdown at ingest; never edited |

### Backlog (improvements section or file)

The backlog holds all deferred work. It may live as a `## Improvements` section in main.md or as a separate `improvements.md` — whichever fits the project's size. Items are tagged by importance × urgency, with the tag in the section heading:

- **[hi/hi]** — Implement proactively before or during the next active work cycle.
- **[lo/hi]** — Consider for the current or next work cycle.
- **[hi/lo]** — High importance, not urgent. Surface during downtime or lulls.
- **[lo/lo]** — Surface when wistful with no hi/lo items remaining.

Tag definitions and section headers belong in the backlog itself. When to proactively surface items is project-specific — declared in Behaviors.

**Surfacing is a clarification moment.** When the LLM raises an improvement, the question is: what's the next concrete action? That action goes into Next Steps. GTD: identify only the immediate next action, not all future steps upfront. If the improvement is large enough to warrant its own NLP file system, it becomes a new project instead.

### Task Flow

```
capture   →  improvements (tagged — any scope, including badly-scoped or multi-step)
surface   →  LLM raises improvement at the right moment; clarify into a next action or new project
pull      →  main.md ## Next Steps (concrete, unblocked)
finish    →  completed.md (append-only log)
```

---

## Ingest

When the builder brings in an external source, that is an ingest. Workflow:

1. Fetch or read the source.
2. Convert to clean Markdown — strip HTML, add heading structure if plain text, convert PDFs. Markdown is required for anchor support.
3. Save to `sources/<name>` with a standard immutable header:
   ```
   > **Immutable source.** Do not edit. Fetched YYYYMMDD-HHMMSS.
   > Original: <url or provenance>
   ```
4. Discuss and synthesize with the person.
5. Distribute synthesis across all relevant project files — a single ingest may touch Process, Reference, Behaviors, and more simultaneously.
6. Log in `completed.md` with source name, path, and all files touched.

Each file that receives synthesis cites `sources/<name>#<anchor>` to the relevant section, not a raw URL. For oversized sources, store key excerpts. Sources are immutable — named descriptively, never edited after creation.

**File-back on query:** when a question produces a valuable synthesis — a comparison, analysis, or connection — file it into the appropriate project file. It shouldn't disappear into chat history. Explorations compound in project files just like ingested sources do.

---

## Lint / Periodic Audit

Lint is a periodic self-check: orphaned files (not referenced from main.md), stale anchors, contradictions between files, knowledge gaps.

When to run:
- **Session start**: check `completed.md` for the last lint entry; if absent or older than 36 hours, run lint before surfacing any other work
- During prep-for-exit
- When a session touches many files
- After a major accumulation cycle

LLM-initiated: the builder shouldn't need to ask.

---

## Section Headings and Cross-References

### Heading Length

Keep section headings to **3–5 words** (after any tag prefix). Detail goes in the body. Short headings produce clean, stable Markdown anchors — a heading change breaks every link that references it.

- Good: `## [hi/hi] Oil finish rags`
- Avoid: `## [hi/hi] Acquire new rags for use during oil finish application`

**Use headings, not bold text, for any content another file might reference.** Bold text has no anchor. A heading auto-generates one: lowercase, spaces → hyphens, special characters stripped. `### My Section` → `#my-section`. If you need to link to something, make it a heading first.

### Cross-References

Reference specific sections using `file.md#anchor` syntax:

```
Cut rabbet on all stock — see `improvements.md#hihi-frame-rabbet-fit`
```

Anchors are auto-generated from headings: lowercase, spaces → hyphens, special characters stripped. Tag prefix `[hi/hi]` becomes `hihi-` in the anchor.

These links may not be clickable in all contexts but are precise human-readable pointers that survive copy-paste and search.

**Cross-references should be bidirectional where meaningful.** When file A references file B, check immediately whether a back-reference belongs in file B. Behavioral rule: when writing any reference, stop and check before moving on.

---

## Behaviors

LLM behavioral instructions live in `main.md` under `## Behaviors`, or in `behaviors.md` once split. These tell the LLM how to operate in this specific project — role, startup ritual, proactive actions, conventions, and any corrections learned across sessions. Start inline in main.md; split to `behaviors.md` using the same split-when-coherent rule.

**All learned behaviors go here explicitly.** When a correction is made in a session, write it into Behaviors immediately. Nothing behavioral should accumulate silently in hidden folders.

Common behavior types:
- **Session startup** — what to read at session start
- **Role vocabulary** — how to refer to the person using this project
- **Proactive surfacing** — what to surface without being asked, and when
- **Accumulation triggers** — when to append to log/catalog/backlog files
- **Tool usage** — which tools to use for which actions
- **Archive behavior** — when session notes move to `archive/`
- **Ingest log** — every ingest appended to `completed.md` with source name, path, and all files touched
- **Lint trigger** — LLM runs lint at session start if last lint is older than 36 hours; also at prep-for-exit, after sessions touching many files, or after a major accumulation cycle
- **Cross-reference** — when writing a reference to another file, immediately check if a back-reference belongs in the target file

---

## Role Vocabulary

Each project names who the human is, declared in Behaviors. If unspecified at project creation, ask: *"How should I refer to you in this project? (artist, player, educator, user, ...)"*

---

## Session Notes

Written to `session-YYYY-MM-DD.md` in the project root during the session. After content is integrated into permanent files, moved to `archive/`. Not read unless explicitly requested.

---

## No Stubs

Do not create a file until there is content to put in it.

---

## Persistence Layers

| Layer | Location | What belongs here |
|---|---|---|
| **Project files** | `<project>/*.md` | Everything — domain knowledge, behaviors, tasks, logs, learned corrections |
| **Immutable sources** | `<project>/sources/` | Raw ingested material — converted to Markdown at ingest; never edited after creation |
| **Global LLM config** | `~/.claude/CLAUDE.md`, settings | Behavior applying to all projects globally |
| **Project memory folder** | `~/.claude/projects/<path>/memory/` | Should remain empty. If something accumulates here, migrate it to the project files. |

The project memory folder exists as a fallback but should not be used in a well-structured NLP file system — everything has an explicit home in the project files. Many things that might seem like "memory" — learned corrections, role vocabulary, behavioral conventions — belong in `## Behaviors` or `behaviors.md`.

---

## Scaffolding a New Project

When told "I want a [type] project":

1. Ask for the project name.
2. Ask how to refer to the person, if not obvious from context.
3. Create `<name>/` directory.
4. Copy `template.md` to `<name>/main.md`. Fill in the project name, goal, and role. Note: `template.md` opens with `@_filesys.md` — this guarantees `_filesys.md` is in context via harness injection, replacing the unreliable "At session start, read" behavioral instruction.
5. Run `ln -s main.md <name>/CLAUDE.md` (backwards compatibility with Claude Code auto-load).
6. Copy `_filesys.md` into `<name>/_filesys.md`. This file is the project's ongoing self-maintenance guide — accumulation, splitting, surfacing, file types, task flow. It does not cover initialization; that is the factory's job. Note: `_filesys.md` duplicates the glossary and some content from `desirements.md` by design — it is a distribution artifact that must be self-contained, not a project file subject to the non-duplication principle. It is factory-managed: the LLM operating within a child project must not edit it. Updates are pushed deliberately from the factory.
7. Run `git init <name>/`. Stage the scaffolded files (`main.md`, `_filesys.md`, `CLAUDE.md`), propose the commit message `"init commit"`, and wait for explicit approval before committing. Uses local git identity — no per-repo setup needed.
8. Create additional files only if there is immediate content for them. No stubs.

The project then builds itself out through the accumulation cycle.

---

## Desirements Summary

1. **Filesystem is the memory.** All durable state lives in `.md` files — including learned behaviors.
2. **Single entry point.** `main.md` is the actual project brain. `CLAUDE.md` is a symlink for Claude Code compatibility.
3. **Session startup ritual.** Startup files are injected via `@filename` harness expansion — not behavioral reads. The LLM sees them because the harness loads them at session start, not because it decided to read them.
4. **Active list is small.** 3–7 WIP-limited next actions in `main.md ## Next Steps`. Never a separate file. New items always captured to improvements, not lost.
5. **Backlog is tagged.** Improvements section or file holds deferred work with hi/lo importance × urgency tags. Items pull into Next Steps when space opens.
6. **Start monolithic, split when coherent.** New content goes into `main.md` first. Extract when a section can stand alone, or when another file needs to cite it.
7. **Know the file type.** Use the taxonomy to name and structure new files correctly.
8. **Short headings, precise references.** 3–5 word section headings. Cross-reference with `file.md#anchor` syntax. Use headings, not bold text, for referenceable content — only headings generate anchors.
9. **Reference over duplication.** Files point to each other; no content repeated.
10. **Role clarity.** Each project names who the human is. Ask if unspecified.
11. **Self-maintaining.** `main.md` is updated at natural milestones. The project documents itself.
12. **Sources are immutable.** Ingested material lives in `sources/` as clean Markdown, frozen at creation. Synthesized files cite `sources/<name>#<anchor>`, not raw URLs.
13. **Ingest is an operation.** When a source enters the project, convert to Markdown, save to `sources/`, synthesize across all relevant files, log in `completed.md`.
14. **File back.** Valuable query syntheses land in project files, not just chat. Explorations compound.
15. **Lint periodically.** LLM checks for orphans, stale refs, and contradictions at session start (if >36h since last), at prep-for-exit, after heavy sessions, or after a major accumulation cycle.

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
- [The GTD Approach to Linking Next Actions and Projects](https://gettingthingsdone.com/2020/06/the-gtd-approach-to-linking-next-actions-and-projects/) — only identify the immediate next action; don't decompose all steps upfront
- [Projects vs. Next Actions – Ask MetaFilter](https://ask.metafilter.com/217774/Projects-vs-Next-Actions) — badly-scoped items belong in project support (backlog), not the next actions list
- [LLM Wiki — Andrej Karpathy](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) — persistent wiki pattern: immutable sources → LLM-maintained wiki → schema; ingest/query/lint operations; index.md and log.md; why maintenance cost near-zero enables compounding knowledge bases
- `research-pkm-llm.md` — synthesized research on PKM science, LLM failure modes, and Claude-specific patterns; the primary evidence base for this factory's design decisions
