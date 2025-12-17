# 1) CORS in Production 

**Goal:** safe, predictable cross-origin behavior.

### Baseline

* [ ] Remove all `AllowAnyOrigin()` calls
* [ ] Use explicit allow-lists for origins
* [ ] Allow **only** required HTTP methods
* [ ] Never use `AllowAnyHeader()` in production
* [ ] Verify HTTPS-only origins

### Hardened

* [ ] If using credentials, allow **specific** origins (never wildcard)
* [ ] Verify cookie settings (SameSite, Secure)
* [ ] Test subdomain handling (if using wildcards)
* [ ] Set a reasonable `SetPreflightMaxAge()` (≈10–60 min)

### Advanced

* [ ] Integration tests for CORS behavior
* [ ] Browser-based test tool runs in CI (automated)
* [ ] Manual spot-checks with `curl` scripts
* [ ] Periodic security audits to catch gaps

# 2) EF Core Query Performance

**Goal:** faster queries, fewer DB round trips.

### Baseline

* [ ] Use `AsNoTracking()` for read-only queries
* [ ] Load only needed fields (projection)
* [ ] Avoid lazy-loading by default; prefer eager/explicit when predictable

### Hardened

* [ ] Batch operations; avoid calling `SaveChanges()` in a tight loop
* [ ] Use compiled queries for frequent calls
* [ ] Push work to the database (indexes, foreign keys, tuned queries)

### Advanced

* [ ] Monitor & log generated SQL; catch slow queries early
* [ ] Add performance regression tests in CI
* [ ] Periodically review indexes & query plans with real data

# 3) Versioning & Releases

**Goal:** predictable releases, safe upgrades, clear communication.
**API style:** **URL path versioning** (e.g., `https://api.example.com/v1/...`, `.../v2/...`).

### Baseline

* [ ] Use **Semantic Versioning** for the product: `MAJOR.MINOR.PATCH`.
* [ ] Maintain a human-readable **CHANGELOG**; tag every release with notes.
* [ ] Pin third-party dependencies; record minimum supported versions.
* [ ] **Choose URL path versioning** for APIs (`/v1`), and **document a default version** (or require explicit version in all client URLs).
* [ ] **No breaking changes inside a path version**: once `/v1` is public, only additive, backward-compatible updates.

### Hardened

* [ ] **Run versions in parallel** (serve `/v1` and `/v2` concurrently); keep endpoints, error shapes, and pagination **stable per version**.
* [ ] **Routing rules** defined: canonical base URL, redirects, and how clients discover `/v2`.
* [ ] **Deprecation signaling**: return `Deprecation` and `Sunset` headers for `/v1` with links to migration docs and EOL dates.
* [ ] **Cache & rate-limit isolation**: keys and quotas **include version** (avoid `/v1` and `/v2` impacting each other).
* [ ] **Schema & docs per version**: separate OpenAPI/Swagger files and **versioned documentation** (`/docs/v1`, `/docs/v2`).
* [ ] **Webhooks & callbacks** are versioned (`/v1/hooks/...`) and documented with migration notes.
* [ ] **Idempotency keys** and **error codes** remain stable within each version; new codes are additive only.
* [ ] DB migrations are version-aware and **reversible**; guard rails to prevent breaking `/v1`.

### Advanced

* [ ] **Release policy**: LTS window for `/v1` (security & critical fixes only), active feature work on `/v2+`.
* [ ] **Compatibility & contract tests** executed per version (server ↔ SDKs ↔ examples).
* [ ] **Traffic steering** for rollouts: canary `/v2`, shadow traffic, easy rollback to `/v1`.
* [ ] **Data migration strategy**: dual-write/dual-read or adapters so `/v1` continues to work during upgrades.
* [ ] **Monitoring & analytics by version**: usage, latency, errors, and deprecation adoption tracked separately.
* [ ] **SDKs & examples** published per API version; sample requests show full path (`/v1/...`).
* [ ] **Feature flags** scoped by version; never change behavior of an existing `/v1` endpoint behind a flag.
* [ ] **Sunset playbook**: announce, deprecate with headers, provide migration guides, staged disablement, and final removal timeline.



# 4) Validation Objectives Checklist

## 1. **Strategic Alignment & Goal Definition**

* [ ] Define the **top X hypotheses or features** to validate this cycle.
* [ ] For each, document:

  * [ ] **User problem** being solved.
  * [ ] **Business goal** it supports (e.g., retention, conversion, engagement).
  * [ ] **Success criteria** — measurable target or threshold.
  * [ ] **Failure criteria** — what would make us stop or pivot.
* [ ] Link each validation to a **north-star metric** to ensure long-term alignment.
* [ ] Schedule a brief **Vision Check** before kicking off: *Does this experiment move us toward our strategic product direction?*


## 2. **Unified Measurement Setup**

### Backend Metrics

* [ ] Track **API performance**: response times, error rates, data accuracy.
* [ ] Log **feature-specific backend events** (e.g., premium activations, retries, errors).
* [ ] Record **usage by user segment** (e.g., free vs. premium, active vs. new users).
* [ ] Integrate metrics into a **central dashboard** (Grafana, Datadog, or Mixpanel backend events).

### Frontend Metrics

* [ ] Measure **UI interaction events** (clicks, navigations, completion rates).
* [ ] Track **conversion funnels** (free → engaged → premium).
* [ ] Monitor **page performance** (load times, crash rates).
* [ ] Use **session recording or heatmaps** (Hotjar, FullStory) for UX insights.

### Data Integrity

* [ ] Assign a **Data Owner** to verify metrics consistency across systems.
* [ ] Validate tracking plans before release to avoid data gaps.


## 3. **User Feedback & Insight Loop**

* [ ] Collect **in-app qualitative feedback** (micro surveys, NPS, chat).
* [ ] Schedule **interviews or usability sessions** for deeper insights.
* [ ] Categorize feedback by:

  * [ ] Type (UX / Feature / Value / Reliability).
  * [ ] Segment (Free users / Premium / New / Churned).
* [ ] Quantify recurring patterns (frequency-weighted analysis).
* [ ] Store insights in a shared **“Validation Learnings” space** (Notion, Confluence, etc.).


## 4. **Real-World Business Validation**

* [ ] Test **Premium or Paywalled Feature** experiments to validate perceived value.
* [ ] Be transparent — communicate it as “Beta Access” or “Early Access” to maintain trust.
* [ ] Measure:

  * [ ] **Conversion rate** and **retention** among upgraded users.
  * [ ] **Usage differences** between free and premium users.
  * [ ] **Revenue potential** and user willingness to pay.
* [ ] Review **user feedback from premium testers** to refine pricing and value proposition.

---

## 5. **Rapid Iteration with Controlled Risk**

* [ ] Deploy via **feature flags** or **controlled rollouts** (staged percentage release).
* [ ] Establish an **error budget** and rollback plan for quick recovery.
* [ ] Run **A/B or multivariate tests** where applicable to ensure valid learnings.
* [ ] Prior to release, verify:

  * [ ] UX and accessibility quality.
  * [ ] Analytics events firing correctly.
  * [ ] Clear user communication (no dark patterns).

## 6. **Learning, Decisions & Recycling**

* [ ] After each release:

  * [ ] Review **quantitative results** (metrics, KPIs).
  * [ ] Analyze **qualitative feedback** (user insights).
  * [ ] Combine both to assess **true user value**.
* [ ] Classify each experiment:

  * [ ] **Recycle** – promising, iterate next version.
  * [ ] **Discard** – doesn’t meet criteria or user fit.
  * [ ] **Renew** – promising but needs refinement or different target.
* [ ] Update your **Validation Dashboard** with outcomes and learning summaries.
* [ ] Run a **“Validation Retrospective”** with product, design, and engineering teams to ensure shared understanding.


## 7. **Continuous Improvement & Institutional Learning**

* [ ] Schedule **biweekly validation reviews** to align on progress and insights.
* [ ] Keep a **shared repository of validated learnings** (what worked, what didn’t, why).
* [ ] Identify **patterns of success** and scale what consistently drives results.
* [ ] Integrate top learnings into the **product roadmap** and **strategy planning** sessions.


## 8. **Governance & Ownership**
* [ ] Document every experiment with:

  * [ ] Hypothesis
  * [ ] Metrics & Data Sources
  * [ ] Results Summary
  * [ ] Decision (Recycle / Discard / Renew)


# 5) C.R.A.F.T.E.D

## 1) Templates & Governance

* [ ] Create a **CRAFTED prompt template** (Markdown) with sections:

  * [ ] `<context>` (links, diffs, stack traces, config)
  * [ ] `<role>`
  * [ ] `<action>`
  * [ ] `<format>`
  * [ ] `<tone>`
  * [ ] `<examples>`
  * [ ] `<definition_of_done>`
* [ ] Add **Prompt DOs/DON’Ts** (data minimization, no secrets, cite sources).
* [ ] Publish in `/docs/ai/CRAFTED-guide.md` and a **Notion** page.
* [ ] Add a **“Prompt of the Week”** space with great internal examples.

## 2) Workflow Integration

* [ ] Add **PR template** section: “Used CRAFTED? Link prompt + response”.
* [ ] Add **Issue template** section for AI help (bug triage, refactor, tests).
* [ ] Add **Commit hook** (optional, soft warning) to check PR body for CRAFTED link.
* [ ] Add **CODEOWNERS** for `/docs/ai/*` (AI Champions review).

## 3) Tooling & Context Plumbing

* [ ] IDE snippets for **VS Code/JetBrains**: `CRAFTED.md` auto-insert.
* [ ] One-click context collectors (scripts or CLI):

  * [ ] `git diff --name-only` + target file snippets
  * [ ] Last CI error log excerpt
  * [ ] package.json/Dockerfile/tsconfig excerpts
* [ ] Redaction step: scrub **secrets/PII** before sending context.
* [ ] Standard **output formats** (code-only, JSON spec, Markdown report).


## 4) Use-Case Playbooks (prebuilt prompts)

* [ ] **Bug triage**: context = stack trace + failing test; action = root-cause + fix plan.
* [ ] **Refactor**: context = file + style guide; action = propose diff + risks.
* [ ] **Unit tests**: action = generate edge cases (empty, nulls, perf).
* [ ] **API review**: action = contract check + breaking change analysis.
* [ ] **Security**: IAM/S3 policy audit (role = cloud security engineer).
* [ ] **Performance**: hotspot analysis + O(n)→O(n log n) rewrite plan.
* [ ] **Docs**: generate README/change log from diff.
* [ ] **Conventional commits**: natural language → normalized commit.


