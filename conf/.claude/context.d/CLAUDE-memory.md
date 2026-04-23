# Memory Protocol — MemPalace

MemPalace is the sole memory store. Never write to Claude Code's native auto-memory (`.md` files).

## On Wake-Up

1. `mempalace_status` — palace overview
2. `mempalace_diary_read` (agent_name: "claude", last_n: 5) — recent sessions
3. `mempalace_search` (query: "<topic>") — before answering about past work

## At End of Session / Save Checkpoint

Call all three:
1. `mempalace_diary_write` — AAAK-formatted summary
2. `mempalace_add_drawer` — verbatim quotes, decisions, code snippets
3. `mempalace_kg_add` — entity relationships (optional)

AAAK format:
```
SESSION:<date>|<topic-slug>|
TASK:<what was done>
FINDINGS:
- <finding>
★★★
```

## Rules

- Search before answering about past work — never guess.
- **Before generating any artifact** (document, plan, summary, config, script) search MemPalace first. If a prior version exists, retrieve and present it — do not regenerate from inference.
- If a fact changes: `kg_invalidate` then `kg_add`.
- Hooks auto-save every 15 exchanges when `MEMPAL_ENABLED=true`.
