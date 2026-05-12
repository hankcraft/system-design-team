# Quality-attribute scenarios — <system name>

Date: <YYYY-MM-DD> · Participants (incl. operators): <names>

A good scenario is testable: **source → stimulus → artifact → environment → response → response measure**. Vague goals ("scalable", "secure") are not scenarios — turn them into measurable ones.

## Ranked scenarios

| Rank | Quality attr | Stimulus | Environment | Expected response | Response measure |
|---|---|---|---|---|---|
| 1 | Availability | Single AZ goes down | Peak traffic | Order writes keep succeeding | p99 < 500 ms; error rate < 0.1% during the outage |
| 2 | Consistency | Two clients reserve the last unit concurrently | Normal | Exactly one succeeds | 0 oversells, ever |
| 3 | Performance | 5× traffic spike (flash sale) | 10 min sustained | System stays within SLO or sheds gracefully | goodput ≥ 90% of offered until shed; no cascading failure |
| 4 | Modifiability | Add a new payment provider | Dev time | Localized change | ≤ 1 service touched; ≤ N days |
| 5 | Security | Compromised internal service | Any | Blast radius bounded | no access to ledger data; audit log intact |
| 6 | Cost | 2× growth | 12 months | Cost grows sub-linearly or linearly, not worse | $ per order non-increasing |

> Rank by business impact. The top 3–5 drive the comparison; the rest are recorded but not deeply traced.

## Notes on contentious scenarios

- <Scenario #N>: why stakeholders disagree on the target, and what would resolve it (a benchmark? a product decision?).
