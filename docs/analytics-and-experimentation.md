# WordCase Analytics and Experimentation Rules

This document defines WordCase's canonical analytics taxonomy, required payload contract, privacy boundaries, and experiment guardrails.

Its purpose is to make analytics useful without allowing telemetry or experiments to alter puzzle truth, fairness, or player trust.

This is a product and engineering behavior document.
When code, dashboards, or experiments disagree with this document, this document is the source of truth until intentionally updated.

---

## 1. Core Principles

WordCase analytics should be:
- deterministic enough to audit gameplay truth
- minimal enough to protect player privacy
- structured enough to compare milestones and experiments safely
- resilient enough that analytics failures never block play

WordCase analytics should **not**:
- define or mutate puzzle semantics
- collect raw guess text or spoiler-bearing payloads
- capture free-form personal data
- encourage experiments that change fairness rules silently

---

## 2. Canonical Event Taxonomy

### 2.1 Naming convention
Use a stable dotted namespace:
- `session.*` for app/session lifecycle
- `daily.*` for Daily Case lifecycle
- `weekly.*` for Weekly Caseboard and Weekly Resolution lifecycle
- `share.*` for spoiler-safe sharing behavior
- `archive.*` for archive/practice behavior (non-canonical)
- `sync.*` for account/sync reconciliation behavior

Event names must be lower-case, deterministic, and backward compatible once used in production.

### 2.2 Session flow events
- `session.start`
- `session.resume`
- `session.pause`
- `session.end`
- `session.route_viewed`
- `session.recovered_from_persisted_state`

### 2.3 Daily flow events
- `daily.case_loaded`
- `daily.case_started`
- `daily.guess_submitted`
- `daily.guess_rejected`
- `daily.feedback_rendered`
- `daily.hint_requested`
- `daily.hint_applied`
- `daily.case_solved`
- `daily.case_failed`
- `daily.result_viewed`
- `daily.result_shared`
- `daily.rollover_presented`

### 2.4 Weekly flow events
- `weekly.board_viewed`
- `weekly.evidence_applied`
- `weekly.unlock_eligible`
- `weekly.resolution_started`
- `weekly.resolution_guess_submitted`
- `weekly.resolution_guess_rejected`
- `weekly.resolution_solved`
- `weekly.resolution_failed`
- `weekly.result_viewed`

### 2.5 Sync and canonical integrity events
- `sync.outbox_enqueued`
- `sync.outbox_sent`
- `sync.outbox_failed`
- `sync.merge_resolved`
- `sync.canonical_conflict_detected`
- `sync.canonical_conflict_resolved`

### 2.6 Taxonomy governance rule
When adding a new event:
1. justify why an existing event cannot answer the question,
2. define required fields and nullability,
3. classify as canonical (`daily.*`, `weekly.*`, selected `sync.*`) or non-canonical,
4. update this file in the same change.

---

## 3. Required Event Fields

All emitted events must include the following contract unless explicitly documented as nullable.

### 3.1 Global required fields (all events)
- `event_name`: canonical taxonomy name (for example `daily.case_started`)
- `event_version`: schema version for that event payload
- `event_id`: unique UUID for deduplication
- `event_ts_utc`: ISO-8601 UTC timestamp generated at emit time
- `client_build_id`: app build/version identifier
- `platform`: `android` in current target scope
- `environment`: `dev`, `staging`, or `prod`
- `app_session_id`: unique app-open session identifier
- `user_scope`: `guest` or `account`

### 3.2 Required puzzle/session identity fields (daily and weekly gameplay events)
- `puzzle_id`: canonical daily or weekly puzzle identifier
- `puzzle_type`: `daily_case` or `weekly_resolution`
- `puzzle_session_id`: identifier for a single attempt run/session
- `puzzle_state_version`: local state schema version
- `attempt_index`: 1-based attempt number for guess/hint/result events

### 3.3 Required version pins for truth/audit safety
The following are required on all canonical gameplay events (`daily.*`, `weekly.*`) and on sync conflict events that reference canonical results:
- `content_version_pin`: published content package version
- `validation_snapshot_version_pin`: dictionary/validation snapshot version
- `feedback_algorithm_version_pin`: feedback evaluator version
- `hint_rules_version_pin`: hint policy version (if hints are enabled for that mode)

### 3.4 Required experiment fields
All events emitted while any experiment assignment is active must include:
- `experiment_assignments`: array of `{ experiment_id, variant_id }`
- `assignment_source`: deterministic assignment source label

If no assignment exists, send an empty array.

### 3.5 Optional but standardized fields
- `weekly_case_id` (required for weekly board events, nullable elsewhere)
- `error_code` (for rejected, failed, and sync-failure events)
- `network_state` (`online`, `offline`, `degraded`) for sync-sensitive events
- `latency_ms` for transport/performance events

---

## 4. Privacy Boundaries and Prohibited Payloads

### 4.1 Data minimization rule
Collect only the minimum data required to answer product questions tied to milestones.
No speculative data hoarding.

### 4.2 Prohibited payloads
Do **not** send:
- raw guess text
- answer word text
- unrevealed clue text that can spoil active puzzles
- free-form user-entered text
- contact lists, message content, clipboard data, installed app lists, precise location, or advertising IDs unless explicitly approved by product/privacy policy
- cross-session behavioral fingerprints beyond documented IDs above
- any direct personal identifiers (email, phone, legal name) inside analytics events

### 4.3 Allowed puzzle telemetry examples
Allowed payloads should use abstracted values such as:
- guess validity outcome category
- guess pattern summary counts
- attempts used
- solved/failed state
- hint usage booleans and counts

### 4.4 Retention and access guardrail
- restrict access to raw event streams to authorized product/engineering roles
- prefer aggregated dashboards for routine decisions
- define retention windows in implementation configs; do not retain raw payloads indefinitely by default

---

## 5. Experiment Guardrails (Non-Negotiable)

Experiments must never alter puzzle truth.

### 5.1 Forbidden experiment targets
No experiment may change, per variant:
- puzzle answer
- accepted/rejected guess policy
- feedback algorithm behavior
- attempt limits that redefine canonical solve/fail semantics
- hint truthfulness or contradiction rules
- daily eligibility windows
- canonical result conflict resolution rules

### 5.2 Allowed experiment scope
Experiments may test presentation and engagement layers that do not redefine truth, such as:
- non-spoiler UI copy
- button placement and visual hierarchy
- reminder timing (within notification policy)
- non-canonical archive discovery surfaces

### 5.3 Experiment implementation rule
All experiment toggles must be read **after** canonical game rules are resolved.
Game-rule functions must not branch directly on experiment variant IDs.

### 5.4 Experiment review checklist
Before launch of any experiment:
1. verify no puzzle-truth mutation path exists,
2. verify required event fields are present,
3. verify privacy prohibited fields are absent,
4. verify kill-switch behavior,
5. document hypothesis and success/failure metrics.

---

## 6. Minimum Instrumentation by Milestone

Instrumentation should grow with product risk.

### Milestone 1 (core vertical slice)
Minimum required:
- session lifecycle: `session.start`, `session.end`, `session.recovered_from_persisted_state`
- daily loop outcomes: `daily.case_started`, `daily.case_solved`, `daily.case_failed`
- hint usage: `daily.hint_requested`, `daily.hint_applied`

Goal: confirm first-session completion, solve/fail clarity, and resume reliability.

### Milestone 2+ (real daily system; required baseline)
Minimum required (in addition to Milestone 1):
- daily identity/version pins on all canonical daily events
- guess attempt funnel: `daily.guess_submitted`, `daily.guess_rejected`, `daily.feedback_rendered`
- result flow: `daily.result_viewed`, `daily.result_shared`
- rollover/session continuity: `daily.rollover_presented`, `session.resume`
- sync integrity baseline: `sync.outbox_enqueued`, `sync.outbox_sent`, `sync.outbox_failed`

This Milestone 2+ baseline is required before claiming trustworthy daily analytics.

### Milestone 3 (weekly system)
Add required weekly instrumentation:
- `weekly.board_viewed`
- `weekly.evidence_applied`
- `weekly.resolution_started`
- `weekly.resolution_solved`
- `weekly.resolution_failed`

### Milestone 4 (account and cross-device sync)
Add required sync/conflict instrumentation:
- `sync.merge_resolved`
- `sync.canonical_conflict_detected`
- `sync.canonical_conflict_resolved`

### Milestone 5+
Add social/retention instrumentation only when features are live, maintaining the same privacy and truth guardrails.

---

## 7. Implementation and Failure Behavior

- analytics emission must be asynchronous and non-blocking
- analytics transport failures must not affect gameplay outcome
- event retries must preserve `event_id` for idempotency
- if telemetry is unavailable, gameplay and save/sync still proceed normally

When in doubt, choose fairness, privacy, and deterministic puzzle truth over measurement breadth.
