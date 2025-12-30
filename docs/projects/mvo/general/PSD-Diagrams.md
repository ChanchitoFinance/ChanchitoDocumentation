## 1) End-to-end workflow (Decision Validation loop)

```mermaid
flowchart TD
  A[Create & Frame Decision<br/>≤ 3 minutes] --> B[Capture Options + Assumptions<br/>confidence + risk flags]
  B --> C[Internal Validation<br/>bias-aware async cycle]
  C --> D[External Validation<br/>fast tests + evidence]
  D --> E[Synthesis: Belief vs Reality<br/>compare + conflicts + missing evidence]
  E --> F[Decision Recommendation<br/>rules-based first]
  F --> G[Finalize Decision Log<br/>immutable + audit]
  G --> H[Execute]
  H --> I[Review<br/>expected vs actual]
  I --> J[Learnings + Pattern Tags<br/>templates + retrieval]
  J --> A
```

## 2) Decision lifecycle (status state machine)

```mermaid
stateDiagram-v2
  [*] --> Draft

  Draft --> Validating: Start validation cycle
  Validating --> Draft: Reframe / add options
  Validating --> Decided: Finalize decision log

  Decided --> Executed: Ship / implement
  Executed --> Reviewed: Review due date reached

  Reviewed --> [*]

  note right of Decided
    Decision Log becomes immutable
    + audit trail enabled
  end note
```

## 3) Bias-aware internal alignment (flow)

```mermaid
flowchart LR
  subgraph Setup[Setup Cycle]
    S1[Owner defines prompt + window] --> S2[Blind mode ON]
    S2 --> S3[Assumptions listed + risk flags]
  end

  subgraph Input[Async Inputs]
    I1[Each teammate submits vote/score] --> I2[Structured comments tied to assumptions]
    I2 --> I3[Submission locks personal response]
  end

  subgraph Reveal[Reveal & Aggregate]
    R1[Blind window closes] --> R2[Reveal responses]
    R2 --> R3[Aggregate scores + participation distribution]
    R3 --> R4[Surface conflicts + flagged assumptions]
  end

  Setup --> Input --> Reveal
```

## 4) External validation (signal acquisition + aggregation)

```mermaid
flowchart TD
  X1[Select validation method] --> X2{Method}
  X2 -->|Poll/Survey/Concept test| X3[Collect responses + artifacts]
  X2 -->|Landing page test| X4[Capture conversion metrics<br/>manual → integrations]
  X2 -->|Link-out evidence| X5[Attach external docs/screenshots/URLs]

  X3 --> X6[Normalize into Signal objects]
  X4 --> X6
  X5 --> X6

  X6 --> X7[Weight evidence<br/>source tags + confidence]
  X7 --> X8[Attach to Decision + Options]
```

## 5) Synthesis view (Belief vs Reality + recommendation path)

```mermaid
flowchart TD
  A[Internal Signals<br/>votes + confidence + comments] --> C[Comparison Layer]
  B[External Signals<br/>tests + metrics + artifacts] --> C

  C --> D{Mismatch?}
  D -->|Yes| E[Highlight conflicts<br/>+ critical assumptions at risk]
  D -->|No| F[Confirm alignment<br/>+ sufficient evidence]

  E --> G{Missing evidence?}
  G -->|Yes| H[Prompt: add evidence<br/>or reduce scope]
  G -->|No| I[Proceed to recommendation]

  F --> I
  H --> I

  I --> J[Rules-based Recommendation<br/>+ rationale draft]
  J --> K[Finalize Decision Log immutable]
```

## 6) Core product objects (data model / class diagram)

```mermaid
classDiagram
  class Decision {
    +id: UUID
    +title: string
    +context: text
    +type: enum(feature|pricing|GTM|ops|strategy)
    +ownerId: UUID
    +status: enum(Draft|Validating|Decided|Executed|Reviewed)
    +createdAt: datetime
    +finalizedAt: datetime?
  }

  class Option {
    +id: UUID
    +decisionId: UUID
    +name: string
    +description: text
  }

  class Assumption {
    +id: UUID
    +decisionId: UUID
    +optionId: UUID?
    +statement: text
    +confidence: 0..100
    +risk: enum(low|medium|high|critical)
  }

  class Signal {
    +id: UUID
    +decisionId: UUID
    +optionId: UUID?
    +type: enum(internal|external)
    +source: string
    +weight: 0..1
    +summary: text
    +createdAt: datetime
  }

  class InternalInput {
    +id: UUID
    +signalId: UUID
    +userId: UUID
    +score: number
    +comment: text
    +blindSubmittedAt: datetime
  }

  class EvidenceArtifact {
    +id: UUID
    +signalId: UUID
    +kind: enum(link|file|screenshot|doc)
    +uri: string
    +notes: text
  }

  class MetricExpectation {
    +id: UUID
    +decisionId: UUID
    +name: string
    +target: string
    +timeHorizon: string
  }

  class OutcomeActual {
    +id: UUID
    +decisionId: UUID
    +name: string
    +value: string
    +measuredAt: datetime
  }

  class Learning {
    +id: UUID
    +decisionId: UUID
    +summary: text
    +patternTags: string[]
  }

  class DecisionLog {
    +id: UUID
    +decisionId: UUID
    +finalChoice: string
    +rationale: text
    +lockedAt: datetime
  }

  Decision "1" --> "many" Option
  Decision "1" --> "many" Assumption
  Decision "1" --> "many" Signal
  Signal "1" --> "many" EvidenceArtifact
  Signal "1" --> "many" InternalInput
  Decision "1" --> "many" MetricExpectation
  Decision "1" --> "many" OutcomeActual
  Decision "1" --> "0..1" DecisionLog
  Decision "1" --> "0..1" Learning
```

## 7) Validation funnel (metric stages)

```mermaid
flowchart LR
  D0[Decision Created] --> D1[Internal Inputs Collected]
  D1 --> D2[External Signals Added]
  D2 --> D3[Decision Finalized]
  D3 --> D4[Execution Started]
  D4 --> D5[Review Completed]
  D5 --> D6[Learning Stored / Tagged]
```
