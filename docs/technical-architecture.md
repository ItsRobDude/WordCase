# WordCase Technical Architecture

This document defines the official implementation architecture for WordCase.

Its purpose is to keep engineering decisions explicit, boring, and consistent across app, shared packages, content loading, persistence, and side-effect systems.

This is a source-of-truth architecture document.
When implementation details disagree with this document, this document should be treated as the intended architecture direction until intentionally updated.

---

## 1. Purpose and Scope

This document defines:
- official mobile stack decisions
- official routing/state/persistence choices
- domain ownership boundaries
- implementation contract references for shared TypeScript-facing shapes
- content manifest/package loading and pinning rules
- save/sync assumptions that affect architecture
- side-effect guardrails for analytics/logging/error handling
- intended repo/module layout direction
- milestone 1 minimum architecture expectations

This document does not define:
- puzzle design rules
- lexicon policy details
- screen-by-screen interaction copy
- monetization systems
- speculative backend-heavy architecture

Those belong in focused product documents.

Implementation contract source for concrete TypeScript-facing shapes:
- `docs/implementation-contracts.md` (canonical state machine contracts, SQLite entity contracts, analytics payload contracts, and runtime content validation interfaces)

---

## 2. Core Architecture Principles

WordCase architecture should be:
- Android-first
- Expo/React Native-first
- TypeScript-first
- daily-first
- fairness-first
- deterministic
- reliable for resume and offline play
- maintainable by a small team

WordCase should keep puzzle truth outside the UI layer.
The app layer should orchestrate and render; shared core modules should decide puzzle truth.

Boring is good.

---

## 3. Official Stack Decisions

### 3.1 Mobile app direction
WordCase uses React Native with Expo for the mobile client.

### 3.2 Language direction
WordCase uses TypeScript across app and shared modules.

### 3.3 Routing decision
WordCase uses Expo Router.

Routing rules:
- route structure should stay simple and explicit
- do not use advanced router tricks unless a later document explicitly allows them

### 3.4 State model decision
WordCase uses Zustand for app/session/UI state.

React Context is allowed only for small, stable cross-cutting providers.
WordCase does not use Redux.

### 3.5 Persistence decision for milestone 1
WordCase uses SQLite via `expo-sqlite` for important app data.

Do not use an MMKV/AsyncStorage hybrid as the main persistence architecture.

If secrets or tokens are required later, a dedicated secure store may be used for those secrets only.
Game/session/progress/content data remains in SQLite.

---

## 4. Domain Ownership Boundaries

### 4.1 Puzzle truth ownership
Puzzle truth must not live in the app layer.

Ownership:
- `packages/game-rules` owns gameplay/state-transition/progression truth
- `packages/validation` owns lexicon and letter-evaluation truth
- UI code renders and orchestrates; UI code does not define puzzle semantics

### 4.2 Hint-selection ownership
Hint-selection logic lives in `packages/game-rules` inside the shared puzzle engine.

Rules:
- UI must not decide which letter to reveal
- hint behavior must be deterministic from pinned puzzle truth plus current puzzle state
- for v1, `hintData` is treated as a validated content contract (not a tuning surface), and out-of-policy values must be rejected before activation

### 4.3 Progression ownership
These belong to `packages/game-rules/src/progression`:
- canonical result recording rules
- streak calculation
- weekly evidence calculation

This belongs to app layer only and remains device-local:
- result acknowledgment state

---

## 5. Content Manifest and Package Model

WordCase uses a hybrid content model:
- bundled bootstrap content for starter/practice basics
- server-published manifest for live daily/weekly content
- local cached manifest and content packages stored in SQLite

### 5.1 Session pinning requirements
Each active session must pin:
- puzzle ID
- content version
- dictionary version
- feedback algorithm version

### 5.2 Activation and validation rules
Before activation, manifest/package data must be:
- schema-validated
- hash/version-checked

A live manifest update must never silently mutate an active pinned session.

If validation fails:
- keep last-known-good content, or
- show unavailable state

Never invent puzzle truth.


### 5.3 Local validation performance contract
Local guess validation must use a pre-hydrated lookup structure keyed to the pinned validation snapshot for the active session.

The app must not:
- parse snapshot files on each guess submission
- perform ad hoc storage queries on each guess submission
- rehydrate validation lookup state for every guess attempt

---

## 6. Save/Sync Assumptions that Shape Architecture

WordCase architecture must enforce these canonical assumptions:
- canonical daily identity and result eligibility are determined by server-published puzzle IDs and validity windows
- server-defined UTC validity windows are authoritative
- local device time is not canonical truth
- wrong/manual device clock must never unlock extra canonical dailies
- offline canonical completion is allowed only if required Daily Case package and matching validation snapshot were already cached locally
- streak is derived from canonical daily results
- weekly evidence is derived from canonical daily results plus weekly resolution state
- result acknowledgment is primarily device-local UI state

These assumptions define storage, pinning, sync reconciliation, and conflict resolution boundaries.

---

## 7. Side-Effect Discipline and Failure Guardrails

WordCase side effects should run in this order:
1. shared core modules compute gameplay truth
2. persistence commits local truth
3. analytics/logging consume emitted domain events after commit

Guardrails:
- analytics/logging/ads/experiments must never define solve/fail/streak/evidence/hint/daily-eligibility semantics
- if analytics or logging fail, gameplay continues unchanged
- if content validation fails, keep last-known-good content or show unavailable state
- if sync fails, keep local canonical records and retry later

---

## 8. Repo and Module Layout Direction

This is intended architecture direction, not a claim that all folders already exist.

```text
apps/mobile
  src/app
  src/features/daily-case
  src/features/weekly-caseboard
  src/features/archive
  src/features/profile
  src/features/results
  src/services/content
  src/services/persistence
  src/services/sync

packages/game-rules
  src/case-engine
  src/progression
  src/session

packages/validation
  src/lexicon
  src/evaluation
  src/generated
```

---

## 9. Milestone 1 Minimum Architecture Expectations

Milestone 1 should include:
- Expo Router navigation with explicit, predictable routes
- Zustand-driven app/session/UI state
- SQLite-backed persistence for puzzle/session/progress/content data
- shared puzzle engine in `packages/game-rules`
- shared validation engine in `packages/validation`
- pinned session content identity and validation-version fields
- deterministic offline resume for already-cached required daily content
- side-effect ordering that cannot redefine gameplay truth

Milestone 1 should not depend on a cloud-first architecture for the core solo loop.

---

## 10. Non-Goals for Now

WordCase deliberately avoids these architecture directions for now:
- Redux as primary app state architecture
- MMKV/AsyncStorage hybrid as primary persistence architecture
- UI-owned puzzle semantics
- unpinned live-content mutation of active sessions
- backend-heavy dependency for each moment of core solo play
- analytics/experiments/ads defining game truth
- complex router patterns that obscure flow ownership

---

## 11. Summary Rule

WordCase architecture should keep canonical puzzle truth in shared TypeScript core modules, persist important local truth in SQLite, and let the app layer orchestrate UI without redefining gameplay semantics.

When in doubt, prefer deterministic fairness, reliable resume/offline behavior, simple module boundaries, and boring implementation choices.
