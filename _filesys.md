# NLP File System — Operational Principles

This file guides ongoing project self-maintenance: accumulation, splitting, surfacing, and task flow. It is a self-contained distribution artifact — duplicating glossary and key concepts from the scaffold factory by design. For full design rationale and literature sources, see `desirements.md` in the scaffold factory (`~/Documents/projects/`).

**This file is factory-managed. Do not edit it within a project.** Changes flow from the scaffold factory and are pushed downstream deliberately.

**@-import limitation:** this file (and any other `@`-imported file) is loaded by the harness at session start and does not refresh mid-session. If it is edited during a session, the LLM still sees the version from session start until the next session begins. Do not edit an `@`-imported file and expect the change to take effect immediately.

---

## Glossary

| Term | Definition |
|---|---|
| **NLP file system** | A directory of Markdown files functioning as persistent LLM context and memory |
| **Project** | A subfolder with its own NLP file system, created by the scaffold factory. A self-contained domain. |
| **Scaffold factory** | The root project that creates new projects on demand (`~/Documents/projects/`) |
| **Clearinghouse** | `main.md`; the single entry point for a project — references outward to all other files, never referenced back to |
| **Next action** | A concrete, unblocked, immediately doable step. Lives in `main.md ## Next Steps`. |
| **Improvement** | Any deferred work item — a single action not yet ready, or a larger goal needing clarification. Belongs in `## Improvements` or `improvements.md`. |
| **Accumulate** | Adding new content to an existing file or section rather than creating a new file; the default behavior before a split is warranted |
| **Split** | Extracting a grown section from main.md into its own named file |
| **Surfacing** | The LLM proactively raising an improvement at the right moment; the opportunity to clarify it into a next action or new project |
| **Prep for exit** | End-of-session internal audit: review things touched this session and ensure each is saved — either accumulated into permanent files or captured in session notes |
| **Ingest** | The operation of bringing an external source into the project: convert to Markdown, save to `sources/<name>`, synthesize across project files, log in `completed.md` |
| **Lint** | A periodic LLM-initiated self-audit: orphaned files, stale cross-references, contradictions, knowledge gaps |
| **sources/** | Directory of immutable raw ingested material — converted to Markdown at ingest; never edited after creation |

---

## main.md Rules

- Always present. Always the entry point. Auto-loaded every session.
- References outward. No other file references back to it.
- Target: under ~200 lines. When it grows past this, split.
- First-person singular ("my", "I").
- No hidden folders for project notes — everything is a `.md` file in the project directory or subdirectories.
- New knowledge lands here first. Extract to a named file when a section has grown enough to stand alone.
- If another file needs to cite content in main.md, extract that content first — the citation need is a split trigger.

### Standard Sections
```
# Project Name
## Goal
## [Domain sections — extracted to files as they grow]
## Index               ← catalog of all files; always includes main.md and CLAUDE.md symlink; splits to index.md when grown
## Behaviors           ← LLM behavioral instructions (see below)
## Next Steps          ← active task list (always here, never splits)
```

---

## Next Steps Rules

- **3–7 items max** (Personal Kanban WIP limit)
- **Next actions only** — concrete, unblocked, doable right now (GTD). If an item can't be written as a specific physical action, it's an improvement — capture it there instead.
- Right scope: "add entry to pieces.md" — wrong scope: "improve the pipeline"
- When full, new items go to improvements. Hitting the ceiling means finish something or capture to backlog — not stop recording.
- Completed items → completed.md
- **When Next Steps becomes empty mid-session**, run the What's Next? Protocol without being asked.

**After completing any action**, prompt for the next one — 60% vibes, 40% priority.

**Blocked items** do not belong in Next Steps. When identified, ask permission to migrate to improvements with a suggested tag. Handle all blocked items before surfacing new work.

### "What's Next?" Protocol

When asked what to do next, or when Next Steps is empty or all-blocked:

1. Check Next Steps for blocked items. For each one, ask permission to migrate it to improvements with a suggested tag. Handle all blocked items before proceeding.
2. If unblocked items remain in Next Steps, surface one of those. If Next Steps is now empty, scan improvements (hi/hi first, then lo/hi or hi/lo based on context and vibes, then lo/lo).
3. Pick **one** — 60% vibes, 40% priority order. Present it as a single binary yes/no choice. Not a list.
4. If yes: propose a concrete next action citing it via `file.md#anchor` and ask to confirm adding it to Next Steps. On confirmation, add it, then ask if they'd like to execute it now.

---

## Section and File Types

When creating a new section or splitting one off, identify the type:

| Type | Purpose | Examples |
|---|---|---|
| **Log** | Append-only history | `completed.md`, `session-YYYY-MM-DD.md` |
| **Backlog** | Tagged improvement queue; items pull into Next Steps | `## Improvements` in main.md or `improvements.md` |
| **Catalog** | Inventory of domain entities | `pieces.md`, `filaments.md`, `characters/` |
| **Process** | Step-by-step how-to for a recurring workflow | `slicer.md`, `assembly.md` |
| **Reference** | Dense lookup material | `glossary.md`, `tools.md` |
| **Exploration** | Unstructured ideation | `ideas.md` |
| **Research** | Findings from external sources | `topic-notes.md` |
| **Formal** | Authored documents | `policy.md`, `lessons.md` |
| **Behaviors** | Accumulated behavioral corrections | `behaviors.md` |
| **Index** | Content-oriented catalog of all files and sections | starts as `## Index` in main.md; splits to `index.md` via standard accumulation when grown |
| **Sources** | Immutable raw ingested material | `sources/<name>.md` — converted to Markdown at ingest; never edited |

Note: `## Glossary` is a common Reference section that starts in main.md when the project develops vocabulary, and splits to `glossary.md` when grown. Not a standard starting section — add when needed.

---

## Backlog (improvements section or file)

Holds all deferred work — any scope, including badly-scoped or multi-step items. May live as `## Improvements` in main.md or as `improvements.md`. Items tagged by importance × urgency, tag in the heading:

- **[hi/hi]** — Implement proactively before or during the next active work cycle.
- **[lo/hi]** — Consider for the current or next work cycle.
- **[hi/lo]** — High importance, not urgent. Surface during downtime or lulls.
- **[lo/lo]** — Surface when wistful with no hi/lo items remaining.

Tag definitions and section headers belong in the backlog itself. When to proactively surface items is project-specific — declare in Behaviors.

**Surfacing is a clarification moment.** When the LLM raises an improvement, the question is: what's the next concrete action? That goes into Next Steps. If the improvement is large enough to warrant its own NLP file system, it becomes a new project.

### Task Flow
```
capture   →  improvements (any scope)
surface   →  LLM raises at the right moment; clarify into a next action or new project
pull      →  main.md ## Next Steps (concrete, unblocked)
finish    →  completed.md
```

---

## Section Headings and Cross-References

**Keep headings to 3–5 words** after any tag prefix. Detail goes in the body. Short headings produce stable anchors — a title change breaks every link to it.

- Good: `## [hi/hi] Oil finish rags`
- Avoid: `## [hi/hi] Acquire new rags for use during oil finish application`

**Use headings, not bold text, for any content another file might reference.** Bold text has no anchor. A heading auto-generates one: lowercase, spaces → hyphens, special characters stripped. `### My Section` → `#my-section`. If you need to link to something, make it a heading first.

**Cross-references should be bidirectional where meaningful.** When writing a reference from file A to file B, immediately check whether a back-reference belongs in file B.

**Cross-reference** with `file.md#anchor` syntax:
```
Cut rabbet on all stock — see `improvements.md#hihi-frame-rabbet-fit`
```

Anchors are auto-generated: lowercase, spaces → hyphens, special characters stripped. `[hi/hi]` → `hihi-` in the anchor.

---

## Accumulation Cycle

1. New content lands in main.md.
2. Section grows coherent and large → extract to a named file.
3. Replace section in main.md with a one-line reference.
4. Add the file to Project Structure.
5. New file follows same principles: references out, first-person singular, no duplication.

**Split criteria:** Could this section be read in isolation and make sense?

---

## Ingest

Fetch → convert to Markdown (anchor support requires heading structure) → save to `sources/<name>` with standard immutable header → distribute synthesis across all relevant project files (one ingest may touch many) → log in `completed.md` with all files touched.

Each receiving file cites `sources/<name>#<anchor>`, not raw URL. File-back: when a query produces a valuable synthesis, file it into the appropriate project file — don't leave it in chat.

---

## Lint

Periodic check for orphaned files (not referenced from main.md), stale cross-references, and contradictions.

When to run:
- **Session start**: check `completed.md` for last lint entry; if absent or older than 36 hours, run lint before surfacing any other work
- During prep-for-exit
- After sessions touching many files
- After a major accumulation cycle

LLM-initiated.

---

## Cold-Start Protocol

Triggered when the last entry in `completed.md` is older than 7 days. Supersedes the normal session-start lint check — run this instead.

1. Read all files listed in the Index.
2. Verify the Index against actual files on disk — flag orphans or missing entries.
3. Check for drift: stale Next Steps items, blocked items not marked as blocked, improvements that may no longer be relevant.
4. Surface a single re-orientation summary: current project state, last work done, one recommended next action.
5. Proceed normally from there.

Goal: "what's next?" answerable in under 60 seconds after any gap.

---

## Behaviors

LLM behavioral instructions live in `main.md` under `## Behaviors`, or in `behaviors.md` once split. All learned corrections go here explicitly — nothing behavioral accumulates in hidden folders.

Common types: session startup, role vocabulary, proactive surfacing, accumulation triggers, tool usage, archive behavior, ingest log, lint trigger, cross-reference.

Universal behaviors (apply to every project via this file):
- When writing a reference to another file, immediately check if a back-reference belongs in the target file.
- At session start, check `completed.md` for the last entry date; if older than 7 days, run the Cold-Start Protocol. Otherwise check the last lint entry; if absent or older than 36 hours, run lint before surfacing any other work.
- At prep-for-exit, after sessions touching many files, or after a major accumulation cycle, run lint: check for orphaned files, stale references, and contradictions.
- Every ingest appended to `completed.md` with source name, path, and all project files touched.
- When asked what to do next, run the "What's Next?" Protocol.
- After any change to Next Steps, show the current queue and ask if anything strikes them or if they'd like a suggestion.
- When the person signals something is needed or missing, infer a blockage and ask about capturing it as an improvement or dependency before moving on.
- When a Next Steps item is completed, propose a git commit message and wait for explicit approval before committing.

Split to `behaviors.md` using the same split-when-coherent rule.

---

## Session Notes

- Written to `session-YYYY-MM-DD.md` in the project root.
- After integration into permanent files, move to `archive/`.
- Not read unless explicitly requested.

---

## Prep for Exit

When wrapping up a session, audit everything touched and ensure each piece is saved — either accumulated into permanent project files or captured in a session notes file. Nothing touched in the session should exist only in chat history. Also run lint — check for orphaned files, stale references, and contradictions introduced this session.

---

## Git

Projects are version-controlled from initialization. Each completed task gets a commit.

**Commit on task completion.** When a Next Steps item is moved to `completed.md`, propose a commit message covering the work done. Show the proposed message and the list of files to be staged. Wait for explicit approval before running `git commit`. Never commit without approval. Same rule applies to `git push`.

**Commit style:** short, lowercase, past tense, no trailing period. Standard initial commit message is `"init commit"`. Single line unless the work genuinely warrants a body.
- Good: `added entry to pieces.md`
- Avoid: `Added entry to pieces.md.`

**Never use** `--no-verify`, `--force`, or amend published commits unless explicitly asked.

---

## No Stubs

Do not create a file until there is content to put in it.

## No Memory Folders

Never write to `~/.claude/projects/*/memory/`. All state belongs in project files — the project directory is the memory. Many things that might seem like "memory" — learned corrections, behavioral conventions, role vocabulary — belong in `## Behaviors` or `behaviors.md`.
