# SLO worksheet — <service / flow name>

Date: <YYYY-MM-DD> · Owner: <name> · Status: draft | agreed

## Critical user flow

> One or two sentences. What does a user actually do, end to end?

## Most expensive unhappy path

> One or two sentences. What's the worst realistic failure of that flow and who feels it?

## SLIs / SLOs (pick 2–4 — resist more)

| # | SLI (what is measured, where, how) | SLO target | Window | Why this matters |
|---|---|---|---|---|
| 1 | e.g. proportion of `POST /events` returning 2xx, measured at the LB | 99.9% | 28d rolling | tenants drop events if we 5xx |
| 2 | e.g. p99 latency of *successful* `POST /events` at the LB | < 200 ms | 28d rolling | webhook senders time out fast |
| 3 | e.g. p99 end-to-end delivery latency (accept → first delivery attempt) | < 30 s | 28d rolling | downstreams expect "near real time" |
| 4 | e.g. correctness: 0 duplicate deliveries beyond at-least-once contract | — | per release | breaks idempotency promises |

Notes:
- Measure **successful** and **failed**-request latency separately. A fast 500 is not a win.
- If the flow is asynchronous, latency includes queue age — make that explicit.
- For data systems, add a *freshness* or *staleness* SLI where relevant.

## Error budget policy

- Budget = 1 − SLO over the window.
- When budget is exhausted: <e.g. freeze risky deploys, prioritize reliability work>.

## Alerting (the signals that prove each SLO)

| SLO # | Page-worthy signal | Ticket-worthy signal |
|---|---|---|
| 1 | burn rate > 14.4× over 1h | burn rate > 1× over 6h |
| 2 | p99 > target for 10 min | p99 trending up over 24h |
