# Rollback runbook — <component / migration name>

Owner: <name> · Last drilled: <YYYY-MM-DD> · Kill switch: <flag / gateway rule>

## When to roll back

Roll back **immediately**, no debate, if any of:
- Candidate error rate > control + <0.1pp> sustained <5 min>.
- Accepted-request p99 > <1.1×> control sustained <10 min>.
- Queue age p99 > <60 s> and rising.
- Any Sev1/Sev2 plausibly attributable to the new path.
- Data integrity doubt (divergence between new and legacy stores).

Don't wait to "understand it first" — roll back, then investigate.

## How to roll back (the kill switch)

1. <Set flag `X` to `legacy` / apply gateway rule `Y`> — routes 100% of `<slice>` to the legacy path.
2. Confirm: <dashboard panel> shows candidate traffic → 0; legacy error rate normal.
3. If commands were queued for the new path: <drain to legacy handler | leave for replay after fix> — pick one and document which.
4. Post in <channel>: what happened, current state, owner.
5. Open an incident if user-visible. File a bug for the root cause before re-attempting the canary.

Target: steps 1–2 complete in **< 5 min**. If you can't hit that, the runbook/automation is the bug — fix it.

## Data reconciliation (if dual-write / async was involved)

- How to detect divergence: <query / job>.
- How to reconcile: <source of truth = legacy; replay/repair procedure>.
- Idempotency: confirm replays won't double-apply (idempotency keys on commands).

## After rollback

- [ ] Root cause identified and a fitness function added that would have caught it.
- [ ] Canary plan updated (new abort condition? earlier soak?).
- [ ] Re-drill the kill switch if it didn't behave as expected.
- [ ] Decide go/no-go to retry.

## Drill log

| Date | Drill type | Time-to-rollback | Issues found | Fixed? |
|---|---|---|---|---|
| | game day | | | |
