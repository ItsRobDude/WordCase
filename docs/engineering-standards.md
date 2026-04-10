# WordCase Engineering Standards

This document defines how WordCase code should be written, organized, reviewed, tested, and maintained.

Its purpose is to keep WordCase understandable and maintainable over time, especially when multiple humans and AI tools contribute to the codebase.

This document is a coding and implementation standard.

If WordCase code "works" but ignores these standards, it should not be considered acceptable without intentional review and approval.

---

## 1. Core Engineering Philosophy

WordCase should be built with these principles:

- slower but cleaner is better than fast and messy
- boring is good
- clear is better than clever
- consistency beats personal style
- maintainability matters more than showing off
- puzzle truth and player trust matter more than convenience
- do not trade long-term sanity for short-term momentum

WordCase is not the place for:
- fragile “smart” code
- hidden state magic
- UI code that quietly becomes the rules engine
- architecture experiments that make future maintenance harder
- AI-generated code that drifts from the docs just because it “kind of works”

---

## 2. Source of Truth Hierarchy

When coding WordCase, contributors should follow this order of truth:

1. WordCase product documents in `docs/`
   - when implementation shape details are needed, use `docs/implementation-contracts.md` as the stable TypeScript-facing contract reference
2. WordCase engineering standards in this document
3. approved milestone/build-order documents
4. code

Important rules:
- the code should reflect the docs
- product behavior should not be silently reinvented inside the code
- if code and docs disagree, fix the disagreement deliberately
- do not assume current code is automatically the product truth

---

## 3. Current Stack Direction

WordCase should stay aligned with the current intended stack direction.

### Current working stack
- mobile app: React Native with Expo
- language: TypeScript
- routing: Expo Router
- app/session/UI state: Zustand
- important local persistence: SQLite via Expo SQLite
- shared rule logic: TypeScript packages

If a later technical architecture document intentionally changes part of this, that document should be updated first and the code should follow the updated source of truth.

---

## 4. Mandatory Tooling

These tools are mandatory unless the engineering docs are intentionally updated later.

### Package management
- **pnpm** only

Do not mix:
- npm workspace commands
- yarn
- bun

### Formatting
- **Prettier** is mandatory

Formatting should be automatic and non-negotiable.
Humans should not waste review time on whitespace fights.

### Linting
- **ESLint** is mandatory
- TypeScript-aware linting is mandatory
- import/order rules should be enforced
- obvious unused code/imports should be caught automatically

### Type checking
- **TypeScript compiler checks** are mandatory

Code that passes visually but fails type safety is not done.

### Testing
Mandatory test tools:
- **Vitest** for shared logic and unit tests
- **React Native Testing Library** for app/component behavior where needed
- **Jest with `jest-expo`** for Expo/React Native test environment support where needed

### Build / app validation
Mandatory build/app validation tools:
- **Expo CLI**
- **Expo Doctor**

The project must maintain a reliable way to validate that the mobile app still bundles and remains sane for Android.

### CI
- **GitHub Actions** should be the standard CI path

---

## 5. What Lint / Format / Test / Build Tools Are Mandatory?

### Mandatory tools summary
- `pnpm`
- `prettier`
- `eslint`
- `typescript`
- `vitest`
- `@testing-library/react-native`
- `jest-expo`
- `expo` CLI
- `expo-doctor`
- GitHub Actions CI

### Practical rule
If a proposed tool does not clearly improve:
- trust
- maintainability
- Android reliability
- or testing quality

it should not be added.

---

## 5.1 Operational Validation Commands (Contributor Contract)

This section is the single operational contract for contributor validation.
Use these exact commands so local validation, milestone gates, and CI all run the same path.

Reference points:
- `AGENTS.md` (execution expectations for contributors)
- `docs/milestone-implementation-plan.md` (milestone-required checks)

### Canonical command names
The repo/workspace must expose these command names:
- `format`
- `lint`
- `typecheck`
- `test`
- `build`
- `check` (aggregated validation: `lint` + `typecheck` + `test` + `build`)

**Requirement vs Enforcement State:**
A requirement is normative as soon as it appears in this document. Lack of automation does not make the requirement optional; it only changes whether enforcement is currently manual or automated.

| Requirement | Canonical command | Required since milestone | Current enforcement | Blocking now |
| :--- | :--- | :--- | :--- | :--- |
| Code Formatting | `pnpm format` | M0 | Manual / Unavailable | No |
| Linting | `pnpm lint` | M0 | Manual / Unavailable | No |
| Typechecking | `pnpm typecheck` | M0 | Manual / Unavailable | No |
| Testing | `pnpm test` | M0 | Manual / Unavailable | No |
| Building | `pnpm build` | M0 | Manual / Unavailable | No |
| All checks | `pnpm check` | M0 | Manual / Unavailable | No |

*(Note: During the docs-only phase, enforcement is primarily manual. Automated scripts will become blocking CI gates once workspace scaffolding is introduced.)*

### Repo-wide usage (default before every commit)
Run from repo root:
- `pnpm format`
- `pnpm lint`
- `pnpm typecheck`
- `pnpm test`
- `pnpm build`

Use this shortcut when available:
- `pnpm check`

### Package-scoped usage (during iteration)
For faster local loops, run checks only for touched package(s):
- `pnpm --filter <package_name> format`
- `pnpm --filter <package_name> lint`
- `pnpm --filter <package_name> typecheck`
- `pnpm --filter <package_name> test`
- `pnpm --filter <package_name> build`

Before merge, always run repo-wide checks from root (or the CI-equivalent command path).

### When to run each command
- `format`: after edits and before opening/merging PRs.
- `lint`: on every feature/fix/doc-with-code update before commit.
- `typecheck`: on every TypeScript change before commit.
- `test`: whenever behavior can change (rules, state, persistence, routing, UI logic).
- `build`: before merge and for milestone completion validation.
- `check`: preferred pre-commit/pre-PR command when available.

### Milestone-specific required checks
- **Milestone 0 and onward:** `format`, `lint`, `typecheck`, `test`, and `build` must pass.
- **Milestones that touch Expo/mobile runtime paths:** run `expo-doctor` and a successful Expo bundle/start validation in addition to the core checks.
- **Milestones that change puzzle truth, validation policy, or canonical result logic:** require targeted tests covering the changed rule path plus passing repo-wide checks.

### Expected CI parity
CI must execute the same command contract, in this order:
1. `pnpm format --check` (or equivalent non-mutating formatter check)
2. `pnpm lint`
3. `pnpm typecheck`
4. `pnpm test`
5. `pnpm build`

Do not create a CI-only validation path that differs from contributor-local commands.

---

## 6. Architecture Boundary Rules

WordCase should enforce strong separation between:

- UI/app shell
- feature screens/components
- persistence/services
- shared puzzle truth
- validation truth
- analytics/logging side effects

### Non-negotiable rules
- UI must not become the hidden source of puzzle truth
- solve/fail/streak/evidence logic must not be duplicated across many layers
- analytics, logs, experiments, and ads must never define puzzle semantics
- persistence stores truth; it does not invent truth
- content packages and validation snapshots must be versioned and pinned
- active sessions must not be silently mutated by unrelated live updates

### Practical rule
If the app screen, a helper, and a shared package all contain versions of the same gameplay rule, that is already a warning sign.

---

## 7. Dependency Standards

WordCase should be strict about adding libraries.

### Rules for adding a dependency
A dependency should only be added if it clearly provides real value such as:
- saving significant implementation effort
- improving reliability
- solving a hard problem better than in-house code
- reducing long-term maintenance burden
- fitting naturally into the Expo / TypeScript / Android-first stack

### WordCase should avoid
- flashy libraries for convenience only
- heavy state-management or architectural frameworks beyond what is needed
- duplicate libraries that solve the same job
- native modules that increase build/setup pain without a clear payoff
- packages that make AI-assisted maintenance harder

### Preferred rule
Prefer first-party Expo-compatible choices where they are good enough.

### Special caution
Any package that touches:
- persistence
- routing
- content loading
- sync
- analytics
- logging
- test infrastructure

should be treated as a serious long-term decision.

---

## 8. Naming Standards

WordCase should use clear, boring, descriptive names.

### Preferred naming style
Use names like:
- `dailyCaseState`
- `weeklyCaseboardProgress`
- `canonicalResultRecord`
- `hintSelectionResult`
- `validationSnapshot`
- `resumeSessionService`

Avoid names that are:
- cute
- vague
- overly shortened
- inside-jokes
- clever for the sake of cleverness

### File and symbol naming rule
A developer should be able to guess what a file/class/function does from its name without opening it.

If a name needs explanation, it is probably not a good WordCase name.

---

## 9. Comments and Documentation in Code

WordCase should use comments carefully.

### Comment rules
Comments should explain:
- why something exists
- why a gameplay rule matters
- why a non-obvious implementation choice was made
- why a workaround is necessary
- why a fairness or sync constraint exists

Comments should not:
- narrate obvious code line by line
- repeat the code in English
- become stale and misleading

### Comments are especially important around
- feedback evaluation
- duplicate-letter handling
- hint behavior
- canonical result precedence
- streak/evidence derivation
- rollover logic
- save/sync conflict rules
- content/version pinning
- device-local versus canonical truth

---

## 10. Logging Standards

WordCase should keep useful logs without leaking puzzle truth or player data carelessly.

### WordCase should log
- technical errors
- important content-loading failures
- validation snapshot mismatch problems
- sync failures/retries
- canonical conflict-resolution decisions where appropriate
- important state transition failures

### WordCase should avoid logging by default
- live unpublished answers
- raw live answer words in production logs
- large lexicon dumps
- full player guess histories unless explicitly needed for controlled debugging
- raw sensitive account/session payloads
- noisy logs that drown real problems

### Logging principle
Logs should help diagnose problems without:
- spoiling content
- leaking player text
- or turning the app into a privacy mess

---

## 11. Error Handling Standards

WordCase should handle errors in a player-friendly and engineering-usable way.

### User-facing behavior
Players should see:
- clear readable messages
- calm failure states
- practical next-step guidance when possible

### Technical behavior
Technical detail belongs in logs, not in normal player UI.

### WordCase should avoid
- raw exception dumps in the app
- vague “something went wrong” messages with no clue what to do
- silent state corruption
- resetting puzzle state just because a non-critical side effect failed

### Important rule
If analytics fails, gameplay continues.
If logging fails, gameplay continues.
If content validation fails, keep last-known-good content or show a clean unavailable state.
Do not invent puzzle truth to “smooth over” a failure.

---

## 12. Mandatory Tests by Rule Area

These tests are required for the most important gameplay-truth systems.

### Feedback logic
Required tests:
- exact-position match behavior
- present-elsewhere behavior
- ruled-out behavior
- mixed feedback in one guess
- deterministic same-input same-output behavior
- version-pinned feedback behavior for published content

### Duplicate letters
Required tests:
- guess contains more copies than answer
- answer contains more copies than guess
- correct-position matches consume occurrences first
- remaining present-elsewhere matches consume remaining occurrences correctly
- extra unmatched duplicates are ruled out
- repeated-letter evaluation is identical across all shared truth paths

### Streak logic
Required tests:
- solved unassisted increments streak
- solved assisted increments streak
- failed daily breaks streak
- missed daily breaks streak
- archive/practice results do not extend streak
- weekly resolution does not incorrectly extend or break daily streak
- streak derivation from canonical records is stable after recomputation

### Rollover logic
Required tests:
- active-session grace behavior
- no invalid extra canonical completion after leaving and returning post-rollover, if that remains the final rule
- stale device time does not create extra daily opportunities
- new daily becomes current default after prior canonical state resolves
- result state is preserved correctly through rollover
- prior live daily does not remain endlessly canonically open by accident

### Hint behavior
Required tests:
- hint unavailable before required first valid guess
- hint available after valid threshold
- one-hint maximum enforced
- hint does not consume an attempt
- hint selection is deterministic
- hint result correctly marks assisted solve
- hint never alters previous guess feedback
- hint never changes answer truth

### Result recording
Required tests:
- solved unassisted canonical record creation
- solved assisted canonical record creation
- failed canonical record creation
- canonical precedence resolution for conflicting outcomes
- weekly evidence derivation from canonical results
- result acknowledgment stays separate from canonical result truth
- offline local result can persist and later sync without rewriting truth incorrectly

---

## 13. What Tests Are Required for the Critical Gameplay Systems?

At minimum, the following are mandatory before a related milestone is considered trustworthy:

- feedback logic unit tests
- duplicate-letter unit tests
- streak derivation unit tests
- rollover unit tests
- hint behavior unit tests
- canonical result-recording unit tests
- session serialization / restore tests
- startup/resume integration tests for the daily flow
- content schema validation tests if content-loading behavior changes

If a change touches any of those systems and no new/updated tests were added where needed, the change is not complete.

---

## 14. Testing Standards

WordCase should treat testing as essential from the start, but should focus effort where trust lives.

### Highest-priority automated test areas
- shared feedback engine
- validation engine
- hint logic
- canonical result logic
- streak derivation
- weekly evidence derivation
- rollover and resume behavior
- content/package validation
- save/sync conflict rules

### Lower-priority early areas
- minor cosmetic animation behavior
- non-critical visual polish details
- purely decorative UI states

### Manual testing is still required
Automated tests are not enough.
Real Android manual testing is required for:
- onboarding clarity
- resume reliability
- one-handed usability
- sound-off usability
- haptics-off usability
- reduced motion / larger text sanity
- visible result clarity
- no fake unfinished screens

---

## 15. CI Standards

WordCase should use CI to enforce boring correctness before merge.

### What CI checks must pass before merge?

For code changes, these checks are mandatory:
- format check passes
- lint passes
- typecheck passes
- all relevant tests pass
- app sanity/build validation passes
- content/schema validation passes if content or lexicon files changed
- Expo Doctor passes if app dependencies or config changed

### Minimum required CI gates
- `format:check`
- `lint`
- `typecheck`
- `test`
- `build:check`
- `content:validate` when relevant
- `expo:doctor` when relevant

The exact script names can vary, but the behaviors above are mandatory.

### Docs-only PRs
Docs-only PRs do not need full app build validation, but they should still avoid breaking doc references or source-of-truth consistency.

---

## 16. File and Module Structure Standards

Every WordCase file should have a clear reason to exist.

### Rules
- each file should have a focused purpose
- each module should own a clearly defined area
- do not mix UI rendering, persistence, and game truth in one file
- avoid giant god-files and god-services
- keep shared rule logic in shared packages
- keep app-layer code focused on rendering, orchestration, and local device behavior

### Preferred organization behavior
The codebase should stay understandable from the product point of view, not just from technical convenience.

---

## 17. Feature Flags and Unfinished Work

WordCase should never leave confusing half-built features exposed casually.

### Rules
- unfinished features should be hidden or cleanly disabled
- do not expose dead-end buttons
- do not ship fake Weekly Caseboard, Archive, Social, Event, or Store surfaces
- if a feature is incomplete, it should remain clearly non-player-facing

The app should feel intentional, even while still under development.

---

## 18. Acceptable AI-Generated Code

AI-generated code is allowed, but only under strict standards.

### What counts as acceptable AI-generated code in this repo?

Acceptable AI-generated code must:
- follow WordCase docs and source-of-truth behavior
- keep puzzle truth out of random UI/helpers
- use boring, descriptive naming
- avoid speculative abstractions
- avoid dependency creep
- include or update tests when changing gameplay-truth behavior
- clearly separate canonical truth from device-local UI state
- not expose fake unfinished features
- not invent product behavior where the docs are explicit

### AI-generated code is not acceptable if it
- silently changes solve/fail semantics
- silently changes validation behavior
- duplicates rule logic across multiple layers
- hardcodes live answers into presentation components
- adds libraries without clear justification
- introduces cleverness that makes future maintenance harder
- skips tests for gameplay-truth changes
- treats “it ran once” as proof that the implementation is good

### Human review rule
All AI-generated code that touches any of these must be reviewed carefully:
- feedback logic
- validation rules
- hint behavior
- result recording
- streak logic
- rollover logic
- save/sync logic
- content loading/version pinning
- dependency changes
- app architecture

### Special rule for AI-generated content/tooling
AI may assist with:
- content suggestions
- schema scaffolding
- tooling scaffolding
- review checklists

AI must not be the final authority for:
- daily answer quality
- fairness sign-off
- live canonical content approval

---

## 19. Code Review Standards

Reviews should focus on substance, not style nitpicks already enforced automatically.

### Reviewers should check
- does this match the docs?
- does this preserve puzzle truth and player trust?
- does this create hidden behavior?
- is this simpler than the alternatives?
- is naming clear?
- is the dependency really justified?
- are required tests present?
- does this increase Android app fragility unnecessarily?
- does this create future sync or state confusion?

### Review principle
A change should not be approved just because it technically passes if it makes the codebase harder to understand later.

---

## 20. Merge Standards

A PR is not ready to merge unless:
- CI passes
- required tests exist and pass
- changed docs and code still agree
- no obvious scope creep remains in the PR
- no fake unfinished surface is being exposed
- any AI-generated logic in critical paths has been reviewed carefully

If a PR changes gameplay truth but does not mention that explicitly in the summary, it is not ready.

---

## 21. Practical Summary

WordCase engineering should follow this practical standard:

- slower but cleaner
- TypeScript-first
- Expo-friendly and Android-realistic
- Prettier + ESLint + TypeScript + tests are mandatory
- Vitest for shared logic
- React Native Testing Library + jest-expo where app behavior needs testing
- SQLite for important local truth
- shared packages own puzzle truth
- UI does not define game semantics
- logs stay useful but do not leak puzzle truth carelessly
- gameplay-critical systems require real tests
- CI must pass before merge
- AI must follow the docs, not improvise them

If WordCase sticks to these standards, the codebase should stay understandable, maintainable, and much harder to accidentally ruin.
