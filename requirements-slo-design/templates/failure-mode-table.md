# Failure-mode table — critical flow: <name>

Walk the critical flow **step by step**. For each step, ask: what can fail, who notices, what do we do, and can we see it happening? This is flow-oriented on purpose — a component list misses the interesting failures (queues backing up, retries amplifying load, a dependency degrading).

| Flow step | Failure mode | Blast radius | Detection signal | Mitigation / design response | Recovery |
|---|---|---|---|---|---|
| 1. accept event at API | datastore unavailable | new events rejected | 5xx rate, DB conn errors | write to durable queue first, ack on enqueue | drain queue when DB returns |
| 2. enqueue | queue at capacity | back-pressure to API | queue depth, enqueue latency | bounded queue + load shed (429) at threshold | scale partitions; shed lowest-priority |
| 3. worker delivers | downstream endpoint slow/erroring | one tenant's deliveries delayed | per-tenant delivery latency, retry count | per-tenant concurrency cap; retry **only when helpful**; backoff + jitter; circuit-break after N | resume on success; alert tenant after threshold |
| 4. worker delivers | poison message (always fails) | worker time wasted, backlog grows | retry count per message, backlog age | max retries → dead-letter; fast-fail stale work | manual/automated DLQ replay |
| 5. worker crashes mid-delivery | at-least-once → possible duplicate | downstream sees dupes | duplicate-delivery metric | idempotency key in payload; consumers dedupe | none needed if contract is at-least-once |

## Saturation watch-list

| Resource | Limit | Signal | What happens at the limit |
|---|---|---|---|
| Queue | <partitions × throughput> | queue depth, age | load shed at API |
| DB writer | <writes/sec> | replication lag, CPU | shard / introduce write buffer |
| Worker pool | <concurrency> | in-flight count | autoscale; back-pressure |

## Overload doctrine (decide up front)

- **Retries:** retry only transient errors, capped, with exponential backoff + jitter. Never retry into a saturated dependency.
- **Queues:** bounded. A full queue sheds load; it does not grow without limit.
- **Load shedding / fast-fail:** at <threshold>, reject new work cheaply (429) rather than degrade everyone.
- **Goodput, not throughput:** under overload, optimize for successfully-completed work, not raw request count.
