# WordCase — Android-First Mystery Word Game

WordCase is an Android-first mystery word game built around **daily case files, coded transmissions, clue deduction, and weekly investigations**.

The goal is **not** to ship another disposable word clone with a dark skin on top.

The goal is to build a word game that feels:
- sharp
- fair
- stylish
- satisfying in short sessions
- strong enough to become part of a player’s daily routine

WordCase should give players a reason to open the app every day, solve something meaningful, feel smart, and want to come back tomorrow.

This README is the current top-level source of truth for WordCase product direction.
Focused documents under `docs/` exist to refine this overview with more exact product and engineering rules.
When a more specific document goes deeper than this README, the more specific document should win.

---

## Project Goal

WordCase should become a **high-quality mobile word game** with a clear identity:

- daily mystery-driven word deduction
- fair and readable puzzle rules
- stylish but restrained presentation
- lightweight social sharing
- strong daily and weekly retention loops
- a product shape that is practical to build and maintain

The project starts **Android first**, with the product, codebase, and content pipeline designed carefully enough that expansion later is possible without rewriting the foundation.

The correct strategy is:

1. define the game clearly
2. define rules and session behavior
3. define screen behavior and content structure
4. define architecture and implementation standards
5. build a focused vertical slice
6. prove the game is actually fun
7. expand carefully

WordCase should prefer **clarity, fairness, and maintainability** over novelty for novelty’s sake.

---

## WordCase in One Minute

WordCase is a **daily mystery word game** framed as a stream of case files and radio-like transmissions.

Each day, the player receives a new case prompt. They make guesses, interpret feedback, reveal evidence, and solve a target word. A successful daily solve contributes evidence to a larger weekly caseboard. Over time, the player builds streaks, grows a history of solved cases, unlocks archive/practice content, and may later compare spoiler-safe results with friends.

WordCase should feel like:
- a daily ritual
- a clever deduction game
- a premium-feeling mobile app
- a mystery board the player gradually uncovers
- a word game that respects the player’s thinking time

It should **not** feel like:
- a noisy casino
- an ad trap
- a clone that depends entirely on obscure words
- a bloated meta-progression machine that buries the actual puzzle

---

## Current Product Shape

WordCase is built around a small set of tightly related layers.

### 1. Daily Case
This is the primary mode and the center of the product.

In the current v1 rules direction:
- each daily case uses one single-word, 5-letter answer
- the player has 6 valid attempts
- feedback is deterministic and explainable
- the player may use one honest optional hint
- solve/fail results are recorded canonically

### 2. Weekly Caseboard
Solved daily cases contribute evidence to a larger weekly investigation.

The weekly layer should provide:
- a visible sense of accumulating progress
- a stronger reason to return across multiple days
- a Weekly Resolution payoff once enough evidence is earned
- a clean weekly history record when complete or incomplete

### 3. Archive / Practice
Archive and Practice exist to support extra play without disrupting the daily ritual.

They may later include:
- past daily cases under archive rules
- starter replay
- curated practice files
- premium archive / special investigation packs later

### 4. Events later
Seasonal or limited-time content may exist later, but only after the core daily and weekly loops are strong.

---

## Design Pillars

### 1. Daily clarity
The player should understand the daily objective quickly.

The core action must be obvious:
- inspect the case frame and clue
- enter a guess
- read the feedback
- narrow the answer
- solve the case

### 2. Fairness and trust
WordCase lives or dies on whether the player trusts it.

That means:
- answer validation must be consistent
- clue feedback must be truthful
- accepted word rules must be explainable
- duplicate-letter logic must be deterministic
- the game must never feel like it “cheated”

### 3. Short sessions with real payoff
A daily session should be short enough to fit into real life, but meaningful enough to feel worth opening.

WordCase should aim for:
- fast startup
- quick resume
- one strong solve or fail moment
- one clear progress beat

### 4. A strong weekly reason to return
Daily content creates the habit.
Weekly case progress deepens the attachment.

### 5. Stylish restraint
WordCase should look and sound distinctive without becoming busy.
The presentation should support concentration rather than compete with it.

### 6. Maintainable depth
The game should be designed so new content, new cases, and new events can be added without breaking the core or burying it under complexity.

---

## What WordCase Is Not

WordCase should not become:
- a generic word-search game
- a crossword clone
- a live-service gimmick machine
- a real-time multiplayer-first product
- a lore dump that gets in the way of play
- a content treadmill that depends on quantity more than quality

It is fine for WordCase to grow later.
It is not fine for WordCase to lose its identity early.

---

## Current Source-of-Truth Docs

The current core source-of-truth docs are:

- `README.md`
- `AGENTS.md`
- `docs/game-rules.md`
- `docs/dictionary-and-validation-rules.md`
- `docs/modes-and-session-state.md`
- `docs/screen-behavior-spec.md`
- `docs/save-sync-and-account-rules.md`
- `docs/content-pipeline-and-liveops.md`
- `docs/progression-economy-and-monetization.md`
- `docs/technical-architecture.md`
- `docs/engineering-standards.md`
- `docs/audio-visual-style-guide.md`
- `docs/accessibility-localization-and-device-support.md`
- `docs/analytics-and-experimentation.md`
- `docs/milestone-implementation-plan.md`

A more specific doc should be treated as more authoritative than this README when it intentionally defines a narrower area in more detail.

---

## Current Build Strategy

WordCase should be built in controlled phases.

The practical order is:
1. project/repo foundation
2. core daily vertical slice
3. real daily content system
4. real weekly caseboard + Weekly Resolution
5. account linking and cross-device trust
6. social/retention expansion and content operations hardening
7. monetization/content expansion
8. beta hardening and release readiness

The daily ritual is the product.
Everything else should support that.

For exact milestone scope and definitions of done, use:
- `docs/milestone-implementation-plan.md`

---

## What to Postpone Until Later

The following should be delayed until the core daily and weekly loops are strong:
- real-time multiplayer
- elaborate user-generated content systems
- heavy narrative scenes
- web-first versions
- elaborate guild/social systems
- battle-pass style layers
- complex currency ecosystems
- broad platform expansion
- flashy experimental modes that distract from the main game
- anything that makes the core harder to understand

WordCase should first become:
1. understandable
2. fun
3. fair
4. stable
5. habit-forming
6. broader later

That order matters.

---

## Immediate Planning Priority

Before deep implementation continues, WordCase should prioritize:

1. keeping the existing docs aligned with each other when a rule changes
2. implementing according to `docs/milestone-implementation-plan.md` rather than inventing side systems early
3. protecting fairness, resume reliability, and daily/weekly truth over breadth
4. continuing to add new docs only when they genuinely reduce guessing instead of fragmenting the source of truth

The repo is already well past the “loose idea” stage.
The next value comes from disciplined implementation, not document sprawl for its own sake.

---

## Success Criteria

WordCase should be considered successful only if it becomes genuinely pleasant to use and worth returning to.

Important success criteria:
- the first session is easy to understand
- the daily case feels satisfying
- the player trusts the rules
- resume and save behavior feel solid
- the weekly layer increases attachment
- the UI feels premium and focused
- the product can grow without losing its identity

A version of WordCase that is technically functional but confusing, unfair, noisy, or forgettable should not be treated as good enough.

---

## Summary

WordCase should be built as a **stylish, fair, Android-first mystery word game** centered on:

- daily case solving
- weekly investigation progress
- strong trust and validation rules
- short but meaningful sessions
- restrained presentation
- low-friction mobile usability
- careful long-term maintainability

The right strategy is:
- lock product truth first
- protect fairness and clarity
- keep detailed docs aligned
- build a focused vertical slice
- prove the daily ritual works
- grow carefully from there

If WordCase follows that path, it has a real chance to become a smart, distinctive daily word game instead of just another clone with a cool name.
