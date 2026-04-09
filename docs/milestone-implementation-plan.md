# WordCase Milestone Implementation Plan

This document defines the recommended order for building WordCase.

Its purpose is to turn WordCase's product vision, gameplay rules, screen behavior, validation rules, and technical direction into a practical implementation sequence.

This is the build-order source of truth.

It should answer:
- what gets built first
- what is intentionally delayed
- what the first real vertical slice includes
- what must exist before a milestone starts
- what “done” means for each milestone
- what tests and manual checks are required before calling a milestone complete

This document is intentionally biased toward:
- product truth
- player trust
- maintainability
- real Android usability
- controlled expansion

over flashy breadth or premature live-service complexity.

---

## 1. Core Build Philosophy

WordCase should be built in a controlled sequence.

Important rules:
- do not try to build the whole game at once
- do not let content breadth outrun puzzle trust
- do not let UI polish outrun screen clarity and state behavior
- do not let account/cloud complexity outrun a strong local single-device core
- do not build social, monetization, or live-ops complexity before the daily ritual is genuinely fun
- do not expose fake unfinished systems as if they are real

WordCase should aim to become:
1. understandable
2. fun
3. fair
4. stable
5. habit-forming
6. broader later

That order matters.

---

## 2. Current Source-of-Truth Docs

Before serious feature work begins, WordCase should have a usable source-of-truth doc set.

### Already required for the core product shape
- `README.md`
- `AGENTS.md`
- `docs/game-rules.md`
- `docs/dictionary-and-validation-rules.md`
- `docs/modes-and-session-state.md`
- `docs/screen-behavior-spec.md`
- `docs/milestone-implementation-plan.md`

### Required before deeper implementation layers begin
These docs may be created just before the milestone that needs them, but they must exist before serious work starts in that area:
- `docs/technical-architecture.md`
- `docs/engineering-standards.md`
- `docs/save-sync-and-account-rules.md`
- `docs/content-pipeline-and-liveops.md`
- `docs/audio-visual-style-guide.md`
- `docs/accessibility-localization-and-device-support.md`
- `docs/progression-economy-and-monetization.md`
- `docs/analytics-and-experimentation.md` (authoritative analytics taxonomy, required fields, privacy boundaries, and experiment guardrails)

If a milestone depends on a missing doc, create or finalize that doc before serious coding starts for that milestone.

---

## 3. Build Strategy Summary

WordCase should be built in this order:

1. project/repo foundation
2. core vertical slice of the daily experience
3. real daily content system
4. real weekly caseboard and resolution loop
5. account/sync and cross-device trust
6. social/retention expansion and content operations hardening
7. monetization and content expansion
8. beta hardening and release readiness

The daily ritual is the product.
Everything else should support that.

---

## 4. What Should Not Be Built Early

The following should be delayed until the core daily and weekly loops are strong:
- real-time multiplayer
- chat-heavy social systems
- broad event systems
- premium store surfaces
- battle pass or pass-track systems
- multiple currencies
- complex avatar/collection ecosystems
- web-first versions
- user-generated canonical daily content
- heavy narrative scenes between ordinary dailies
- elaborate backend requirements for core solo play

WordCase should first become excellent at:
- one daily case
- one trustworthy solve/fail loop
- one clean result moment
- one reliable resume model
- one clear weekly payoff later

---

## 5. Milestone 0 — Project Foundation

### Goal
Prepare the repo and app foundation so future work stays clean, boring, and maintainable.

### Scope
- establish monorepo/app/package structure
- establish pnpm-only commands and workspace shape
- establish formatting/lint/typecheck/test/build expectations
- establish Expo app shell and route shell
- establish TypeScript-first package boundaries
- establish SQLite persistence direction and basic plumbing
- establish bundled-content loading direction
- establish environment/config conventions
- establish initial docs and asset structure

### This milestone is not about
- a full playable daily
- real daily publishing
- weekly caseboard gameplay
- account linking
- cloud sync
- monetization

### Docs required before Milestone 0 begins
- `README.md`
- `AGENTS.md`
- `docs/game-rules.md`
- `docs/dictionary-and-validation-rules.md`
- `docs/modes-and-session-state.md`
- `docs/screen-behavior-spec.md`
- `docs/milestone-implementation-plan.md`
- `docs/technical-architecture.md`
- `docs/engineering-standards.md`

### Definition of done
Milestone 0 is done when:
- the repo has an approved boring structure
- the app boots into a shell successfully
- formatting/lint/typecheck/test/build commands exist and run consistently
- SQLite and local content loading direction are real, not just implied
- no one needs to guess where new code should go
- the missing core engineering docs for Milestone 1 are no longer missing

---

## 6. Milestone 1 — Core Vertical Slice

### Goal
Prove that WordCase is fun and trustworthy on one Android device before building the broader daily system.

### Exact scope
Milestone 1 should include:
- startup/title flow
- first-launch starter case
- home screen with one clear primary action
- one fully playable bundled Daily Case-like puzzle experience
- Daily Case screen
- guess submission
- deterministic feedback
- duplicate-letter handling
- invalid guess handling
- duplicate-guess rejection
- one honest optional hint
- solve/fail results screen
- minimal settings
- minimal profile surface
- same-device autosave/resume through background and app termination
- restrained first-pass audio/haptics/visual feedback where practical

### What Milestone 1 should not include
- real date-driven live daily publishing
- cloud account linking
- cross-device sync
- real social systems
- monetization
- event systems
- fake placeholders for later systems

### What exactly is in Milestone 1?
Milestone 1 is a **single-device, local-first, content-bundled vertical slice** that proves:
- the core gameplay loop is fun
- the player understands the product quickly
- the app feels reliable to resume
- the game can solve/fail honestly and clearly
- the product tone works on an actual phone

### Is Weekly Caseboard in Milestone 1 a real playable system or just a shell?
For Milestone 1:
- Weekly Caseboard is **not** a player-facing playable system
- it should not appear as a fake production feature
- if a minimal internal/test stub exists for engineering reasons, keep it hidden from normal player flow

The real playable Weekly Caseboard belongs to Milestone 3.

### Is Archive visible in Milestone 1 or deferred?
Default answer:
- Archive is **deferred** from the first real player-facing Milestone 1 slice

Exception:
- Archive may be visible in Milestone 1 only if it contains real playable content, such as a starter replay and at least one true practice file
- if that content does not exist yet, keep Archive hidden rather than exposing an empty/fake shell

### Docs required before Milestone 1 begins
- `README.md`
- `AGENTS.md`
- `docs/game-rules.md`
- `docs/dictionary-and-validation-rules.md`
- `docs/modes-and-session-state.md`
- `docs/screen-behavior-spec.md`
- `docs/milestone-implementation-plan.md`
- `docs/technical-architecture.md`
- `docs/engineering-standards.md`
- `docs/audio-visual-style-guide.md`
- `docs/accessibility-localization-and-device-support.md`

### Definition of done for the first vertical slice
Milestone 1 is done when:
- a fresh install launches cleanly
- the starter case teaches the loop and produces a fast first win
- the player can reach Home and start the main case flow
- one bundled Daily Case-like puzzle is fully playable from start to result
- guess validation and feedback behave exactly according to the docs
- hint behavior works and is honestly recorded
- solve/fail results are clear and accurate
- the app resumes the active case correctly after backgrounding and process death on the same device
- no fake Weekly Caseboard or fake Archive shell is exposed to players
- the product already feels calm, readable, and daily-first

### Required automated tests before Milestone 1 is complete
At minimum:
- unit tests for input normalization
- unit tests for guess validity and rejection reasons
- unit tests for duplicate-letter evaluation
- unit tests for duplicate-guess rejection
- unit tests for hint eligibility and hint application
- unit tests for solve/fail state transitions
- unit tests for result classification (assisted/unassisted/failed)
- unit tests for session serialization/restoration of an in-progress case
- integration tests for startup routing into starter case, active case, and result state

### Required manual checks before Milestone 1 is complete
At minimum:
- first install on Android feels understandable within the first minute
- starter case teaches the rules without long tutorial text
- active case can be backgrounded and resumed cleanly
- app process kill and reopen restores the same local case state
- invalid guess messages are readable and calm
- duplicate letters behave correctly in visible gameplay
- sound-off play is still fully understandable
- haptics-off play is still fully understandable
- larger text / reduced motion settings do not break the main flow
- one-handed interaction feels reasonable on an ordinary phone
- the app does not feel cluttered or fake

---

## 7. Milestone 2 — Real Daily Case System

### Goal
Replace the bundled single-case vertical slice with a real daily content system that preserves trust, offline resilience, and daily identity.

### Scope
- real daily case identity by puzzle ID
- real published daily content package loading
- local caching of daily content and validation snapshot
- daily availability window behavior
- canonical daily result recording on one device
- daily rollover behavior according to save/sync rules
- streak calculation from canonical records
- spoiler-safe result sharing basics
- archive foundations for past dailies and/or practice content when real

### Milestone 2 answer: Archive visible or deferred?
By Milestone 2:
- Archive should become visible only if it contains real browseable content
- if archive foundations are implemented, they must be truly playable and clearly non-canonical

### Docs required before Milestone 2 begins
- all Milestone 1 docs
- `docs/save-sync-and-account-rules.md`
- `docs/content-pipeline-and-liveops.md`
- `docs/analytics-and-experimentation.md` (authoritative analytics taxonomy, required fields, privacy boundaries, and experiment guardrails)

### Definition of done
Milestone 2 is done when:
- the app can open the real current daily from content packages
- the daily is pinned to content/validation versions correctly
- a player can complete a cached daily offline and retain the result locally for later sync
- streak is derived from canonical daily results rather than free-floating mutable state
- archive/practice surfaces are real if exposed
- daily rollover behavior is trustworthy and no longer guessed

---

## 8. Milestone 3 — Weekly Caseboard and Weekly Resolution

### Goal
Add the real weekly meta-loop that deepens the daily ritual without turning into homework.

### Scope
- real weekly board model
- evidence accumulation from solved dailies
- home weekly progress card
- weekly unlock threshold behavior
- real Weekly Resolution puzzle flow
- weekly resolved/incomplete history states
- result/home routing for weekly resolution

### Is Weekly Caseboard in Milestone 3 real or just a shell?
By Milestone 3:
- Weekly Caseboard must be a **real playable system**, not a shell
- if shown to players, it must actually track evidence and support the intended resolution flow

### Docs required before Milestone 3 begins
- all Milestone 2 docs
- `docs/content-pipeline-and-liveops.md` updated to include Weekly Resolution schema/process
- `docs/screen-behavior-spec.md` aligned with real weekly behavior
- `docs/game-rules.md` aligned with final weekly rules

### Definition of done
Milestone 3 is done when:
- solved dailies update the weekly board correctly
- unlock threshold and weekly availability behavior are correct
- Weekly Resolution is playable, fair, and deterministic
- weekly completion does not corrupt daily truth
- Home and Results reflect weekly progress clearly without clutter

---

## 9. Milestone 4 — Account Linking and Cross-Device Sync

### Goal
Add optional account linking and cross-device trust without weakening the local-first solo core.

### Scope
- guest-to-account linking
- canonical result sync
- cross-device restore
- streak/evidence recomputation from canonical records
- conflict handling for canonical daily results
- result acknowledgment behavior aligned with device-local UI truth
- last-known-good content behavior across devices where relevant

### Docs required before Milestone 4 begins
- all Milestone 3 docs
- `docs/save-sync-and-account-rules.md` finalized, not hand-wavey
- `docs/technical-architecture.md` aligned with final sync ownership boundaries

### Definition of done
Milestone 4 is done when:
- a guest can link an account without losing deserved progress
- two devices converge on canonical daily truth correctly
- stale-device behavior is understandable and recoverable
- derived streak/evidence remain correct after sync
- cross-device weirdness does not rewrite puzzle truth silently

---

## 10. Milestone 5 — Social / Retention Layer and Content Ops Hardening

### Goal
Add lightweight social and stronger content operations only after the daily and weekly core is trusted.

### Scope
- improved spoiler-safe sharing
- friend comparison basics later if ready
- notification rules and quiet reminder behavior
- stronger publish/review pipeline
- internal content tooling refinement
- archive growth and better browseability

### Docs required before Milestone 5 begins
- all Milestone 4 docs
- `docs/content-pipeline-and-liveops.md` finalized enough for regular publishing
- `docs/analytics-and-experimentation.md` (authoritative analytics taxonomy, required fields, privacy boundaries, and experiment guardrails)
- `docs/progression-economy-and-monetization.md` if any retention reward layer is added

### Definition of done
Milestone 5 is done when:
- content operations are repeatable and not fragile
- notifications and social features feel helpful, not spammy
- sharing remains spoiler-safe
- the broader retention layer still supports the daily instead of burying it

---

## 11. Milestone 6 — Monetization and Content Expansion

### Goal
Add only the monetization and expansion layers that fit WordCase’s emotional contract.

### Scope
- ad-free purchase
- carefully constrained additional content packs if justified
- optional cosmetic/theme expansion if justified
- no monetization that interrupts active solving

### Docs required before Milestone 6 begins
- all Milestone 5 docs
- `docs/progression-economy-and-monetization.md`
- `docs/audio-visual-style-guide.md` if cosmetic/theme expansion is real

### Definition of done
Milestone 6 is done when:
- monetization supports the product rather than deforming it
- core daily play remains fair and calm
- content expansion does not create dictionary/content trust debt

---

## 12. Milestone 7 — Beta Hardening and Release Readiness

### Goal
Reduce risk before broader player release.

### Scope
- bug fixing
- performance tuning
- fairness audits
- accessibility hardening
- low-end Android sanity checks
- crash reduction
- analytics sanity checks
- content QA hardening
- polish on the most-used screens

### Docs required before Milestone 7 begins
- all major docs intended for launch-critical behavior should now exist and be internally consistent

At minimum:
- `README.md`
- `AGENTS.md`
- `docs/game-rules.md`
- `docs/dictionary-and-validation-rules.md`
- `docs/modes-and-session-state.md`
- `docs/screen-behavior-spec.md`
- `docs/save-sync-and-account-rules.md`
- `docs/content-pipeline-and-liveops.md`
- `docs/technical-architecture.md`
- `docs/engineering-standards.md`
- `docs/audio-visual-style-guide.md`
- `docs/accessibility-localization-and-device-support.md`
- `docs/analytics-and-experimentation.md` (authoritative analytics taxonomy, required fields, privacy boundaries, and experiment guardrails)
- `docs/milestone-implementation-plan.md`

### Definition of done
Milestone 7 is done when:
- the most-used flows are stable on real Android devices
- major resume/offline/result bugs are not still sitting open
- the product feels fair, understandable, and worth returning to
- the codebase is still boring enough to maintain
- launch confidence comes from real testing, not hope

---

## 13. General Milestone Completion Rules

A milestone is not complete just because one demo worked once.

A milestone is complete only when:
- the intended workflow works reliably
- the docs and implementation agree closely enough to trust them
- player-facing behavior is understandable
- obvious trust/fairness risks have been addressed
- no fake unfinished feature is being used to hide missing real behavior
- the result does not create obvious maintainability debt that should have been prevented immediately

---

## 14. General Test Expectations

Every milestone should include tests appropriate to the area it changes.

### Minimum automated test expectations
- core game truth changes require unit tests
- state transition changes require unit tests and targeted integration tests
- persistence/resume changes require serialization/restore tests
- sync/conflict changes require deterministic merge/conflict tests
- content loading/versioning changes require validation tests

### Minimum manual test expectations
- test the main happy path
- test a plausible failure path
- test background/resume behavior where relevant
- test sound-off and haptics-off usability where relevant
- test one real Android phone flow before calling the milestone done
- if the milestone touches accessibility-sensitive UI, run the corresponding manual accessibility checks

---

## 15. Final Priority Summary

If WordCase has to choose what to protect most during implementation, the priority order should be:

1. player trust
2. puzzle truth
3. resume/offline reliability
4. maintainable structure
5. real-world Android usability
6. retention depth
7. monetization and extras

WordCase should win by becoming a smart, fair, reliable daily ritual first.
The broader content, social, and monetization layers can come later.
