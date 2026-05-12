---
name: system-design-skills
description: Bundle of system design skills for greenfield architecture, architecture tradeoff analysis, brownfield modernization, and concrete LikeC4 output. Use when someone asks to design a system, architect a service, compare architecture options, modernize a legacy system, run a design review, define SLOs, choose among hard-to-reverse technical options, or emit C4/LikeC4 diagrams.
---

# System Design Skills

Router bundle for system design work. Pick the smallest approach that fits the situation, run it with the user, then produce a concrete LikeC4 model using the bundled `likec4` skill.

## Routing

| Situation | Load |
| --- | --- |
| Greenfield, startup scale, mid scale, first product version, new API, internal tool, or service contract baseline | `requirements-slo-design/SKILL.md` |
| Multiple plausible architectures, expensive platform/data/consistency/service-boundary choice, regulated or transaction-heavy flow | `architecture-tradeoff-analysis/SKILL.md` |
| Brownfield modernization, legacy replacement, monolith decomposition, live traffic migration, service extraction, scaling a hot path safely | `evolutionary-modernization/SKILL.md` |
| Writing, fixing, validating, exporting, or generating final `.c4` / `.likec4` diagrams | `likec4/SKILL.md` |

Default route: greenfield -> requirements/SLO first; brownfield -> evolutionary modernization; hard-to-reverse choice -> tradeoff analysis. Methods can be cumulative for large programs, but do not run every method by default.

## Triage

Ask only what cannot be inferred from the prompt or repo:

1. Greenfield or brownfield?
2. Scale band: startup (<100k users), mid (~100k-10M), or large/high-stakes.
3. Tech, org, compliance, latency, datastore, cloud, deadline, or team constraints.
4. The genuinely hard decision: framing requirements, choosing among architectures, or shipping safely under live traffic.

## Workflow

1. Load the routed subskill, not every subskill.
2. Produce that method's lightweight artifacts and templates.
3. Escalate to another method only when the current method exposes a real need:
   - quality-attribute conflict -> `architecture-tradeoff-analysis`
   - live migration risk -> `evolutionary-modernization`
   - unclear service contract or SLOs -> `requirements-slo-design`
4. Finish with `likec4/SKILL.md` for the concrete `.c4` deliverable.

## Evaluation Lens

Judge every design against measurable outcomes: latency, throughput/goodput, availability/error rate, cost per useful unit of work, and operability. Record hard-to-reverse decisions as ADRs. Keep diagrams, SLOs, failure modes, rollout confidence, and rollback paths concrete enough to change engineering behavior.
