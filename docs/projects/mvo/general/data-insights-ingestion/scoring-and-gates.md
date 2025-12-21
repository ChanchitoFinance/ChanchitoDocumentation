# Scoring and Phase Gates

IDL converts daily aggregates into three normalized scores (0–100).

## Core Scores

### Attention Quality Score (AQS)

Measures **how deeply content is consumed**.

Derived from:

- Dwell time
- Watch percentage (median / p75)
- Scroll depth

### Demand Score (DS)

Measures **whether users want more**.

Derived from:

- Saves
- Follows
- Shares
- Comments
- Return visits

### Monetization Intent Score (MIS)

Measures **commitment under friction**.

Derived from:

- Waitlist joins
- Invite / demo requests
- Deposits or preorders
- Behavior after pricing exposure

## Phase Gates (Simplified)

### Idea → Prototype

- AQS and DS above baseline
- Sufficient engaged users
- No strong negative signals

### Prototype → MVP

- MIS crosses commitment threshold
- Activation and usage confirmed

### MVP → Scale / Pivot

- Retention stabilizes
- MIS sustained
- System health acceptable

## Decision Outputs

Each evaluation produces:

- Phase
- PASS / HOLD / FAIL
- Decision reason (machine-generated)
- Next-best-action recommendation

Thresholds are configurable and category-dependent.
