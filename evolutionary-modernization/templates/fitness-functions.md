# Fitness functions — <migration name>

A fitness function is an **automated, objective check that an architectural quality still holds**. It runs in CI, in a scheduled job, or continuously against prod telemetry — not in someone's head. Each one needs: what it measures, where it runs, the threshold, and what failing it *does*.

| # | Quality protected | Check (automated) | Where it runs | Pass threshold | On failure |
|---|---|---|---|---|---|
| 1 | Latency of new path ≥ legacy | compare p99 of accepted requests, candidate vs control | continuous, on canary traffic | candidate p99 ≤ 1.1 × control p99 | hold canary; alert |
| 2 | No error-rate regression | candidate 5xx rate vs control | continuous | candidate ≤ control + 0.1pp | auto-rollback canary step |
| 3 | Backlog stays drainable | queue age p99 | continuous | < 60 s | page; shed load |
| 4 | Cost per order non-increasing | $ per processed order, candidate vs control | daily | ≤ control | flag in review; block next step |
| 5 | Deployment safety | every deploy goes through canary stages with automated analysis | CI/CD pipeline gate | no stage skipped | block release |
| 6 | Rollback time bounded | game-day: time to flip kill switch and confirm legacy serving | weekly drill | < 5 min | fix the runbook/automation |
| 7 | No new cyclic dependency | static analysis of module/service deps | CI on every PR | 0 new cycles across the seam | fail the build |
| 8 | Contract held | consumer-driven contract tests across the seam | CI | all pass | fail the build |

## Notes

- Prefer functions that **fail the build or auto-rollback** over functions that only emit a metric.
- Tie canary metrics to *user-visible* outcomes (goodput, accepted-request latency, error rate), not internal proxies that can look healthy while users suffer.
- A function that never fails is either trivially true or not actually testing anything — review it.
