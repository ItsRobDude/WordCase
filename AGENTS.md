# AGENTS.md

This file is the operating guide for AI contributors working in the WordCase repository.

WordCase is an Android-first mystery word game built around daily case files, coded transmissions, clue deduction, and weekly investigations.
It is being built as a serious product, not as a loose prototype pile.
The game must remain fair, readable, stylish, restrained, and maintainable.

This file is intentionally compact.
Read this first, then read only the additional docs relevant to the task you are working on.

If a referenced doc does not exist yet, that does **not** grant permission to invent major new product behavior in code.
Use `README.md`, this file, and the most conservative safe assumptions that fit WordCase's current direction.
Document those assumptions clearly.

---

## 1. WordCase in One Minute

WordCase is:

- an Android-first, portrait-first mystery word game
- centered on a **Daily Case** as the core player ritual
- deepened by a **Weekly Caseboard** that turns multiple days into a larger payoff
- designed for short, meaningful sessions with fast startup and reliable resume
- built on fairness, trust, clarity, and stylish restraint
- spoiler-safe in its social and sharing behavior
- respectful of the player's thinking time
- intended to feel premium even before any premium purchase exists
- TypeScript-first and conservative in its technical choices
- documentation-first so humans and AI contributors stay aligned

WordCase should feel like:
- intercepting something
- deciphering something
- narrowing possibilities
- solving a case
- building a daily habit that feels smart and satisfying

WordCase should **not** feel like:
- a noisy casino
- an ad trap
- a clone with a dark skin on top
- a generic level grinder
- a content treadmill that hides weak fundamentals under volume

---

## 2. Source of Truth and What to Read

### Source-of-truth order
When working in WordCase, follow this explicit precedence order:

1. focused docs for the touched area
2. engineering standards/process docs
3. AGENTS execution rules
4. code

Important rules:
- the code should reflect the docs
- product behavior should not be silently reinvented inside the code
- if code and docs disagree, fix the disagreement deliberately
- do not assume current code is automatically the product truth
- if a focused doc goes deeper than `README.md`, the focused doc wins for that area

### Required conflict-template when docs disagree
If two docs (or two sections) conflict, contributors must include this short template in the PR description or final report:

- **Conflicting docs/sections:** `<doc path + section>` vs `<doc path + section>`
- **Chosen interpretation:** `<the interpretation followed in this change>`
- **Why this preserves fairness/trust:** `<short rationale>`
- **Follow-up doc updates needed:** `<specific docs/sections to reconcile>`

### Read only the docs you need
Do **not** read every doc by default.
Use this routing guide to stay focused and avoid context bloat.

Baseline for all code tasks:
- `README.md`
- this file
- `docs/engineering-standards.md`

Validation execution contract:
- run the command contract defined in `docs/engineering-standards.md` section **"5.1 Operational Validation Commands (Contributor Contract)"** so local checks and CI stay aligned.

| If the task involves... | Also read... |
| --- | --- |
| puzzle rules, solve/fail logic, hints, attempts, or case completion | `docs/game-rules.md`, `docs/dictionary-and-validation-rules.md` |
| startup, onboarding, resume, pause, results, rollover, or state transitions | `docs/modes-and-session-state.md`; `docs/save-sync-and-account-rules.md` if canonical result/rollover authority is touched |
| screen layout, tap flow, input placement, navigation, or UI behavior | `docs/screen-behavior-spec.md`, `docs/accessibility-localization-and-device-support.md`; add `docs/audio-visual-style-guide.md` if visuals/motion/sound/haptics are involved |
| word acceptance, answer policy, phrase support, obscurity limits, or edge-case vocabulary | `docs/dictionary-and-validation-rules.md` |
| local saves, cloud sync, guest/account flow, restore, offline behavior, or completion integrity | `docs/save-sync-and-account-rules.md`, `docs/technical-architecture.md` |
| daily publishing, weekly cases, archive packs, review flow, or event scheduling | `docs/content-pipeline-and-liveops.md` |
| hints, rewards, ad behavior, ad-free purchase, cosmetic unlocks, or retention systems | `docs/progression-economy-and-monetization.md` |
| sound, haptics, visual tone, icon style, typography, spacing, or animation behavior | `docs/audio-visual-style-guide.md`, `docs/accessibility-localization-and-device-support.md` |
| analytics, telemetry, experiments, or funnel measurement | `docs/analytics-and-experimentation.md` |
| module boundaries, persistence, content loading, backend scope, or app architecture | `docs/technical-architecture.md` |
| deciding what should be built next | `docs/milestone-implementation-plan.md` |

If a task does not touch one of these areas, do not pull in extra docs.

---

## 3. Current Working Rule

Unless explicitly told otherwise:

- work on the **smallest safe slice** of the current milestone
- do not widen scope because a future feature seems related
- do not build future systems early just because they sound exciting
- do not sneak backend/social/economy complexity into a small vertical-slice task
- do not let UI polish outrun gameplay clarity and correctness

WordCase should be built in controlled layers.

The basic order is:
1. product truth
2. puzzle fairness and state behavior
3. focused playable slice
4. reliable daily/weekly structure
5. careful expansion later

---

## 4. Non-Negotiable Product Invariants

These rules should not drift unless the docs are intentionally updated.

### Core product shape
- The **Daily Case** is the heart of WordCase.
- The **Weekly Caseboard** deepens the daily ritual; it does not replace it.
- Archive and practice content exist to support the core loop, not bury it.
- WordCase is Android-first and portrait-first.
- One-handed usability matters.
- The product must remain understandable to ordinary players within the first minute.

### Fairness and trust rules
- Puzzle feedback must be deterministic and explainable.
- Hints must help honestly and must not lie.
- Accepted and rejected guesses must follow a documented validation policy.
- Published daily answers must **not** silently change after release.
- If a live puzzle truly requires correction, that correction must be deliberate, documented, and treated as exceptional.
- Difficulty should come primarily from deduction, not from obscure vocabulary, hidden rules, or UI friction.
- The player should be able to understand why they won or lost.
- The game must not feel like it cheated.

### Session and UX rules
- Startup should be fast.
- Resume should be fast and reliable.
- Active case state must autosave after meaningful actions.
- Backgrounding the app must not casually destroy progress.
- Guest play should remain allowed unless product docs intentionally change that rule.
- The player must not be forced through account friction before understanding the game's value.
- Notifications, social features, and progression systems should invite return, not harass the player.

### Concentration and presentation rules
- WordCase must respect the player's thinking time.
- Do not interrupt active puzzle-solving with ads.
- Do not use presentation that competes with concentration.
- Readability is more important than spectacle.
- Stylish restraint is preferred over flashy noise.
- Everything important should remain usable with sound off and haptics off.

### Social and sharing rules
- Sharing must be spoiler-safe.
- Social comparison should be lightweight and optional.
- Real-time multiplayer is not part of the core product identity.
- Social layers must not make the core puzzle dependent on coordination.

### Content rules
- Live daily and weekly content must be structured and reviewable.
- Unreviewed generated content should not be published as live canonical content.
- Content should be checked for ambiguity, fairness, and bad phrasing.
- Solved-case history and archive behavior should preserve player trust.

### Monetization rules
- Monetization must support the product, not deform it.
- Do not make the game feel pay-to-solve.
- Do not use manipulative interruption in the active solve flow.
- Canonical Daily/Weekly hints are never purchasable; any future hint economy is non-canonical Archive/Practice only and must not affect streak/evidence/canonical outcomes.
- Ad-free, carefully handled hints, and later premium content packs may fit.
- For hint monetization interpretation, align with the hint section in `docs/progression-economy-and-monetization.md`.
- Any monetization layer must remain secondary to fairness and clarity.

---

## 5. Non-Negotiable Technical Invariants

### Stack direction
- mobile client: TypeScript
- mobile app direction: React Native with Expo
- routing direction: Expo Router
- app/session/UI state direction: Zustand
- important local persistence direction: SQLite via Expo SQLite
- shared logic: TypeScript packages where useful
- backend/API later where needed: TypeScript
- background/scheduled work later where needed: TypeScript worker

### Architecture rules
- core puzzle truth should not be duplicated carelessly across UI, backend, and tools
- validation rules should be centralized where practical
- content data should be structured and typed, not hardcoded into random screens
- UI code should render and orchestrate interaction; it should not become the hidden source of game truth
- the app should not depend on a backend for every moment of core solo play unless docs intentionally require it
- local persistence and reliable resume are first-class responsibilities, not late polish

### Product/tech boundary rules
- do not hardcode live daily answers into presentation components
- do not bury solve/fail semantics inside ad, analytics, or UI helper code
- do not let cloud or social features silently redefine local puzzle behavior
- do not create content formats that only one fragile screen knows how to read

### Dependency rules
- prefer fewer dependencies
- do not add a package unless it clearly saves real work or improves reliability
- avoid flashy or heavy libraries for small problems
- WordCase does not need engine-like complexity for ordinary UI and puzzle behavior

### Data and config rules
- keep configuration and secrets out of source files
- keep case/content identifiers stable once introduced
- use boring, obvious file and folder names
- avoid schema or content-format churn without good reason

---

## 6. Repo Shape

Expected top-level structure:

- `apps/mobile`
- `apps/api`
- `apps/worker`
- `apps/content-tools`
- `packages/game-rules`
- `packages/validation`
- `packages/ui`
- `packages/audio`
- `packages/analytics`
- `packages/utils`
- `docs/`
- `assets/art`
- `assets/audio`
- `assets/fonts`
- `assets/marketing`

Keep names boring and obvious.

If some of these folders do not exist yet:
- do not create speculative sprawl
- create only the smallest structure the current milestone actually needs

---

## 7. Working Rules for Agents

### Scope discipline
- work on the smallest safe slice of the requested task
- do not jump ahead to later milestones unless explicitly told
- do not sneak in account, social, backend, economy, or content-tooling complexity when the task does not need it
- do not expose unfinished modes or fake feature buttons as if they work
- do not widen a gameplay task into a branding task or a branding task into a backend task

### Coding style
- slower but cleaner
- boring is good
- clear is better than clever
- consistency beats personal style
- prefer small focused files
- avoid giant god-files and tangled helpers
- avoid magic behavior hidden behind clever abstractions

### Naming
Use descriptive names like:
- `dailyCaseState`
- `caseResultScreen`
- `guessValidationResult`
- `weeklyCaseboardProgress`
- `caseContentLoader`
- `resumeSessionService`

Avoid:
- cute names
- vague names
- clever shorthand
- unexplained acronyms
- naming that only makes sense after reading the implementation

### Comments
Comment:
- why something exists
- why a rule matters
- why a non-obvious implementation choice was made
- why a workaround is necessary
- why a fairness or content constraint is important

Do not comment obvious code line by line.

### UI and interaction discipline
- prefer readability and obvious interaction over novelty
- avoid tiny tap targets and clutter-heavy layouts
- do not add modal spam
- keep the main case screen focused
- when in doubt, make the repeated daily flow faster and clearer

### Audio/visual discipline
- if a task touches visuals or sound, follow the style guide
- do not generate or implement wildly inconsistent styles across surfaces
- in-app readability matters more than decorative detail
- a polished quiet interaction is better than a loud flashy one

### Analytics discipline
- instrument intentionally
- do not spray events everywhere “just in case”
- analytics should help answer product questions, not create noise
- be privacy-conscious and avoid needless payload bloat
- do not let analytics logic become entangled with core game truth

### Content/data discipline
- keep puzzle content separate from UI rendering concerns
- prefer typed content structures
- do not scatter case logic across many unrelated files
- do not encode answer-policy assumptions in multiple places
- if content behavior is unclear, push toward a more explicit schema rather than more special cases

---

## 8. Package Manager and Commands

Use **pnpm only**.

Do not mix:
- npm workspace commands
- yarn
- bun
- extra monorepo tools unless explicitly approved

Before assuming a command exists, check the repo scripts.
If you introduce a new important command, document it.

---

## 9. Milestone Discipline

Before implementing anything beyond trivial tooling, check:
- `docs/milestone-implementation-plan.md`

Rules:
- do not build content-operations complexity before the core puzzle is trustworthy
- do not build backend/cloud dependency before the local playable core is solid
- do not build heavy social systems before spoiler-safe sharing and basic comparison are justified
- do not let monetization design outrun proof that the game is genuinely fun
- do not let event systems outrun a strong daily and weekly loop
- do not let art polish outrun readability and usability

If unsure whether something belongs in the current phase, choose the more conservative implementation.

---

## 10. Content and Puzzle Discipline

WordCase depends heavily on trust, so treat content and puzzle behavior as high-risk product truth.

### Rules
- published cases should have stable identifiers
- solve/fail behavior must be explicit and testable
- attempt limits, hint rules, and feedback rules must not drift silently
- dictionary and validation policy should remain centralized and documented
- share outputs must not reveal spoilers or sensitive unrevealed state
- if generation tools are used for content assistance, their outputs must still be reviewed before canonical use
- archive behavior should preserve the meaning of past cases

### Practical principle
A word game can become untrustworthy much faster than it becomes impressive.
Protect trust first.

---

## 11. Before You Edit

Before making changes:

1. summarize the task in a few bullets
2. list the files and docs you actually need
3. state any assumptions you are making
4. keep the planned change as small as possible
5. if the task is large, choose a smaller first pass instead of a giant hero implementation

If a referenced doc does not exist yet:
- do not invent a giant replacement framework
- use README + this file + the smallest conservative assumption set
- note the gap clearly in your final report

---

## 12. Before You Finish

Before finishing:

1. run the relevant checks for the code you touched
2. test the specific flow you changed
   - for no-code/docs-only edits, run docs consistency checks only (for example: markdown lint, link checks, spell checks) when available
3. fix obvious issues instead of leaving them behind
4. verify docs and scripts still match reality
5. make sure you did not accidentally introduce unrelated complexity
6. make sure the result still matches WordCase's product direction
7. if you changed puzzle behavior, validation behavior, session state, or monetization behavior, call that out explicitly

Check execution source of truth:
- use `docs/engineering-standards.md` section **"5.1 Operational Validation Commands (Contributor Contract)"** for exact commands, milestone gates, package-vs-repo usage, and CI parity expectations.

Your final report should include:
- what changed
- assumptions made
- commands run
- anything intentionally deferred
- remaining risks or cleanup items

---

## 13. Forbidden Moves

Do not:
- invent undocumented puzzle rules
- hide validation truth inside UI-only code
- hardcode live answers into presentation components
- silently change published daily answers
- introduce ads or monetization interruptions during active solving
- make the player pay to preserve basic fairness or dignity
- publish unreviewed generated content as live canonical content
- add heavy dependencies without clear justification
- duplicate puzzle truth across app, packages, and backend casually
- expose unfinished modes as if they work
- add auth or online requirements to core solo play without explicit product direction
- add spoiler leaks to share cards, notifications, or comparison surfaces
- optimize for flashy presentation at the expense of readability
- let analytics, ads, or experiments quietly redefine game semantics
- create a bloated progression layer that buries the puzzle
- treat current code accidents as permission to drift from the documented product

---

## 14. If You Are Unsure

If WordCase docs do not fully answer something:
- choose the most conservative, maintainable option
- protect fairness and readability first
- preserve reliable save/resume behavior
- keep the implementation small
- document the assumption clearly
- do not improvise a large new pattern

When in doubt, WordCase prefers:
- clarity
- fairness
- determinism
- readability
- maintainability
- player trust

over novelty, speed, scope creep, or cleverness.
