# PROJECT SPECIFICATION DOCUMENT (Aligned & Concise)

## Decision Validation Layer for Modern Teams

### 1) Executive Summary

This product is a **Decision-as-a-Product platform**: it helps teams **validate decisions before committing**, by combining **internal conviction** with **external evidence** in one workflow — and then **closing the loop** so learning compounds over time.

**Core promise:**
Most tools collect opinions. **We help you make better decisions — and learn from them.**

### 2) Problem

Teams repeatedly make high-stakes bets (features, pricing, GTM, ops) with:

- Fragmented signals (Slack, docs, surveys, roadmaps)
- Untracked assumptions
- Biased dynamics (loudest voice / HIPPO)
- No durable record of _why_ decisions were made
- No comparison of expected vs actual outcomes

Result: slow alignment, repeated mistakes, founder/team amnesia.

### 3) Category & Differentiation

**We sell: Decision Validation.**
Not “feedback,” not “voting,” not “roadmaps.”

**Differentiators (owned wedges):**

1. **Decision is the primary object** (not ideas/features)
2. **Unified internal + external signal layer**
   “What we think” vs “what reality says”
3. **Institutional decision memory** (searchable, compounding)
4. **Bias reduction by design** (blind/staged input, separation of authorship/evaluation, evidence weighting)
5. **Founder/small-team native execution** (fast, opinionated, outcome-first)
6. **Learning loops** (expected → actual → model improves)

### 4) Target Users (ICP-first)

**Primary ICP: Founders / small teams (2–20) making constant bets**

- Need speed + clarity
- Low PM/research overhead
- Want validation in **24–72 hours**, not quarterly planning cycles

**Secondary: SMB / mid-market teams (8–80) needing risk reduction + justification**

- Care about political cover, proof, recurring decision cycles
- Value internal history and external evidence for alignment

### 5) Core Product Objects

**Decision**

- Title, context, type (feature/pricing/GTM/ops/strategy), owner
- Status: Draft → Validating → Decided → Executed → Reviewed
- Options/alternatives

**Assumptions**

- Explicit assumption list per decision/option
- Confidence scoring + risk flagging (critical/high-risk)

**Signals**

- Internal signals (team votes, confidence, comments)
- External signals (polls, tests, landing page conversion, evidence artifacts)
- Evidence weighting + source tagging

**Outcome & Learning**

- Expected outcome metrics (defined at decision time)
- Actual outcomes (post-execution)
- Learning summary linked back to assumptions + decision

### 6) Core Workflows

#### A) Create & Frame Decision (≤ 3 minutes)

- Define decision + options
- Capture key assumptions + confidence/risk

#### B) Bias-Aware Internal Alignment (async-first)

- Blind/staged input before visibility
- Separate idea ownership from evaluation
- Time-boxed feedback window
- Internal voting + structured comments tied to assumptions

#### C) External Validation (fast, lightweight)

- Run external tests (poll/survey/concept test)
- Define audience target (later: segmentation)
- Aggregate results into comparable signals

#### D) Synthesis: Belief vs Reality

- Compare internal confidence vs external evidence
- Highlight conflicts, missing evidence, critical assumptions at risk
- Produce a **Decision Recommendation** (rules-based first; AI later)
- Final Decision Log: rationale locked + immutable record

#### E) Execution → Review → Compounding Learning

- Track outcome metrics
- Post-decision review: expected vs actual
- Store learnings + pattern tags
- Enable search + reuse via templates

### 7) Functional Requirements (MVP → v1)

**Decision Management**

- Create/edit decision, options, types, statuses
- Immutable “Final Decision Log” once decided

**Assumptions & Confidence**

- Assumption capture, confidence scoring, risk flags

**Internal Validation**

- Voting/scoring
- Blind input mode (until submission)
- Time-boxed cycles
- Structured comments tied to assumptions

**External Validation**

- Simple polls/surveys/concept tests (native or link-out initially)
- Landing page link + conversion capture (manual first, integrations later)
- Signal aggregation into the decision

**Synthesis**

- Internal vs external comparison view
- Evidence weighting + “missing evidence” prompts
- Recommendation + rationale draft

**Memory & Retrieval**

- Searchable decision history (filters by type, owner, outcome, tags)
- Templates for recurring decisions
- Basic pattern discovery (what worked / failed)

**Notifications**

- Cycle deadlines, new input, decision finalized, review due

### 8) Non-Functional Requirements

- **Low cognitive load:** create a decision in under 3 minutes
- **Async-first:** no meetings required
- **Fast:** core views load < 2 seconds
- **Reliability:** finalized decisions immutable + audit logs
- **Security:** permissions/roles, encryption at rest/in transit
- **Exportability:** CSV/PDF/JSON exports
- **Scalability:** supports 2 → 100+ users without redesign

### 9) Metrics (Behavioral, Decision-Centric)

**Validation Funnel**

- Decision created → internal inputs → external signals → decision finalized → review completed

**Signal Quality**

- Ratio: external evidence added per decision
- % decisions with critical assumptions explicitly logged
- Conflict rate: internal confidence vs external evidence mismatch

**Speed**

- Time to decision (target bands: 24–72h for founders; 1–2 weeks for SMB teams)

**Bias Reduction**

- Participation distribution (avoid dominance)
- Blind-mode usage rate
- Time-box adherence rate

**Learning Compounds**

- Review completion rate
- % decisions linked to outcomes
- Reuse of templates / reference to past decisions
- Outcome calibration over time (expected vs actual accuracy improving)

### 10) Explicit De-scope (Later / Optional Expansion)

These can exist later, but are **not** core to the decision-validation positioning:

- Social “infinite feeds”, creator economy loops
- Gamification-first mechanics
- Investor discovery tooling
- Public idea marketplace

If introduced, they should be framed as **external evidence acquisition**, not the product’s identity.
