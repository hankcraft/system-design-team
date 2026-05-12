---
name: evolutionary-modernization
description: "Approach 3 of system design, evolutionary modernization with fitness functions. Use for brownfield work, e.g. legacy modernization, decomposing a monolith, extracting a service, replacing shared infrastructure, scaling a hot path under live traffic, especially when a big-bang rewrite is risky. Ships controlled, reversible changes bounded by automated checks, canaries, and rollback paths (Strangler Fig)."
---

# Evolutionary modernization with fitness functions

Brownfield design is not a one-shot blueprint; it's a series of controlled changes, each bounded by automated checks, canaries, and a rollback path. The mental model is the **Strangler Fig**: route a slice of traffic through the new component, prove it on real traffic, expand, and retire the old path only when the metrics hold. **Fitness functions** are the automated guardrails that say "this change keeps the qualities we care about".

Who runs it: staff engineers, system architects, SRE-heavy teams. It converges slower than a rewrite *on paper*, but it has the lowest delivery risk in a live system — and the source material strongly favours gradual modernization, release safety, and continual validation over speculative replacement.

## Process

1. **Baseline the current system.** Architecture, dependencies, recent incidents, current SLIs, queue behaviour, operational load (interrupt rate, toil). You can't improve what you haven't measured. → output: a LikeC4 *current-state* model (via bundled `likec4`) + a dependency/flow map.
2. **Decompose the critical flows; find the seams.** Where can you cut with bounded blast radius? Which misbehaviour actually matters? → output: seam list with blast-radius notes.
3. **Define fitness functions and guardrails** for latency, error rate, backlog age, cost, deployment safety, rollback time. These are *automated*, run in CI and/or continuously. → output: `templates/fitness-functions.md`.
4. **Route a small slice of traffic** through the new component via a gateway, feature flag, or strangler facade. Persist commands durably; keep the legacy path authoritative until proven. → output: a LikeC4 *target-state* (or transitional) model.
5. **Canary and compare** control vs candidate on the agreed metrics. Stage it: internal → 5% → 25% → 100%. → output: `templates/canary-plan.md`.
6. **Protect the path:** bounded queues, timeouts, "retry only when helpful", backoff with jitter, load shedding / fast-fail for stale work, a kill switch back to legacy. → output: rows in the canary plan + alerts.
7. **Retire the old path** only when metrics and operational behaviour are consistently acceptable — then update diagrams, ADRs, runbooks. Define retirement criteria up front so the migration *ends*. → output: updated LikeC4 model + `templates/rollback-runbook.md`.

For any genuinely irreversible cut along the way (a data-model change, a consistency-model change, a contract break), pause and run **`architecture-tradeoff-analysis`** on that specific decision.

## Worked mini-case

*Checkout extraction from a legacy monolith.* Route **only order submission** through a new API behind the existing gateway; persist commands durably; process asynchronously; keep fulfillment on the legacy side until correctness is proven. Stages: internal traffic → 5% → 25% → 100%. Compare: accepted-request latency, error rate, queue age, backlog drain time, cost per order. Fail fast on stale queued work; bound retries with backoff + jitter; keep a kill switch that routes all traffic back to the monolith if the canary deviates.

## Failure modes to watch

Zombie legacy code that never gets retired (the migration never *ends*); canaries tied to the wrong metrics; load shedding that quietly hides the need to scale; retries that amplify overload into a cascade; teams drowning in operational interrupts before the migration completes; coexistence cost underestimated (duplicate data paths, anti-corruption layers, extra tests, more dashboards).

## Required artifacts

- LikeC4 current-state model **and** target-state model (via bundled `likec4`)
- Dependency / flow map of the critical paths
- Fitness-function definitions, runnable in CI / continuously (`templates/fitness-functions.md`)
- Canary plan with stages, metrics, and abort criteria (`templates/canary-plan.md`)
- Contract + regression tests across the seam
- Canary dashboards; queue-age and saturation alerts
- Rollback runbook with a kill switch (`templates/rollback-runbook.md`)
- ADRs tracking each irreversible cut; defined legacy-retirement criteria

## Engineer-facing checklist

- [ ] Current and target **flows** are mapped — not just current and target components.
- [ ] Fitness functions and canary metrics are explicit and automated.
- [ ] Rollback, kill switch, and runbook are ready *before* rollout.
- [ ] Queue age, retries, and overload behaviour are bounded and observable.
- [ ] Legacy-retirement criteria are defined, so the migration actually ends.
