# WordCase Implementation Contracts

This document defines stable TypeScript-facing contracts for state machine shape, persisted canonical entities, analytics payloads, and content package runtime validation.

Its purpose is to eliminate implementation guessing in app and package code.
When implementation details need to change, update this file first and version changes intentionally.

---

## 1. Contract Stability Rules

- Contract names and field names in this file are **stable** once used in production code.
- Additive change is preferred over breaking change.
- Breaking contract changes require:
  - contract version bump,
  - migration/update notes,
  - updates to dependent docs and tests in the same change.
- UI-facing convenience state may evolve, but must map cleanly to canonical contracts below.

---

## 2. Canonical Session State Machine Contracts

These contracts describe canonical gameplay/session truth boundaries for Daily Case and Weekly Resolution.
They are intentionally explicit so app routing/state layers do not invent hidden states.

### 2.1 TypeScript state enums

```ts
export type DailySessionState =
  | 'unopened'
  | 'in_progress'
  | 'solved_unassisted'
  | 'solved_assisted'
  | 'failed'
  | 'missed'
  | 'abandoned';

export type DailySessionTransition =
  | 'open_case'
  | 'submit_valid_guess'
  | 'submit_invalid_guess'
  | 'apply_hint'
  | 'solve'
  | 'fail'
  | 'rollover_protect_carryover'
  | 'abandon_carryover'
  | 'mark_missed'
  | 'acknowledge_result';

export type WeeklyResolutionState =
  | 'locked'
  | 'eligible'
  | 'in_progress'
  | 'solved'
  | 'failed_run'
  | 'expired_unresolved';

export type WeeklyResolutionTransition =
  | 'evidence_threshold_reached'
  | 'start_resolution'
  | 'submit_valid_guess'
  | 'submit_invalid_guess'
  | 'solve'
  | 'fail_run'
  | 'retry_run'
  | 'weekly_rollover_expire';
```

### 2.2 Canonical daily transition rules

```ts
export type DailyStateTransitionMap = {
  unopened: 'in_progress' | 'missed';
  in_progress:
    | 'in_progress'
    | 'solved_unassisted'
    | 'solved_assisted'
    | 'failed'
    | 'abandoned';
  solved_unassisted: never;
  solved_assisted: never;
  failed: never;
  missed: never;
  abandoned: never;
};
```

Rules:
- `solved_unassisted`, `solved_assisted`, `failed`, `missed`, and `abandoned` are terminal canonical daily states.
- A Protected Carryover Daily remains `in_progress` after rollover until one terminal state is reached.
- `abandoned` is allowed only for Protected Carryover Dailies and implies `mark_missed` semantics for canonical eligibility.
- Result acknowledgment is UI-local and **must not** rewrite canonical outcome state.

### 2.3 Canonical weekly transition rules

```ts
export type WeeklyStateTransitionMap = {
  locked: 'eligible' | 'expired_unresolved';
  eligible: 'in_progress' | 'expired_unresolved';
  in_progress: 'in_progress' | 'solved' | 'failed_run' | 'expired_unresolved';
  failed_run: 'in_progress' | 'expired_unresolved';
  solved: never;
  expired_unresolved: never;
};
```

Rules:
- `failed_run` is non-terminal and retryable within the same weekly window.
- `solved` and `expired_unresolved` are terminal for a given `week_id`.
- Carryover daily completion after weekly rollover contributes evidence to the daily record's original `week_id`, not the newly active week.
- Weekly unlock eligibility evaluation (`locked` -> `eligible`) is deterministic and must execute on each canonical evidence mutation and again at week-close finalization for the same `week_id`.
- Conflicting unlock/evidence writes for the same `week_id` use server-authoritative canonical ordering (`recordedAtServer`) as tie-break authority; device-captured timestamps are non-authoritative hints.
- Late-arriving evidence synced after week close must trigger recomputation for that historical `week_id` only and must not be reattributed to the active week.
- Once a `week_id` reaches unlocked/eligible status, reconciliation must never retroactively transition it back to `locked`.

---

## 3. Persisted SQLite Entity Contracts

These are canonical entity names and required fields for local persistence.
Column naming uses snake_case to keep SQL schemas boring and explicit.

### 3.1 `daily_case_records`

One row per canonical daily case identity.

```ts
export interface DailyCaseRecord {
  daily_case_id: string;
  week_id: string;
  publish_date_utc: string; // YYYY-MM-DD
  validity_starts_at_utc: string; // ISO-8601 UTC
  validity_ends_at_utc: string; // ISO-8601 UTC
  content_version_pin: string;
  validation_snapshot_version_pin: string;
  feedback_algorithm_version_pin: string;
  hint_rules_version_pin: string;
  canonical_state: DailySessionState;
  valid_guess_count: number;
  hint_used: 0 | 1;
  attempts_used: number;
  concluded_at_utc: string | null;
  created_at_utc: string;
  updated_at_utc: string;
}
```

### 3.2 `weekly_case_records`

One row per canonical weekly identity.

```ts
export interface WeeklyCaseRecord {
  week_id: string;
  validity_starts_at_utc: string;
  validity_ends_at_utc: string;
  unlock_threshold: number; // v1 = 4
  evidence_count: number;
  resolution_state: WeeklyResolutionState;
  resolution_attempts_used: number;
  resolved_at_utc: string | null;
  expired_at_utc: string | null;
  created_at_utc: string;
  updated_at_utc: string;
}
```

### 3.3 `carryover_daily_records`

Tracks Protected Carryover Daily lifecycle independent from new-day default selection.

```ts
export interface CarryoverDailyRecord {
  carryover_id: string;
  daily_case_id: string;
  source_week_id: string;
  carryover_status: 'protected_active' | 'resolved' | 'abandoned' | 'expired';
  protected_since_rollover_utc: string;
  resolved_at_utc: string | null;
  abandoned_at_utc: string | null;
  expired_at_utc: string | null;
  created_at_utc: string;
  updated_at_utc: string;
}
```

### 3.4 `canonical_result_records`

Canonical conclusion records for daily and weekly outcomes.

```ts
export type CanonicalResultOutcome =
  | 'daily_solved_unassisted'
  | 'daily_solved_assisted'
  | 'daily_failed'
  | 'daily_missed'
  | 'daily_abandoned'
  | 'weekly_resolution_solved'
  | 'weekly_resolution_failed';

export interface CanonicalResultRecord {
  result_id: string;
  puzzle_id: string;
  puzzle_type: 'daily_case' | 'weekly_resolution';
  week_id: string;
  outcome: CanonicalResultOutcome;
  attempts_used: number;
  hint_used: 0 | 1;
  content_version_pin: string;
  validation_snapshot_version_pin: string;
  feedback_algorithm_version_pin: string;
  hint_rules_version_pin: string;
  canonical_precedence_rank: 1 | 2 | 3 | 4;
  concluded_at_utc: string;
  acknowledged_locally: 0 | 1;
  created_at_utc: string;
  updated_at_utc: string;
}
```

Required precedence rank mapping for daily conflicts:
- `1`: solved unassisted
- `2`: solved assisted
- `3`: failed
- `4`: missed/abandoned/in-progress fallback


### 3.5 `active_session_snapshots`

Exact device-local resume snapshots for in-progress puzzle state.
These snapshots preserve resume fidelity and are not canonical shared truth records.

```ts
export interface ActiveSessionSnapshotRecord {
  puzzle_session_id: string;
  puzzle_id: string;
  puzzle_type: 'daily_case' | 'weekly_resolution';
  week_id: string;
  canonical_state: DailySessionState | WeeklyResolutionState;
  content_version_pin: string;
  validation_snapshot_version_pin: string;
  feedback_algorithm_version_pin: string;
  hint_rules_version_pin: string;
  attempts_used: number;
  valid_guess_count: number;
  hint_used: 0 | 1;
  submitted_guesses_json: string;
  current_input: string;
  keyboard_state_json: string;
  fixed_hint_positions_json: string;
  last_route_context: string;
  created_at_utc: string;
  updated_at_utc: string;
  last_interaction_at_utc: string;
}
```

Rules:
- canonical result/history tables store long-lived shared truth.
- active session snapshot storage stores exact device-local resume state.
- result acknowledgment remains device-local UI state and must not rewrite canonical outcome truth.


---

## 4. Analytics Event Contracts

This section pins canonical event names, required properties, and privacy/redaction rules.
It mirrors the taxonomy in `docs/analytics-and-experimentation.md` and gives TypeScript-facing shapes.

### 4.1 Canonical event names

```ts
export type CanonicalAnalyticsEventName =
  | 'session.start'
  | 'session.resume'
  | 'session.pause'
  | 'session.end'
  | 'session.route_viewed'
  | 'session.recovered_from_persisted_state'
  | 'daily.case_loaded'
  | 'daily.case_started'
  | 'daily.guess_submitted'
  | 'daily.guess_rejected'
  | 'daily.feedback_rendered'
  | 'daily.hint_requested'
  | 'daily.hint_applied'
  | 'daily.case_solved'
  | 'daily.case_failed'
  | 'daily.result_viewed'
  | 'daily.result_shared'
  | 'daily.rollover_presented'
  | 'weekly.board_viewed'
  | 'weekly.evidence_applied'
  | 'weekly.unlock_eligible'
  | 'weekly.resolution_started'
  | 'weekly.resolution_guess_submitted'
  | 'weekly.resolution_guess_rejected'
  | 'weekly.resolution_solved'
  | 'weekly.resolution_failed'
  | 'weekly.result_viewed'
  | 'sync.outbox_enqueued'
  | 'sync.outbox_sent'
  | 'sync.outbox_failed'
  | 'sync.merge_resolved'
  | 'sync.canonical_conflict_detected'
  | 'sync.canonical_conflict_resolved';
```

### 4.2 Base required analytics properties

```ts
export interface AnalyticsEventBase {
  event_name: CanonicalAnalyticsEventName;
  event_version: number;
  event_id: string; // UUID
  event_ts_utc: string; // ISO-8601 UTC
  client_build_id: string;
  platform: 'android';
  environment: 'dev' | 'staging' | 'prod';
  app_session_id: string;
  user_scope: 'guest' | 'account';
  experiment_assignments: Array<{ experiment_id: string; variant_id: string }>;
  assignment_source: string | null;
}

export interface CanonicalGameplayAnalyticsFields {
  puzzle_id: string;
  puzzle_type: 'daily_case' | 'weekly_resolution';
  puzzle_session_id: string;
  puzzle_state_version: string;
  attempt_index: number;
  content_version_pin: string;
  validation_snapshot_version_pin: string;
  feedback_algorithm_version_pin: string;
  hint_rules_version_pin: string;
  weekly_case_id?: string | null;
  error_code?: string | null;
  network_state?: 'online' | 'offline' | 'degraded';
  latency_ms?: number;
}
```

### 4.3 Redaction/privacy contract

All analytics payload builders must enforce this compile-time/runtime contract:

```ts
export interface AnalyticsPrivacyGuard {
  forbid_raw_guess_text: true;
  forbid_answer_text: true;
  forbid_unrevealed_clue_text: true;
  forbid_freeform_user_text: true;
  forbid_direct_personal_identifiers: true;
  forbid_precise_location_and_ad_id: true;
}
```

Required behavior:
- reject payload assembly that attempts to include prohibited fields.
- allow only abstracted gameplay summaries (for example validity category, attempts used, hint-used boolean).
- keep event transport failures non-blocking for gameplay and persistence flows.

---

## 5. Content Package Runtime Validation Contracts

These interfaces define app/runtime validation boundaries for daily and weekly package activation.
Validation must run before content becomes active in session state.

### 5.1 Runtime package contracts

```ts
export interface RuntimeHintData {
  kind: 'single_letter_reveal';
  maxHints: 1;
  unlockAfterValidGuesses: 1;
}

export interface RuntimeDailyCasePackage {
  id: string;
  kind: 'daily_case';
  publishDateUtc: string; // YYYY-MM-DD
  publishWindow: { startsAtUtc: string; endsAtUtc: string };
  weekId: string;
  dayIndexInWeek: 1 | 2 | 3 | 4 | 5 | 6 | 7;
  contentVersion: string;
  answer: string;
  title: string;
  caseFrame: string;
  clueText: string;
  hintData: RuntimeHintData;
  dictionaryVersion: string;
  feedbackAlgorithmVersion: string;
  difficulty: { tier: 'gentle' | 'standard' | 'challenging'; flags: string[] };
  isCanonical: boolean;
  status:
    | 'draft'
    | 'review_ready'
    | 'fairness_reviewed'
    | 'approved'
    | 'scheduled'
    | 'published'
    | 'archived'
    | 'hotfix_pending'
    | 'corrected_exception';
}

export interface RuntimeWeeklyResolutionPackage {
  id: string;
  kind: 'weekly_resolution';
  weekId: string;
  publishWindow: { startsAtUtc: string; endsAtUtc: string };
  unlockThreshold: 4;
  contentVersion: string;
  answer: string;
  title: string;
  caseFrame: string;
  clueText: string;
  hintData: RuntimeHintData;
  dictionaryVersion: string;
  feedbackAlgorithmVersion: string;
  difficulty: { tier: 'gentle' | 'standard' | 'challenging'; flags: string[] };
  isCanonical: boolean;
  status: RuntimeDailyCasePackage['status'];
}
```

### 5.2 Runtime validator interfaces

```ts
export interface RuntimeValidationResult {
  ok: boolean;
  errors: Array<{
    code:
      | 'schema_invalid'
      | 'hash_mismatch'
      | 'version_pin_mismatch'
      | 'hint_policy_invalid'
      | 'publish_window_invalid'
      | 'canonical_status_invalid';
    message: string;
    field_path?: string;
  }>;
}

export interface ContentPackageRuntimeValidator {
  validateDailyCasePackage(pkg: unknown): RuntimeValidationResult;
  validateWeeklyResolutionPackage(pkg: unknown): RuntimeValidationResult;
}
```

Required runtime behavior:
- reject activation when validation fails.
- preserve last-known-good package or show unavailable state.
- do not silently mutate active pinned sessions due to newly fetched content.


### 5.3 Runtime validation snapshot lookup interface

```ts
export interface ValidationSnapshotLookup {
  snapshot_version: string;
  hasWord(normalizedGuess: string): boolean;
}

export interface ValidationSnapshotLookupProvider {
  get(snapshot_version: string): ValidationSnapshotLookup;
}
```

Required runtime behavior:
- local guess validation must resolve against a pre-hydrated in-memory lookup for the active pinned snapshot.
- do not parse validation snapshot files per guess.
- do not query SQLite or other storage per guess for ordinary lookup paths.

Preferred v1 implementation:
- hydrate generated validation snapshot data once into process-global in-memory lookup structures inside `packages/validation`.
- reuse those structures for all active lookups.
- avoid introducing Trie/SQLite/per-guess query complexity unless profiling shows the simple approach is insufficient.

---

## 6. Contract Usage Guidance

- `packages/game-rules` and `packages/validation` should own these contract exports where practical.
- `apps/mobile` should consume contracts and avoid redefining parallel shape types.
- tests for transition legality, persistence serialization, analytics payload safety, and runtime content validation should assert against this contract file.
