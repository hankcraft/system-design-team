# Canary plan — <component being introduced>

Date: <YYYY-MM-DD> · Owner: <name> · Kill switch: <flag / gateway rule name>

## What's changing

- New component: <name> — replaces / fronts <legacy component> for <which slice of traffic>.
- Legacy path remains authoritative until: <retirement criterion>.
- Routing mechanism: <gateway rule | feature flag | strangler facade>.
- Data: <how commands/data are persisted; how dual-write or shadow-read is handled; anti-corruption layer?>.

## Stages

| Stage | Traffic | Soak time | Promote if (all true) | Abort/rollback if (any) |
|---|---|---|---|---|
| 0 | internal / synthetic only | 1 day | smoke tests pass; no errors | any error |
| 1 | 5% | 1 day | fitness fns 1–4 pass; no SLO burn | candidate error rate > control + 0.1pp; p99 > 1.1× control; queue age > 60s |
| 2 | 25% | 2 days | same, sustained | same |
| 3 | 100% | 1 week | same, sustained; cost per order ≤ control | same |
| 4 | legacy path disabled | — | retirement criteria met (below) | — |

## Metrics compared (candidate vs control)

- Accepted-request latency (p50 / p99) — successful requests only.
- Error rate (4xx that indicate our fault, 5xx).
- Goodput under any traffic spike during the soak.
- Queue age p99 and backlog drain time.
- Cost per <order / event / unit>.
- Operational interrupts attributable to the new path.

## Guardrails on the new path

- Bounded queue (depth limit <N>); load shed (429) above <threshold>.
- Timeouts on every downstream call; **retry only transient errors**, capped at <N>, exponential backoff + jitter.
- Fast-fail / dead-letter for stale queued work older than <T>.
- Circuit breaker on flaky downstreams after <N> consecutive failures.
- Kill switch: <one action> routes 100% back to legacy in < <T>.

## Retirement criteria for the legacy path

All true for <2 weeks>: candidate at 100%; fitness functions green; no Sev1/Sev2 attributable to the new path; cost per unit ≤ legacy; runbooks/diagrams/ADRs updated. Then: remove legacy code, remove the flag, archive the ADR thread.
