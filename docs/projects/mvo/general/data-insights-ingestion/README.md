# Data Insights Ingestion (IDL)

This folder documents the **Insights Digestion Layer (IDL)**.

IDL is responsible for converting **implicit user behavior** into
**quantifiable insights** and **phase decisions** across the
Idea → Prototype → MVP pipeline.

IDL does **not** ask users explicit questions.
All insights are derived from **observed behavior** and **commitment signals**.

## What IDL Produces

For every idea, IDL generates four artifacts:

1. **Phase Scorecard**

   - Attention & consumption quality
   - Demand signals
   - Commitment / monetization intent
   - Funnel drop-offs

2. **Decision Gate Result**

   - PASS / HOLD / FAIL
   - Machine-derived explanation

3. **Status Flag**

   - new / active_discussion / trending / validated / controversial

4. **Next-Best Action**
   - Concrete recommendation for iteration or promotion

## What IDL Is Not

- Not a raw analytics warehouse
- Not a survey or explicit feedback system
- Not a replacement for PostHog

PostHog collects events. IDL **interprets** them.

## Documents

- `event-model.md` — which events exist and why they matter
- `scoring-and-gates.md` — how scores and phase decisions are computed
- `visibility-and-outputs.md` — what different users can see
