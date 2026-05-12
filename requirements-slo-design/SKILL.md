---
name: requirements-slo-design
description: "Approach 1 of system design, requirements-and-SLO-first. Use for greenfield startup or mid-scale systems, first product versions, new APIs, internal tools, or re-baselining a service's contract. Starts from user journeys and measurable outcomes, picks the simplest viable architecture, and delays irreversible commitments. Hand off to bundled likec4 at the end."
---

# Requirements-and-SLO-first design

Start from **user journeys and measurable outcomes**, not from components. Order of operations: business requirements → non-functional targets (SLIs/SLOs) → scale estimates → the *smallest* architecture that satisfies them. This minimizes accidental complexity and delays irreversible commitments. It is the best default for greenfield startup and most mid-scale work, and for any team under time, budget, or skill constraints.

Who runs it: a software engineer can usually do the first pass alone. A system architect adds boundaries, long-horizon constraints, and review discipline. **Pull SREs in when SLOs, alerts, and failure modes are defined — not after the diagram is "done".**

## Process

Work through these *with the user*. Each step has a verifiable output.

1. **Identify the primary user flow and the most expensive unhappy path.** Write them down. → output: one or two sentences each.
2. **Write the functional requirements and the hard constraints.** Separate "must" from "nice". → output: a short list.
3. **Define minimum viable SLIs and SLOs** for latency, availability, throughput, and — where relevant — correctness or freshness. Two or three is enough to start. → output: `templates/slo-worksheet.md`.
4. **Estimate demand:** peak vs average traffic, growth, read/write mix, storage growth, headroom. → output: `templates/capacity-worksheet.md`.
5. **Choose the simplest viable topology** that meets the targets — not the most fashionable one. Name the complexity you rejected and why. → output: a topology sketch (becomes the LikeC4 container view).
6. **Run a small failure-mode analysis on the critical path** and define observability from the start (the signals that prove each SLO). → output: `templates/failure-mode-table.md`.
7. **Validate the shape** with load, rollback, and resilience tests; **record the few decisions that are hard to reverse** as ADRs. → output: a basic test plan + ADR(s).

Then invoke bundled **`likec4`** to turn the topology + critical flow into a `.c4` file (context view, container view, one dynamic view for the primary flow).

## Default bias

You usually do **not** need to start with a specific application style such as microservices. A typical startup-scale design is: a small stateless API tier, a durable queue, idempotent workers, a managed relational store for config/tenant data — instrumented with request latency, error rate, queue depth, and backlog age. Asynchronous durability often *improves* availability at the price of *higher* latency — so track that latency explicitly rather than pretending it's free.

**Premature decomposition is the classic failure here.** Splitting services before the team has clear invariants or operational maturity adds accidental complexity faster than resilience. Keep a modular monolith or a small service set until something concrete forces a split.

## Worked mini-case

*B2B webhook delivery platform, startup scale.* Critical flow: "accept event → persist durably → deliver with retries." Targets: acceptance availability, end-to-end delivery latency, queue age, cost per million events. First design: stateless API tier + durable queue + idempotent workers + managed relational store for tenant/config — not a swarm of microservices.

## Failure modes to watch

Unrealistic SLOs; weak traffic estimates; forgetting long-tail latency; observability as an afterthought; premature decomposition; getting attached to the first plausible diagram (this method can under-explore alternatives — if a real quality-attribute conflict appears, escalate to `architecture-tradeoff-analysis`).

## Required artifacts (keep small but concrete)

- LikeC4 context view + container view + one dynamic flow (via bundled `likec4`)
- SLO worksheet (`templates/slo-worksheet.md`)
- Capacity worksheet (`templates/capacity-worksheet.md`)
- Failure-mode table (`templates/failure-mode-table.md`)
- A basic test plan (load / rollback / resilience)
- ADR(s) for hard-to-reverse decisions only

## Engineer-facing checklist

- [ ] Primary user flow and unhappy path are written down.
- [ ] Two or three SLIs/SLOs are explicit.
- [ ] Scale estimate has peak, average, and headroom assumptions.
- [ ] Simplest viable topology is chosen, and the rejected complexity is named.
- [ ] Diagrams, failure modes, and initial tests are created *before* build-out starts.
