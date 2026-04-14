# Factory Assessment

Generated 2026-04-13. Research base: `research-pkm-llm.md`, subdir scan of all 20 projects.

---

## Verdict

The core design is sound — filesystem-first, accumulate-then-split, GTD next-action discipline, immutable sources, @-import injection. These are all correct calls validated by research. The factory solves a real problem and solves it better than most alternatives.

But it has structural flaws that will cause abandonment at exactly the moments it's most needed: returning after a gap, crossing a project state boundary, or recovering from an LLM error. These are not edge cases. They are the common case for a person managing 20 heterogeneous projects.

---

## What Works

- **Filesystem-first** — cognitive offloading research confirms external storage reduces working memory load. Correct.
- **Accumulate → split → reference cycle** — matches how knowledge actually develops. Correct.
- **GTD next-action discipline** — WIP limit and concrete action granularity are research-backed. Correct.
- **Immutable sources with anchor citations** — correct epistemic hygiene; citations survive model changes and link rot.
- **@-import over behavioral read instructions** — behavioral instructions are unreliable; harness injection is not. Correct call.
- **No memory folders** — files are more durable, more inspectable, more correctable. Correct.
- **completed.md as lint log** — session-start trigger at 36 hours is a reasonable cadence.
- **Behaviors in files, not hidden state** — correct; hidden state is the failure mode of most persistent assistants.

---

## Critical Issues

### 1. Session overhead violates design

The factory loads `@_filesys.md` (180 lines) + `@desirements.md` (260 lines) = 440+ lines of system overhead at every session start. Research confirms: instructions buried past ~100 lines are progressively deprioritized. The factory's own context discipline rule ("curated ruthlessly, under ~200 lines") is being violated by the factory itself.

`desirements.md` is design rationale — it explains *why* decisions were made. An operational LLM does not need this in every session. It is a reference document, consulted when designing or changing the factory. Loading it universally adds noise without benefit for sessions where no factory-design work is happening.

**Fix**: Remove `@desirements.md` from `main.md`. Add it back as an on-demand read — load when the builder explicitly invokes design discussions or scaffolding decisions. Document this in `## Behaviors`.

---

### 2. No project lifecycle

The scaffolding protocol creates projects. There is no protocol for what happens when a project is done, stalled, or reclassified.

Subdir scan: of 20 projects, approximately 6 are genuinely active tracked projects. The rest are:

| Type | Examples | Count |
|---|---|---|
| Reference / how-to docs | audio, discord, ff7, imagemagick | 4 |
| System documentation | home_assistant, updates | 2 |
| Stalled with no recovery plan | proton, 41dover, vex_sort | 3 |
| No documentation at all | cadoodle, census-data, clauding, floof, research | 5 |
| Early stage / unclear | aaacme, malcom, orca | 3 |
| Active tracked | art, dnd, ai-teaching, resources/pomodoro | 4 |

Only active tracked projects need the full apparatus: Behaviors, Next Steps, Improvements, lint. Reference docs don't need Next Steps. System docs need a change-log pattern, not a project tracker. The 5 undocumented projects are pure cognitive debt — every session that opens the parent directory confronts the builder with ambiguous folders that may represent real work or may be empty.

The factory applies the same template to everything, adding maintenance overhead where there is no return. This is a reliable path to abandonment.

**Fix**: Define project states: **active**, **reference**, **dormant**, **archived**. Add lightweight templates for reference and system doc types. Add a retirement protocol to the scaffolding protocol. Address the 5 undocumented projects — either document or explicitly archive them.

---

### 3. No backlog decay

Research: 67% of notes are never revisited. Systems that grow without decay create re-entry dread and get abandoned. The factory has no concept of information aging.

The `[hi/hi]` tag has no forcing function — there is no age signal. The Karpathy integration improvement sat as `[hi/hi]` through multiple sessions. An item added today and an item added six months ago look identical. There is no mechanism for surfacing "this has been unblocked and hi/hi for 8 weeks and you haven't touched it — is it still real?"

Improvements can accumulate without ceiling. The factory currently has no cap on the backlog, contradicting GTD's principle that an unbounded list creates dread.

**Fix**: Add a creation/surface date to improvement items. Add a ceiling (~10-12 items) to the backlog — when full, triage is required before adding. Items above the threshold age into an explicit "stale" state with a defined review trigger.

---

### 4. No cold-start protocol

The factory has a warm-start protocol (lint check, What's Next, etc.) but no cold-start protocol for returning after a gap. After two weeks away, the LLM arrives with no recent context and follows the same session-start checklist designed for continuity.

A cold-start LLM cannot do the 36-hour lint check meaningfully — it sees the timestamp but has no sense of what the project's stable state looked like. It doesn't know if the Index is accurate, whether stale items are still real, or whether anything has changed. The re-entry experience is undefined.

Research: re-entry cost must be near-zero; "what's next?" must be answerable in < 60 seconds. If returning requires reconstructing state from scratch by reading multiple files, users don't return.

**Fix**: Define a cold-start threshold (e.g., last session > 7 days ago per `completed.md`). When triggered: read all active project files, verify Index against actual files, check for drift, then surface a single re-orientation summary before any work. This is a distinct protocol from warm-start lint.

---

### 5. Context poisoning unaddressed

Research identifies LLM self-editing as the most dangerous failure mode for persistent assistants. The factory depends on the LLM editing project files. An incorrect belief written to a file gets read back into every subsequent session and reinforced. The model becomes confident about false state.

The lint operation is the only defense, and lint is also LLM-initiated. A poisoned LLM linting itself does not reliably detect the poison.

There is no convention for distinguishing builder-written content from LLM-written content. There is no protocol for what happens when the LLM proposes to overwrite something the builder explicitly wrote.

**Fix**: Establish a lightweight convention — use `>` blockquotes for LLM-generated content added to files during a session. Behavioral rule: never silently overwrite builder-written prose; flag proposed changes to existing builder-written sections and require explicit confirmation.

---

### 6. Vibes not a protocol

The most important decision in the system — what to work on next — is the least specified. "60% vibes, 40% priority" is a spirit rule, not a protocol. "Vibes" is undefined.

This makes the LLM's surfacing behavior unpredictable and uncalibrateable. The builder cannot correct it because they cannot observe what rule the LLM is applying. This is a semantic vagueness failure mode — research confirms LLMs parse vague goal language incorrectly and inconsistently.

**Fix**: Define vibes concretely. A working definition: *the improvement most adjacent to the builder's apparent current focus, inferred from the most recent session topic in `completed.md`; or, if no recent focus is clear, the shortest unblocked item regardless of domain.* This is inspectable and correctable.

---

## Internal Consistency Issues

### 1. Desirements startup claim wrong

"The LLM reads declared startup files without being asked" — this is now incorrect. The factory uses @-imports, which are harness injections, not behavioral reads. The point should be updated to reflect the actual mechanism.

### 2. Sources not cross-referenced

`research-pkm-llm.md` and `sources/karpathy-llm-wiki.md` are in the Index but not yet cross-referenced. `research-pkm-llm.md` should cite `sources/karpathy-llm-wiki.md` where relevant; `desirements.md` should reference `research-pkm-llm.md` in Literature Sources.

### 3. completed.md entry structure

Entries are not structured for reliable machine parsing. The 36-hour lint check requires finding the last lint entry; if completed.md grows large, this means scanning the whole file for a lint-type entry among work entries. A consistent type prefix (`## [LINT]` vs `## [WORK]`) would make this reliable and fast.

### 4. desirements↔_filesys sync burden

No protocol for keeping the two files in sync after changes. A desirements.md edit may need to propagate to `_filesys.md` and vice versa — this already happened once this session. No checklist or trigger for this.

### 5. Import limitation undocumented

@-imports do not refresh mid-session. If `_filesys.md` is edited during a session, the LLM still sees the old version until the next session start. Non-obvious; should be documented explicitly — otherwise the builder edits a file expecting immediate effect and is silently wrong.

---

## Ecosystem Assessment (20 Projects)

The builder manages a genuinely diverse portfolio: filament painting art, D&D campaign management, home automation across 3 floors, AI curriculum development, cat behavioral intervention, retro gaming, robotics design, Linux system administration, pomodoro timing infrastructure. The range is wide and the active projects are substantive.

**Critical finding: the pomodoro project has a live bug.** Runaway API calls with no stop mechanism — documented as a known issue in the project itself but absent from the factory's awareness. A robust factory should surface critical bugs in child projects during compliance audits.

**The 5 undocumented projects are the biggest drag.** Unknown purpose + unknown state + no next action = maximum cognitive weight per unit of utility. These should be addressed in the compliance audit before anything else.

**Stall pattern**: projects stall when the next action is unclear or blocked but neither is explicitly captured. `proton`, `41dover`, and `vex_sort` all have "waiting for something" blockers that are implied by the todo structure but not stated. The factory's blockage-detection behavior should catch this but requires the LLM to infer — explicit "BLOCKED: waiting for X" markers in Next Steps would make this reliable.

---

## Priority Fixes

**Before factory v1:**

1. Remove `@desirements.md` from `main.md` — load on demand only; update Behaviors accordingly
2. Define project lifecycle states; add retirement protocol to scaffolding
3. Add cold-start protocol to `_filesys.md`
4. Structure completed.md entries with type prefixes (`[LINT]`, `[WORK]`, `[INGEST]`) for reliable parsing
5. Document @-import limitation in `_filesys.md`

**High importance, not urgent:**

6. Add backlog ceiling (~12 items) with triage trigger
7. Define "vibes" concretely in What's Next Protocol
8. Add lightweight templates for reference and system doc project types
9. Establish convention for LLM-written vs builder-written content

**Eventually:**

10. Backlog aging mechanism (creation date on improvement items)
11. Source freshness convention (when does an immutable source need re-checking?)
12. desirements↔_filesys.md sync protocol
13. Address the 5 undocumented projects
14. Surface pomodoro bug in compliance audit

---

## Sources

- `research-pkm-llm.md` — synthesized research on PKM science, LLM failure modes, Claude-specific patterns
- Subdir scan: all 20 project directories read and assessed 2026-04-13
