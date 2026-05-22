# NLP File System — Scaffold Factory

A pattern for using a directory of Markdown files as persistent LLM memory, and a factory that scaffolds new projects following the pattern on demand.

An **NLP file system** is just that: a project directory where Markdown files function as the LLM's operating context across sessions. All durable state lives on disk, in human-readable files, version-controlled. The LLM navigates, reads, writes, and maintains them — no hidden memory layer, nothing tied to a single tool's session store.

This repo is the **scaffold factory**: a root project that generates new NLP-file-system projects from templates and pushes operational updates downstream.

---

## Features

- **Filesystem-first memory.** All state lives in `.md` files. No hidden folders, no chat-only knowledge.
- **`main.md` as project brain.** Single entry point per project; references outward, never referenced back.
- **`CLAUDE.md` auto-load.** Symlinked to `main.md` for Claude Code compatibility; other harnesses can target `main.md` directly.
- **Scaffold factory.** One command-style request ("new project: X") creates a project directory, templates `main.md`, copies `_filesys.md`, sets up `.gitignore` and `.claude/settings.json`, and inits the repo.
- **Two templates.** `template.md` (active project) and `template-reference.md` (reference / lookup content).
- **Version-controlled from init.** Git init on scaffold; every completed task → proposed commit message + file list; explicit approval required before commit or push.
- **Tagged improvements backlog.** Deferred work tagged by importance × urgency (`[hi/hi]`, `[lo/hi]`, `[hi/lo]`, `[lo/lo]`); items pull into Next Steps when space opens.
- **GTD-style Next Steps.** 3–7 concrete, unblocked actions per project. Personal Kanban WIP limit. Badly-scoped items get routed to improvements instead.
- **Lifecycle states.** Every project declares `active`, `reference`, `dormant`, or `archived` in its `## Status`. State determines maintenance apparatus.
- **Periodic lint.** LLM-initiated audit for orphan files, stale cross-references, and contradictions. Runs at session start (if stale), prep-for-exit, and after heavy accumulation.
- **Cold-start protocol.** When a project has been idle > 7 days, the LLM reads the full Index, verifies it against disk, surfaces drift, and offers a single re-orientation summary before doing any work.
- **Immutable ingest.** External sources land in `sources/` as clean Markdown with a frozen header; receiving files cite `sources/<name>#<anchor>` rather than raw URLs.
- **Cross-project references.** Content that belongs to a sibling project gets moved (not duplicated); pointers use `../B/file.md`.
- **Factory-managed `_filesys.md`.** The operational-principles file is hash-stamped and distributed deliberately to child projects — children don't edit it, the factory pushes updates downstream.
- **Parent-context isolation.** Scaffolded projects ship with a `.claude/settings.json` that excludes the factory's `CLAUDE.md`, so a child session loads only its own project context — the factory's brain doesn't leak in.
- **Caveman mode on by default.** New projects ship with a terse-output behavior (roughly 65% token reduction); removable per-project by deleting one line.

---

## How it works

Each project is a directory containing:

```
<project>/
├── main.md            # project brain (under ~200 lines, curated)
├── CLAUDE.md          # symlink to main.md
├── _filesys.md        # factory-managed operating principles
├── .gitignore         # subdirectories ignored by default; sources/ allowed
├── .claude/           # harness settings (gitignored)
├── sources/           # immutable ingested material (committed)
└── [domain files].md  # extracted as main.md sections grow
```

`main.md` holds the standard sections — `Goal`, `Index`, `Behaviors`, `Next Steps`, plus domain sections — and grows by accumulation. When a section becomes coherent and large enough to stand alone (or another file needs to cite it), it splits into its own named file and `main.md` keeps a one-line reference.

The full design rationale lives in [`desirements.md`](./desirements.md). The exact operational rules every project follows live in [`_filesys.md`](./_filesys.md).

---

## Using the factory

From within this directory, in a session that has loaded `main.md`, tell the LLM something like:

> new project: a place to track my woodworking pieces

The factory will ask any missing questions (project name, role vocabulary, active vs. reference) and run the scaffolding protocol. Output: a new subdirectory inside the factory, git-initialized, ready to use.

The full protocol is in [`main.md`](./main.md) under *Scaffolding Protocol*.

---

## Background

The pattern is synthesized from several adjacent ideas:

- **GTD** — concrete next actions, badly-scoped items routed to a backlog
- **Personal Kanban** — WIP-limited active list, pull when ready
- **Project Brain / Context-as-Code** — externalized, version-controlled LLM context
- **LLM Wiki (Karpathy)** — immutable sources, LLM-maintained synthesis, near-zero maintenance cost
- **Semantic File System (LSFS)** — topically coherent files improve NL retrieval

See [`desirements.md`](./desirements.md) for the full literature trail and rationale, and [`research-pkm-llm.md`](./research-pkm-llm.md) for the synthesized research notes on personal knowledge management and LLM failure modes that informed the design.

---

## License

[AGPL-3.0-or-later](./LICENSE). Forks and derivatives — including modifications served over a network — must remain under the same license.
