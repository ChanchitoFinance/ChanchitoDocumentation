# Event Model (Implicit Signals)

IDL relies exclusively on **implicit events** emitted by the product.
No explicit “would you pay” or survey-style inputs exist.

## Event Categories

### 1. Attention / Consumption

Signals that indicate **interest quality**.

| Event                   | Trigger            | Insight    |
| ----------------------- | ------------------ | ---------- |
| feed_impression         | Idea card rendered | Reach      |
| detail_view_start / end | Idea opened        | Dwell time |
| video_progress          | Media watched      | Watch %    |
| scroll_depth            | Content consumed   | Depth      |

### 2. Engagement / Demand

Signals that indicate **desire for more**.

| Event           | Trigger           | Insight             |
| --------------- | ----------------- | ------------------- |
| save / bookmark | Wants to revisit  | Demand              |
| follow_idea     | Wants updates     | Subscription intent |
| share           | Shares externally | Virality            |
| comment / reply | Invests effort    | Engagement depth    |
| return_session  | Comes back        | Sustained interest  |

### 3. Commitment / Friction Tolerance

Signals that indicate **willingness to commit**.

| Event              | Trigger                    | Insight             |
| ------------------ | -------------------------- | ------------------- |
| pricing_view       | Pricing surfaced naturally | Price tolerance     |
| waitlist_join      | Requests access            | Intent              |
| invite_request     | Asks for entry             | Intent              |
| demo_request       | Requests interaction       | Strong intent       |
| deposit / preorder | Accepts friction           | Monetization signal |

## Design Principle

Events are **descriptive**, not interrogative.

We measure what users **do**, not what they **say**.
