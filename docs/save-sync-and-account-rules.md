# WordCase Save, Sync, and Account Rules

This document defines how WordCase handles canonical daily identity, local save behavior, offline completion, account linking, and cross-device conflict resolution.

Its purpose is to keep canonical puzzle truth fair, deterministic, and trustworthy while still allowing fast local play and interruption-tolerant resume.

This is a product behavior document.
When code and this document disagree, this document should be treated as the intended source of truth for save/sync/account behavior until intentionally updated.

---

## 1. Core Principles

WordCase save and sync behavior should preserve:
- canonical fairness
- deterministic conflict resolution
- daily-first trust
- fast local responsiveness
- calm low-friction account handling

WordCase should not:
- grant extra canonical opportunities through local clock manipulation
- let unsynced counters overwrite canonical puzzle truth
- force players through noisy merge prompts for normal cases

---

## 2. Canonical Daily Authority

### 2.1 Canonical day boundary
Canonical daily identity and result eligibility are determined by server-published puzzle IDs and server-defined validity windows.

Authority rules:
- server time and server-defined UTC validity windows are authoritative
- local device day is not authoritative
- account timezone is not authoritative
- local timezone may be used for display only

### 2.2 Device clock behavior
Wrong or manually changed device clock must never:
- unlock extra canonical dailies
- restore missed cases
- grant extra streak credit

Device time may affect local countdown display only.
Canonical truth is resolved against server-defined puzzle windows and puzzle IDs.

---

## 3. Offline Canonical Play

### 3.1 Offline canonical completion eligibility
Offline canonical completion is allowed only when required content was already cached locally, including:
- the Daily Case package
- the matching validation snapshot

### 3.2 No offline live-daily invention
If fully offline and required content is not cached:
- the app must not invent or guess a new live daily
- canonical play for that missing daily is unavailable until content is available

### 3.3 Offline result sync
Locally completed offline results should sync later.
They receive canonical credit if they remain valid under the conflict rules in this document.

### 3.4 Required local cache for offline daily play
Required local cache includes:
- daily puzzle package
- validation snapshot
- local session state
- local canonical pending progress records
- sync outbox

---

## 4. Guest Linking and Merge Behavior

### 4.1 Default merge behavior
Default guest-to-account linking behavior is automatic smart merge.
It is not a blunt keep-local versus keep-cloud choice for ordinary cases.

### 4.2 Smart merge rules
If cloud is empty:
- upload local canonical records

If both local and cloud have progress:
- merge by puzzle ID and progress type
- derive streak from canonical daily records, not from raw synced counters
- derive weekly evidence from canonical records, not from raw synced counters

Canonical result conflict precedence for the same puzzle ID and version:
1. Solved Unassisted
2. Solved Assisted
3. Failed
4. Missed (including explicit abandon), Unopened, or In Progress

### 4.3 Merge prompt rule
Do not prompt the user for ordinary merge cases.
Only prompt for truly exceptional merge cases that cannot be resolved safely by documented rules.

---

## 5. Cross-Device Conflict Resolution

### 5.1 Today's daily state conflicts
For the same puzzle ID and version:
- concluded canonical result beats unresolved in-progress state
- in-progress beats unopened

If two concluded results conflict:
- resolve using canonical result conflict precedence from Section 4.2

If two unresolved in-progress snapshots conflict:
- use the most recently synced valid snapshot until a concluded result exists

### 5.2 Streak conflicts
Streak is derived from canonical daily results.
Do not sync streak as top-level mutable truth.

### 5.3 Weekly evidence conflicts
Weekly evidence is derived from canonical solved daily results and weekly resolution state.
Do not sync weekly evidence as top-level mutable truth.

Weekly unlock evaluation contract (per `weekId`):
- evaluate unlock on every canonical evidence mutation affecting that `weekId`
- evaluate unlock again at week-close finalization for that `weekId`
- compute `weeklyResolutionUnlocked = (evidenceCount >= unlockThreshold)` from canonical solved daily records only (v1 threshold is 4)

Timestamp authority and tie-breaks:
- server-authoritative canonical write timestamp (`recordedAtServer`) is the ordering authority for conflicting writes
- device-captured timestamps (for example `actionAtDevice`) are diagnostics/order hints only and must not overrule server ordering

Delayed sync after week close:
- if evidence for a historical `weekId` arrives after that week has closed (for example delayed sync from offline carryover resolution), recompute unlock and weekly history state for that same historical `weekId`
- do not reattribute that evidence to the newly active week

Non-relock rule:
- once `weeklyResolutionUnlocked=true` is reached for a `weekId`, later reconciliation must not set it back to `false`
- reconciliation may only preserve or upgrade the historical weekly outcome state (`incomplete` -> `unresolved` -> `resolved`), never downgrade solely because of ordering noise

Canonical weekly history outcome must resolve to exactly one state per `weekId` using the same names as game/session docs:
- `resolved` if any Weekly Resolution run solved before week end
- `unresolved` if Weekly Resolution unlocked but no run solved before week end
- `incomplete` if Weekly Resolution never unlocked before week end

### 5.4 Result acknowledgment conflicts
Result acknowledgment is primarily device-local UI state.
Canonical solve/fail state is shared truth.

A second device may show concluded status and offer View Results without forcing a blocking result flow only because another device already acknowledged it.

### 5.5 Solved-on-one-device while open-on-another
A second device may briefly be stale before sync.
After sync, canonical solved state wins unless the second device also produced a conflicting valid concluded result before syncing.

If the second device is actively in the puzzle when remote completion arrives:
- stop further canonical submission
- show a clear completed-on-another-device message
- provide View Results and Return Home actions

### 5.6 Explicit abandon canonical recording
When a player confirms abandon for a Protected Carryover Daily, the canonical write must record:
- `resultCategory = missed_abandoned`
- `actionSource = explicit_user_abandon`
- `actionAtDevice` (device-captured action timestamp, for diagnostics and ordering hints)
- `recordedAtServer` (server-authoritative canonical timestamp used for tie-breaking and audit)

Reversibility rule:
- explicit abandon is not reversible by UI action
- later sync may still be superseded by a stronger valid concluded result for the same puzzle ID/version under precedence rules

### 5.7 Near-simultaneous abandon/solve conflict rule
Race-condition example:
1. Device A records `missed_abandoned` locally while offline or unsynced.
2. Device B solves the same daily and syncs before Device A reconnects.
3. Device A syncs its pending abandon record afterward.

Expected winner:
- solved result wins (Solved Unassisted/Solved Assisted outrank missed_abandoned)
- canonical stored result remains solved
- Device A must converge to solved state after sync and clear its pending abandon outbox item as superseded

---

## 6. Archive and Practice Sync Scope

### 6.1 In-progress archive/practice state
In-progress archive/practice sessions are device-local only in v1.
Do not sync or merge live in-progress archive/practice board state across devices.

### 6.2 Archive/practice completion records
If archive/practice completion records are synced later:
- keep them clearly non-canonical
- keep them separate from live daily truth
- do not let them alter canonical daily history, streak truth, or weekly canonical evidence

---

## 7. Summary Rule

WordCase save and sync behavior should be boring in the best way:
- canonical server truth for daily identity
- resilient local caching for honest offline play
- deterministic conflict resolution
- low-friction smart merge for normal linking
- strict separation between canonical daily truth and non-canonical archive/practice state

When in doubt, prefer fairness, determinism, and player trust over convenience shortcuts.
