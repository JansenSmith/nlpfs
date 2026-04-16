# NLP File System — Operational Principles

Guides project self-maintenance: accumulation, splitting, surfacing, task flow. Self-contained distribution artifact — duplicates glossary and key concepts from factory by design. Design rationale: `desirements.md` in scaffold factory (`~/Documents/projects/`).

**Factory-managed. Do not edit within a project.** Changes flow from factory, pushed downstream deliberately.

**@-import limitation:** loaded at session start; no mid-session refresh. Edit during session = LLM sees old version until next session. Don't edit and expect immediate effect.

---

## Glossary

| Term | Definition |
|---|---|
| **NLP file system** | Directory of Markdown files functioning as persistent LLM context and memory |
| **Project** | Subfolder with its own NLP file system, created by scaffold factory. Self-contained domain. |
| **Scaffold factory** | Root project that creates new projects on demand (`~/Documents/projects/`) |
| **Clearinghouse** | `main.md`; single entry point — references outward, never referenced back to |
| **Next action** | Concrete, unblocked, doable step. Lives in `main.md ## Next Steps`. |
| **Improvement** | Deferred work item — not yet ready or needs clarification. Belongs in `## Improvements` or `improvements.md`. |
| **Accumulate** | Adding content to existing file/section rather than creating new; default before split is warranted |
| **Split** | Extracting grown section from main.md into its own named file |
| **Surfacing** | LLM proactively raising improvement at right moment; clarify into next action or new project |
| **Prep for exit** | End-of-session audit: ensure everything touched is saved to permanent files or session notes |
| **Ingest** | Bringing external source in: convert to Markdown, save to `sources/<name>`, synthesize, log |
| **Lint** | Periodic LLM-initiated audit: orphaned files, stale refs, contradictions, knowledge gaps |
| **sources/** | Immutable raw ingested material — converted to Markdown at ingest; never edited after |

---

## main.md Rules

- Always present. Always entry point. Auto-loaded every session.
- References outward. No other file references back.
- Target: under ~200 lines. Grows past → split.
- First-person singular ("my", "I").
- No hidden folders — everything is a `.md` file in project directory or subdirectories.
- New knowledge lands here first. Extract when section stands alone.
- Another file needs to cite content in main.md → extract first; citation need = split trigger.

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
- **Next actions only** — concrete, unblocked, doable now (GTD). Can't write as physical action → improvement.
- Right scope: "add entry to pieces.md" — wrong scope: "improve the pipeline"
- Full → new items to improvements. Ceiling = finish or capture to backlog, not stop recording.
- Completed → completed.md
- **Next Steps empty mid-session** → run What's Next? Protocol without being asked.

**After any action**, prompt for next — 60% vibes, 40% priority.

**Blocked items** don't belong in Next Steps. When identified, ask permission to migrate to improvements with suggested tag. Handle all blocked before surfacing new work.

### What's Next? Protocol

When asked what to do next, or Next Steps empty/all-blocked:

1. Check Next Steps for blocked items. Ask permission to migrate each to improvements with suggested tag. Handle all blocked first.
2. Unblocked items remain → surface one. Empty → scan improvements (hi/hi first, then lo/hi or hi/lo on context and vibes, then lo/lo).
3. Pick **one** — 60% vibes, 40% priority. Single binary yes/no. Not a list.
4. Yes → propose concrete next action citing via `file.md#anchor`, ask to confirm adding to Next Steps. Confirmed → add, ask if they'd like to execute now.

---

## Section and File Types

On new section or split, identify type:

| Type | Purpose | Examples |
|---|---|---|
| **Log** | Append-only history | `completed.md`, `session-YYYY-MM-DD.md` |
| **Backlog** | Tagged improvement queue; items pull into Next Steps | `## Improvements` in main.md or `improvements.md` |
| **Catalog** | Inventory of domain entities | `pieces.md`, `filaments.md`, `characters/` |
| **Process** | Step-by-step how-to for recurring workflow | `slicer.md`, `assembly.md` |
| **Reference** | Dense lookup material | `glossary.md`, `tools.md` |
| **Exploration** | Unstructured ideation | `ideas.md` |
| **Research** | Findings from external sources | `topic-notes.md` |
| **Formal** | Authored documents | `policy.md`, `lessons.md` |
| **Behaviors** | Accumulated behavioral corrections | `behaviors.md` |
| **Index** | Catalog of all files and sections | starts as `## Index` in main.md; splits to `index.md` when grown |
| **Sources** | Immutable raw ingested material | `sources/<name>.md` — converted to Markdown at ingest; never edited |

`## Glossary` starts in main.md when project develops vocabulary; splits to `glossary.md` when grown. Not a default section — add when needed.

---

## Backlog

Holds all deferred work — any scope, including badly-scoped or multi-step. Lives as `## Improvements` in main.md or as `improvements.md`. Tagged by importance × urgency:

- **[hi/hi]** — Implement proactively before/during next active work cycle.
- **[lo/hi]** — Consider for current or next work cycle.
- **[hi/lo]** — High importance, not urgent. Surface during downtime or lulls.
- **[lo/lo]** — Surface when wistful with no hi/lo items remaining.

Tag definitions and section headers belong in the backlog itself. When to surface is project-specific — declare in Behaviors.

**Surfacing = clarification moment.** LLM raises improvement → what's the next concrete action? Goes to Next Steps. Large enough for own NLP file system → new project.

### Task Flow
```
capture   →  improvements (any scope)
surface   →  LLM raises at right moment; clarify into next action or new project
pull      →  main.md ## Next Steps (concrete, unblocked)
finish    →  completed.md
```

---

## Section Headings and Cross-References

**3–5 words** after any tag prefix. Detail in body. Short headings = stable anchors — title change breaks every link.

- Good: `## [hi/hi] Oil finish rags`
- Avoid: `## [hi/hi] Acquire new rags for use during oil finish application`

**Headings, not bold text, for referenceable content.** Bold has no anchor. Heading auto-generates one: lowercase, spaces → hyphens, special chars stripped. `### My Section` → `#my-section`. Need to link → make it a heading first.

**Bidirectional where meaningful.** Writing ref from A to B → immediately check if back-ref belongs in B.

**Cross-reference** with `file.md#anchor` syntax:
```
Cut rabbet on all stock — see `improvements.md#hihi-frame-rabbet-fit`
```

Anchors: lowercase, spaces → hyphens, special chars stripped. `[hi/hi]` → `hihi-` in anchor.

---

## Cross-Project References

Content belongs in sibling project → move it, cite it (`../B/file.md`), commit per repo.

---

## Accumulation Cycle

1. New content lands in main.md.
2. Section grows coherent and large → extract to named file.
3. Replace section in main.md with one-line reference.
4. Add file to Index.
5. New file: references out, first-person singular, no duplication.

**Split criteria:** Can this section be read in isolation and make sense?

---

## Ingest

Fetch → convert to Markdown (heading structure required for anchors) → save to `sources/<name>` with standard immutable header → distribute synthesis across all relevant project files → log in `completed.md` with all files touched.

Receiving files cite `sources/<name>#<anchor>`, not raw URL. File-back: valuable query synthesis → file into project, not chat.

---

## Lint

Periodic check: orphaned files (not referenced from main.md), stale cross-references, contradictions.

When to run:
- **Session start**: check `completed.md` for last lint entry; absent or >36h → run lint before any other work
- During prep-for-exit
- After sessions touching many files
- After major accumulation cycle

LLM-initiated.

---

## Cold-Start Protocol

Triggered when last entry in `completed.md` is older than 7 days. Supersedes normal session-start lint — run this instead.

1. Read all files in Index.
2. Verify Index against actual files on disk — flag orphans or missing entries.
3. Check for drift: stale Next Steps, blocked items not marked blocked, irrelevant improvements.
4. Surface single re-orientation summary: current state, last work done, one recommended next action.
5. Proceed normally.

Goal: "what's next?" answerable in <60 seconds after any gap.

---

## Behaviors

Behavioral instructions live in `main.md ## Behaviors` or `behaviors.md` once split. All corrections go here explicitly — nothing accumulates in hidden folders.

Common types: session startup, role vocabulary, proactive surfacing, accumulation triggers, tool usage, archive behavior, ingest log, lint trigger, cross-reference.

Universal behaviors (every project, via this file):
- Writing ref to another file → immediately check if back-ref belongs in target.
- Session start → check `completed.md` last entry date; >7 days → Cold-Start Protocol. Otherwise check last lint; absent or >36h → run lint before any work.
- Prep-for-exit, heavy sessions, major accumulation → run lint: orphaned files, stale refs, contradictions.
- Every ingest → append to `completed.md` with source name, path, all files touched.
- Asked what to do next → run What's Next? Protocol.
- Next Steps changed → show current queue, ask if anything strikes them or they'd like a suggestion.
- Person signals something needed or missing → infer blockage, ask about capturing as improvement or dependency before moving on.
- Next Steps item completed → propose git commit message and file list; wait for explicit approval before committing.

Split to `behaviors.md` using same split-when-coherent rule.

---

## Session Notes

- Written to `session-YYYY-MM-DD.md` in project root.
- After integration into permanent files, move to `archive/`. `archive/` is a vessel graveyard — synthesis must be complete before archiving. Not a deferral mechanism.
- Not read unless explicitly requested.

---

## Prep for Exit

Audit everything touched — save to permanent files or session notes. Nothing touched lives only in chat history. Run lint: orphaned files, stale refs, contradictions from this session.

---

## Git

Version-controlled from init. Each completed task gets a commit.

**Commit on task completion.** Item moves to `completed.md` → propose commit message + file list to stage. Wait for explicit approval. Never commit without approval. Same for `git push`.

**Style:** short, lowercase, past tense, no trailing period. Init message: `"init commit"`. Single line unless work warrants body.
- Good: `added entry to pieces.md`
- Avoid: `Added entry to pieces.md.`

**Never:** `--no-verify`, `--force`, amend published commits — unless explicitly asked.

---

## No Stubs

No file until content exists.

## No Memory Folders

Never write to `~/.claude/projects/*/memory/`. All state in project files. Corrections, conventions, vocabulary → `## Behaviors` or `behaviors.md`.
