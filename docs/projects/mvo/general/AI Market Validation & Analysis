# **1.7 AI Market Validation & Analysis (v1.0)**

## **Purpose**

The AI Market Validation & Analysis system reduces uncertainty around idea viability by combining **behavioral hypothesis validation** with **multi-signal market-level validation**. The system provides **evidence-backed signals**, not predictions or guarantees of success.

The system explicitly treats market validation as **probabilistic**, not deterministic. All outputs are advisory and non-authoritative.

---

## **1.7.1 Customer & Market Framing**

**Customer Segment Identification**
The system MUST identify and explicitly state the primary customer segment(s) implied by the idea, including:

* Primary user
* Buyer (if different)
* Context of use
* Environment (consumer, SMB, enterprise, regulated, etc.)

**Market Context Framing**
The system MUST classify the idea’s market context:

* B2C / B2B / B2B2C
* Horizontal vs vertical
* New category vs existing category

This framing is displayed at the top of the analysis to anchor interpretation of all subsequent signals.

---

## **1.7.2 Multi-Layer Hypothesis System (Existing + Extended)**

### **Behavioral Hypothesis Layers**

The system MUST extract and maintain **five behavioral hypotheses**, one per layer:

1. **Existence** – The problem actually occurs for the identified segment
2. **Awareness** – Users recognize and articulate the problem
3. **Consideration** – Users actively seek solutions or workarounds
4. **Intent** – Users show intent to adopt a solution
5. **Pay Intention** – Users demonstrate willingness to pay

### **Hypothesis Structure**

For each hypothesis, the system MUST store:

* Layer name
* Hypothesis title
* Detailed description
* Evidence summary (qualitative + quantitative)
* Confidence level (Low / Medium / High)
* Supporting sources
* Contradicting signals (if any)

---

## **1.7.3 Web & Evidence-Based Validation (Existing)**

**Evidence Collection**
For each hypothesis, the system MUST:

* Generate at least 10 targeted search queries
* Perform web research using those queries
* Collect observable behavioral evidence (forums, reviews, complaints, workflows)
* Collect quantitative references (studies, surveys, datasets, statistics)

**Evidence Classification**

* Behavioral (what people do)
* Stated (what people say)
* Quantitative (numbers, studies, metrics)

Evidence MUST be explicitly linked to hypotheses and timestamped.

---

## **1.7.4 Market-Level Validation Signals (NEW)**

In addition to hypotheses, the system MUST extract **market-level signals** that validate **demand strength, timing, and fit**.

These signals are **orthogonal** to hypotheses and MUST be presented separately.

---

### **A. Demand Intensity & Momentum**

**Purpose:** Validate whether demand is emerging, growing, stable, or declining.

**Signals MAY include:**

* Search interest trends (e.g., Google Trends)
* Social media trend velocity (TikTok, X, Reddit, YouTube)
* Forum and community activity growth
* Frequency of problem mentions over time

**Output Classification:**

* Emerging
* Accelerating
* Stable
* Declining
* Seasonal

---

### **B. Problem Salience & Urgency**

**Purpose:** Validate how painful and time-sensitive the problem is.

**Signals MAY include:**

* Language intensity analysis
* Repetition of workaround behaviors
* Evidence of productivity loss, revenue loss, or risk exposure
* Time-frequency indicators (“daily”, “weekly”, “always”)

---

### **C. Existing Spend & Budget Signals**

**Purpose:** Validate whether money is already flowing.

**Signals MAY include:**

* Existing paid tools used as substitutes
* Consulting or service-based solutions
* Pricing benchmarks in adjacent markets
* Evidence of ad spend in the category

---

### **D. Competitive Landscape & Saturation**

**Purpose:** Understand market crowding without judging winners.

**Signals MAY include:**

* Number of direct competitors
* Indirect substitutes
* App store density
* Feature convergence
* Venture funding density

**Classification Examples:**

* Sparse
* Fragmented
* Crowded
* Commoditized

---

### **E. Switching & Adoption Friction**

**Purpose:** Validate ease of adoption.

**Signals MAY include:**

* Workflow lock-in
* Data migration cost
* Contractual friction
* Emotional or habit-based resistance

---

### **F. Distribution & Reachability**

**Purpose:** Validate whether customers are reachable.

**Signals MAY include:**

* Existing gathering points (platforms, communities)
* Dominant incumbents controlling channels
* Organic vs paid acquisition feasibility
* Cold-start risks (network effects, marketplaces)

---

### **G. Geographic & Cultural Fit**

**Purpose:** Validate market scope.

**Signals MAY include:**

* Regional concentration of demand
* Regulatory constraints
* Cultural differences in problem perception
* Language-specific demand patterns

**Classification:**

* Local
* Regional
* Global
* Global with localization

---

### **H. Timing & Market Readiness**

**Purpose:** Validate why this opportunity exists now.

**Signals MAY include:**

* Enabling technology shifts
* Regulatory changes
* Cost curve changes
* Behavioral shifts (remote work, AI adoption)

---

### **I. Economic Plausibility (Lightweight)**

**Purpose:** Prevent unrealistic market assumptions without heavy modeling.

**Constraints:**

* NO full TAM/SAM/SOM modeling
* NO revenue predictions

**Allowed Outputs:**

* Order-of-magnitude estimates
* Comparable market references
* Bottom-up sanity checks (users × plausible price)

---

## **1.7.5 Signal Synthesis & Interpretation**

**Signal Aggregation**
The system MUST synthesize:

* Behavioral hypothesis confidence
* Market-level signal strength

**Presentation Rules:**

* Signals are displayed side-by-side
* Conflicting signals MUST be highlighted
* No single composite “score” is allowed

**Explicit Disclaimer**
The UI MUST state:

> “These signals reduce uncertainty but do not predict success. Market outcomes depend on execution, timing, and external factors.”

---

## **1.7.6 Final Market Assessment**

**Assessment Output Includes:**

* Summary of strongest validating signals
* Summary of weakest or missing signals
* Key unknowns that require direct validation
* Suggested next validation steps (tests, experiments, interviews)

**Pivot Guidance**
The system MAY suggest:

* Reframing the customer segment
* Narrowing scope
* Adjusting distribution assumptions
* Testing pricing earlier

AI MUST NOT:

* Declare the idea viable or non-viable
* Rank ideas
* Predict success or failure

---

## **1.7.7 Analysis Storage & Versioning**

**Storage Requirements**

* All analyses MUST be stored per idea
* Each analysis is versioned and timestamped
* Analyses are immutable once completed

**Change Tracking**
Users MUST be able to:

* View historical analyses
* Compare changes between versions
* See which edits triggered new analyses

---

# **How This Appears in the App (UI Structure)**

## **Idea Detail Page → “Market Validation” Tab**

### **Section Order (Fixed)**

1. **Market Snapshot**

   * Customer segment
   * Market type
   * Geography
   * Timing context

2. **Behavioral Hypotheses**

   * Collapsible per layer
   * Confidence + evidence
   * What’s validated vs unknown

3. **Market Signals**

   * Each signal category as a card
   * Clear classification labels
   * Evidence snippets + sources

4. **Conflicts & Gaps**

   * Explicit contradictions
   * Missing signals
   * Risk flags

5. **Synthesis & Next Steps**

   * What looks strong
   * What is weak
   * What to test next

---

## **Progressive Disclosure Rules**

* Summary first
* Evidence collapsible
* No walls of text by default
* Advanced users can expand everything

---

## **Design Guardrails**

* No scores
* No success labels
* No green/red verdicts
* AI is visually distinct and collapsible
* Humans > AI signals (consistent with earlier sections)
