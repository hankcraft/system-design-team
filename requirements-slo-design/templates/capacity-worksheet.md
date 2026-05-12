# Capacity worksheet — <service / flow name>

Date: <YYYY-MM-DD> · Owner: <name>

State every number as an *assumption* with a basis. A wrong-but-explicit estimate beats a vibe.

## Demand

| Quantity | Average | Peak | Basis / assumption |
|---|---|---|---|
| Requests/sec (or events/sec) | | | e.g. 1M events/day ÷ 86 400, peak = 5× average during business hours |
| Concurrent users / connections | | | |
| Read : write ratio | | | |
| Payload size (p50 / p99) | | | |

## Growth

- Horizon: <e.g. 12 months>
- Growth assumption: <e.g. 2× traffic, 3× stored data>
- Trigger to re-plan: <e.g. sustained > 60% of provisioned capacity>

## Storage

| Data set | Row/object count | Size each | Total | Growth/yr | Retention |
|---|---|---|---|---|---|
| | | | | | |

## Headroom & sizing

- Target utilization ceiling: <e.g. 60% CPU / 70% queue throughput at peak>
- Implied capacity: <derive from demand × headroom>
- Scaling unit: <what you add when you add capacity — instances? shards? partitions?>
- Known scaling cliff: <e.g. single-writer DB at ~X writes/sec, then we shard>

## Cost (per unit of useful work)

| Component | Sizing | Est. monthly cost | Cost per <1M events / order / …> |
|---|---|---|---|
| | | | |
