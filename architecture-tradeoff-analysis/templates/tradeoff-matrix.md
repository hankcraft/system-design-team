# Trade-off matrix — <system name>

Scale: `++` strong / `+` ok / `0` neutral / `−` weak / `−−` poor. Coarse on purpose.

## Candidates

- **A — <name>:** <one paragraph>
- **B — <name>:** <one paragraph>
- **C — <name>:** <one paragraph>

## Scoring

| Quality (top scenarios) | Weight | A | B | C | Notes |
|---|---|---|---|---|---|
| Latency (successful-request p99) | | | | | |
| Throughput / goodput under spike | | | | | |
| Availability (AZ/region loss) | | | | | |
| Consistency / correctness invariants | | | | | |
| Cost (per unit of useful work) | | | | | |
| Operability (observe, deploy, roll back) | | | | | |
| **Reversibility** (cost to undo this choice later) | | | | | |
| Security / blast radius | | | | | |
| Modifiability | | | | | |

## Sensitivity points (one decision strongly affects one quality)

| Decision | Affects | Candidates affected | Note |
|---|---|---|---|
| e.g. choice of replication factor | availability vs cost | A, B | |

## Trade-off points (one decision affects two qualities in opposite directions)

| Decision | Helps | Hurts | Candidates |
|---|---|---|---|
| e.g. synchronous cross-region commit | consistency | write latency, cost | B |

## Risks & non-risks

- **Risk:** <assumption that, if wrong, breaks a candidate> — *to be retired by:* <benchmark/spike>.
- **Non-risk:** <thing people worried about that the analysis shows is fine>.

## Recommendation

Chosen: **<A/B/C>**. One paragraph on why, referencing the top scenarios and the benchmark result. → Record in an ADR; build the LikeC4 model via bundled `likec4`.
