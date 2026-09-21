# Watch advice — RESOLVED ledger

<!-- drained-through: (none) -->
<!-- armed-at: 0276a2bf4cfa5ee85c8e805e69b3b4f1eb638f20 -->

The drain ledger for the TRACKED watch advice (`advice.jsonl` / `advice.md`, appended by
`codex_watch.py` on every reviewed commit): one row per NEEDS_REVISION / ERROR entry that
has been handled, keyed `ts · ref`. `python3 .claude/scripts/watch_drain.py status` counts
what is owed; `list --unresolved` prints the entries that still need a row; `rotate
--through <ts>` archives what is rowed and advances the marker above. The `armed-at`
marker is the review's starting line: commits at or before it are never selected.

| status | meaning |
|---|---|
| `FIXED §<id>` | cured in that commit |
| `REFUTED` | the finding is wrong; the row says why |
| `RETRY — <when>` | an ERROR entry re-reviewed later |

| ts | kind | ref | model | verdict | status | evidence |
|---|---|---|---|---|---|---|
