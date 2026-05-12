# ADR-<NNN>: <short decision title>

- **Status:** proposed | accepted | superseded by ADR-<NNN> | deprecated
- **Date:** <YYYY-MM-DD>
- **Deciders:** <names, including operators>
- **Review by:** <YYYY-MM-DD or trigger condition> — when do we revisit this?

## Context

What forces are at play — business goals, the ranked quality-attribute scenarios, hard constraints, the scale band. Link the QAW scenario list and the trade-off matrix. Why is this decision hard-to-reverse (which is why it gets an ADR)?

## Decision

We will <do X>. State it in one or two sentences.

## Alternatives considered (and why rejected)

| Option | Summary | Why not chosen |
|---|---|---|
| <A> | | |
| <B> | | |
| <C — chosen> | | *(chosen — see below)* |

Do not omit this section. An ADR without rejected alternatives is a press release.

## Evidence

- Benchmark / spike result for the riskiest assumption: <numbers, link>.
- Relevant prior incidents or constraints.

## Consequences

- **Positive:** <what gets easier / better>.
- **Negative / costs:** <what we now have to live with — operational cost, coupling, latency, $>.
- **Follow-ups:** <work this decision creates>.

## Exit criteria

This decision is working if <measurable conditions>. Revisit it if <triggers>: e.g. write QPS exceeds X, cross-region latency exceeds Y, cost per order rises above Z.
