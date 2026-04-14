> **Immutable source.** Do not edit. Fetched 20260414-124036.
> Original: https://github.com/JuliusBrussee/caveman

# Caveman — Source

## What It Is

A Claude Code skill/plugin that makes the LLM talk like a caveman — cutting ~65–75% of output tokens while keeping full technical accuracy. Based on the observation that terse output is faster, easier to read, and (per a March 2026 paper) sometimes *more accurate* than verbose output.

> ["Brevity Constraints Reverse Performance Hierarchies in Language Models"](https://arxiv.org/abs/2604.00025) — constraining large models to brief responses improved accuracy by 26 percentage points on certain benchmarks and completely reversed performance hierarchies.

## Before / After

| Normal Claude (69 tokens) | Caveman Claude (19 tokens) |
|---|---|
| "The reason your React component is re-rendering is likely because you're creating a new object reference on each render cycle. When you pass an inline object as a prop, React's shallow comparison sees it as a different object every time, which triggers a re-render. I'd recommend using useMemo to memoize the object." | "New object ref each render. Inline object prop = new ref = re-render. Wrap in `useMemo`." |

Same fix. 75% less word.

## Intensity Levels

| Level | Trigger | Effect |
|---|---|---|
| **Lite** | `/caveman lite` | Drop filler, keep grammar. Professional but no fluff. |
| **Full** | `/caveman full` | Default. Drop articles, fragments, full grunt. |
| **Ultra** | `/caveman ultra` | Maximum compression. Telegraphic. Abbreviate everything. |

Level persists until changed or session ends.

## Benchmarks

| Task | Normal | Caveman | Saved |
|---|---|---|---|
| Explain React re-render bug | 1180 | 159 | 87% |
| Fix auth middleware | 704 | 121 | 83% |
| Set up PostgreSQL pool | 2347 | 380 | 84% |
| Explain git rebase vs merge | 702 | 292 | 58% |
| Architecture: microservices vs monolith | 446 | 310 | 30% |
| **Average** | **1214** | **294** | **65%** |

Caveman only affects *output* tokens — reasoning/thinking tokens untouched.

## Install (Claude Code)

```bash
claude plugin marketplace add JuliusBrussee/caveman && claude plugin install caveman@caveman
```

## Always-On Snippet

Paste into CLAUDE.md or any agent's rules file for session-persistent activation:

```
Terse like caveman. Technical substance exact. Only fluff die.
Drop: articles, filler (just/really/basically), pleasantries, hedging.
Fragments OK. Short synonyms. Code unchanged.
Pattern: [thing] [action] [reason]. [next step].
ACTIVE EVERY RESPONSE. No revert after many turns. No filler drift.
Code/commits/PRs: normal. Off: "stop caveman" / "normal mode".
```

## Skills

| Skill | Trigger | What it does |
|---|---|---|
| `caveman-commit` | `/caveman-commit` | Terse commit messages. Conventional Commits. ≤50 char subject. |
| `caveman-review` | `/caveman-review` | One-line PR comments. No throat-clearing. |
| `caveman-compress` | `/caveman:compress <file>` | Rewrites CLAUDE.md files into caveman-speak; saves ~46% of input tokens per session. Keeps human-readable original as `.original.md`. |

## Integration Notes

- Auto-activates in Claude Code via SessionStart hooks (installed by plugin)
- Can also be always-on via the snippet above in CLAUDE.md — no plugin required
- Toggle off per-session with "stop caveman" or "normal mode"
- Code blocks, URLs, file paths, commands, headings, dates, version numbers pass through untouched — only prose gets compressed
