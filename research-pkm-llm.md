# Research: PKM Science and LLM Patterns

Synthesized findings relevant to the NLP file system factory. Generated 2026-04-13.

---

## PKM Science: What Works and What Fails

### Framework summary

| Framework | Strength | Failure mode |
|---|---|---|
| GTD | Total capture; clears working memory | Weekly review collapses under pressure; system dies with it |
| Zettelkasten | Genuine idea development via linked atomic notes | Taxonomic overhead; devolves into bookmark pile without discipline |
| PARA | Frictionless triage by actionability | Constant re-sorting as states change; users find it irritating |
| CODE (Forte) | Addresses saved-but-never-used problem | Distill/Express stages require focused time most people don't have |

Practitioner consensus (2025): combining PARA for organization with Zettelkasten-style linking for developed notes outperforms any single system.

### Abandonment data

- Forte Labs survey: 68% of PKM tool adopters abandoned within 6 months — the tool wasn't the failure point; the missing piece was a framework for deciding what to capture, organize, and retrieve.
- Research at scale (8,000 notes, 64,000 links): **67% of saved notes are never revisited.** The problem is not storage — it's retrieval and use.

### Ranked failure modes

1. Perfectionism / over-engineering — configuring becomes the hobby
2. Capture without retrieval — archives grow, utility doesn't
3. Review ritual collapse — first dropped under pressure; system dies after
4. Tool hopping — migration consumes the time that would build the knowledge base
5. Complexity mismatch — copying someone else's mature system wholesale

### Cognitive offloading: the uncomfortable findings

- Offloading demonstrably improves task accuracy, especially for high-memory-load tasks (confirmed 2025 meta-analysis).
- **Saving reliably reduces memory encoding.** When people expect future access, they have lower recall of the information and only enhanced recall of *where* to find it (Sparrow et al., 2011 Google Effect; confirmed in meta-analyses).
- People remember *deleted* information better than *saved* information — deletion triggers encoding; saving triggers offloading.
- **Practical implication**: systems that make it too easy to save anything degrade the user's internal knowledge while filling the archive with material they'll never retrieve. Capture-first with weak retrieval is counterproductive.
- 2025 research (Frontiers in Psychology): reliance on external tools correlates with **overconfidence** — users trust the system's completeness even when it has gaps.

### Design principles that make systems survive

1. **Minimal capture friction** — reachable in seconds; mode-switching kills capture
2. **Low review overhead** — completable on the user's worst-energy day; if not, it won't happen
3. **Use-driven, not storage-driven** — "will I retrieve this?" not "should I capture this?"
4. **Progressive complexity** — start with 1-2 habits; add complexity only after 30+ days of stability
5. **Output as the test** — if the system never produces output (decisions, action, writing), it's filing, not thinking

---

## Resilient System Design

### The core problem

Most productivity systems have no graceful degradation path. They require continuous maintenance; gaps cause collapse. The review ritual is load-bearing but is also the first thing dropped.

### Properties of systems that survive gaps

1. **State recoverable from artifact alone** — if understanding current state requires memory of past sessions, the system fails after any gap. Files must be self-describing.
2. **Re-entry cost near-zero** — "what's next?" answerable in < 60 seconds. Systems requiring reading 2,000 words before action are abandoned after two-week absences.
3. **Backlog bounded and prioritized** — unbounded accumulation creates re-entry dread. An explicit inbox-zero mechanism (GTD's processing, PARA's archive) helps systems survive interruptions.
4. **Minimum viable ritual** — design mandatory overhead for the user's worst day. Everything else is optional.
5. **Decay as a feature** — information that isn't retrieved or linked should age out rather than accumulate forever. Unbounded growth → abandonment. (No academic consensus, but strong practitioner evidence.)

### Habit cadence

Lally et al.: 18–254 days depending on complexity. **Systems requiring daily engagement are fragile. Weekly survives better than daily; monthly survives better than weekly.** Design the review for the minimum sustainable cadence, not the aspirational one.

---

## LLM Context Management (Early 2026)

### What has stabilized

- **Context as a resource, not a log** — treat like working memory; prune stale content
- **60-70% rule** — don't let context exceed 60-70% capacity; auto-compaction fires at ~83.5% and retains only ~20-30% of specific details (file paths, error codes, decisions)
- **Fresh sessions per unrelated task** — long-running context accumulates noise faster than signal
- **Behavioral rules beat prose** — explicit directives ("never do X") survive context better than explanatory prose; reasoning decays, rules persist

### LLM as executive function prosthetic: what works

- Reduces task initiation overhead
- Effective for narrow, structured tasks: reminders, status surfacing, checklist maintenance
- **Does not work**: multi-step planning, goal coordination across sessions without explicit files, longitudinal analysis requiring connecting events across time

### Known failure modes (ranked by severity)

1. **Context poisoning** — an incorrect belief written to a file gets read back on every subsequent session and reinforced; the model fixates on false state. **Most dangerous for persistent assistants.**
2. **Goal drift** — over long sessions, the agent optimizes for a subtly reframed version of the original goal; especially risky when the LLM edits its own instructions
3. **Execution instability** — models degrade mid-task: malformed calls, loss of structure, forgotten earlier decisions; performance degrades *within* a session
4. **Recency bias** — when handling long inputs, models over-rely on the most recent tokens; instructions near the top of a long CLAUDE.md get deprioritized as the session grows
5. **Over-reliance / flow disruption** — gains are real but dependency disrupts flow when the assistant is wrong or unavailable

---

## Claude-Specific Findings

### CLAUDE.md: what the community has learned

- Treat it like `.gitignore` — essential, not optional
- **Keep root CLAUDE.md to 50-100 lines.** Past that, rules buried in the middle are progressively deprioritized
- Use `@imports` for detailed sections; load on demand, not always
- Behavioral rules beat prose — explicit directives survive better than explanatory text
- Subdirectory CLAUDE.md overrides project-level; enables per-directory context without root bloat

### Context window hard facts

- 200K shared window consumed by messages, replies, file reads, and tool outputs simultaneously
- **Auto-compaction fires at ~83.5%** — summary retains ~20-30% of specific details
- No persistent memory across compaction — CLAUDE.md survives because it's re-read from disk; in-session state does not
- **Quality degrades before the hard limit** — inconsistency appears well before the context fills
- Any information that must survive a session must be explicitly written to a file before compaction

### Known limitations

- Stateless between sessions by default
- @-imports load at session start; **do not refresh mid-session** if the imported file changes
- No cross-session goal tracking without external files
- Long CLAUDE.md files self-defeat — the system meant to provide stability becomes noise

---

## Sources

- [Personal Knowledge Management at Scale — dsebastien](https://www.dsebastien.net/personal-knowledge-management-at-scale-analyzing-8-000-notes-and-64-000-links/)
- [Minimum Viable PKM — Devesh Uba](https://deveshuba.com/minimum-viable-pkm/)
- [Decay vs Permanence — Medium](https://medium.com/@ann_p/decay-vs-permanence-should-pkms-forget-to-stay-useful-5069da096023)
- [Google Effects on Memory — Sparrow et al. (Science, 2011)](https://www.science.org/doi/10.1126/science.1207745)
- [Cognitive Offloading or Cognitive Overload? — Frontiers in Psychology (2025)](https://www.frontiersin.org/journals/psychology/articles/10.3389/fpsyg.2025.1699320/full)
- [Consequences of Cognitive Offloading — PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC8358584/)
- [The LLM Context Problem in 2026 — LogRocket](https://blog.logrocket.com/llm-context-problem-strategies-2026/)
- [LLM-based Agents Suffer from Hallucinations — arXiv (2025)](https://arxiv.org/html/2509.18970v1)
- [Agentic AI Threats: Memory Poisoning — Lakera](https://www.lakera.ai/blog/agentic-ai-threats-p1)
- [Claude Code Context Window Guide — Morph](https://www.morphllm.com/claude-code-context-window)
- [CLAUDE.md Best Practices — DEV Community](https://dev.to/cleverhoods/claudemd-best-practices-from-basic-to-adaptive-9lm)
- [Claude Code Best Practices — Anthropic](https://code.claude.com/docs/en/best-practices)
- `sources/karpathy-llm-wiki.md` — Karpathy's persistent wiki pattern; ingest/query/lint operations, compounding knowledge base design
