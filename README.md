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

As more focused documents are added under `docs/`, they should refine and clarify this README rather than drift away from it.

---

## Project Goal

WordCase should become a **high-quality mobile word game** with a clear identity:

- daily mystery-driven word deduction
- fair and readable puzzle rules
- stylish but restrained presentation
- lightweight social sharing
- strong daily and weekly retention loops
- a product shape that is practical to build and maintain

The project should start **Android first**, with the product, codebase, and content pipeline designed carefully enough that expansion later is possible without rewriting the foundation.

The correct strategy is:

1. define the game clearly
2. define rules and session behavior
3. define screen behavior and content structure
4. build a focused vertical slice
5. prove the game is actually fun
6. expand carefully

WordCase should prefer **clarity, fairness, and maintainability** over novelty for novelty’s sake.

---

## WordCase in One Minute

WordCase is a **daily mystery word game** framed as a stream of case files and radio-like transmissions.

Each day, the player receives a new case prompt. They make guesses, interpret feedback, reveal evidence, and solve a target word. A successful daily solve contributes evidence to a larger weekly caseboard. Over time, the player builds streaks, fills an archive, unlocks themed case content, and compares spoiler-safe results with friends.

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

## Core Product Direction

WordCase is being built with these assumptions:

- the first real shipping target is **Android**
- the product should be **portrait-first** and easy to use one-handed
- the core loop must work in short sessions, but still feel meaningful
- the daily experience is the heart of the product
- the weekly meta-layer exists to deepen commitment, not distract from the daily
- the game should be strong with sound on, but fully readable and satisfying with sound off
- the product should be understandable to ordinary players within the first minute
- the app should feel premium even before any premium purchase exists

The game should be treated as a serious product, not as a loose prototype pile.

---

## Core Player Fantasy

WordCase should make the player feel like they are:

- intercepting something
- deciphering something
- connecting clues
- building a case
- getting a little smarter each day

The player should not feel like they are merely clearing generic levels.

The emotional rhythm should be:

1. curiosity
2. orientation
3. deduction
4. confirmation
5. case progress
6. anticipation for the next one

That rhythm matters more than flashy surface features.

---

## Design Pillars

### 1. Daily clarity
The player should understand the daily objective quickly.

The core action must be obvious:
- inspect the clue frame
- enter a guess
- read the feedback
- narrow the answer
- solve the case

The game should not need a long tutorial before the first satisfying win.

### 2. Fairness and trust
WordCase lives or dies on whether the player trusts it.

That means:
- answer validation must be consistent
- clue feedback must be truthful
- accepted word rules must be explainable
- obscure edge-case vocabulary should be used carefully
- the game must never feel like it “cheated”

If the player thinks the puzzle is unfair, the mystery fantasy collapses.

### 3. Short sessions with real payoff
A daily session should be short enough to fit into real life, but meaningful enough to feel worth opening.

WordCase should aim for:
- fast startup
- quick resume
- one strong solve moment
- one clear progress beat

### 4. A strong weekly reason to return
Daily content creates the habit.
Weekly case progress deepens the attachment.

The player should feel that each daily solve contributes to something larger:
- a caseboard
- a file set
- a weekly reveal
- a collectible record of solved investigations

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

## Target Audience

Primary audience:
- players who enjoy daily word games, deduction games, and lightweight mystery framing
- players who want a smarter-feeling daily ritual on mobile
- players who enjoy sharing results without needing heavy social coordination

Secondary audience:
- players who like polished casual puzzle games but want a little more atmosphere
- players who enjoy streaks, archives, collections, and weekly objectives
- players who prefer calm but engaging gameplay over twitch action

WordCase should be welcoming to ordinary players, not only hardcore word-game experts.

---

## Core Game Shape

WordCase should be built around a small set of tightly related game layers.

### 1. Daily Case
This is the primary mode and the center of the product.

Each day, the player receives a fresh case built around a single 5-letter target answer in v1.

The daily case should provide:
- a case title
- a clue frame or transmission frame
- bounded guesses
- trustworthy feedback
- an end-of-case result screen
- evidence contribution to the weekly layer

The daily case should be the first thing most players see and the main reason to return.

### 2. Weekly Caseboard
Each successful daily case contributes evidence to a larger weekly investigation.

The weekly layer should provide:
- a visible sense of accumulating progress
- a slightly bigger deduction payoff
- a reason to care about multiple days as a set
- a “closing the case” moment at the end of the week

This layer should deepen commitment without turning into homework.

### 3. Archive / Practice Files
Players should be able to engage with past or side content without disrupting the daily rhythm.

Archive content should eventually support:
- past daily cases after their active window
- curated practice files
- tutorial-like starter files
- special themed case packs later

### 4. Events and themed content later
Seasonal or limited-time content may exist later, but only after the core daily and weekly loops are strong.

---

## Main Modes

### Daily Case
The default mode.
One daily puzzle.
One clean solve or fail state.
One contribution to current progress.

### Weekly Case
A meta-puzzle or caseboard that pulls multiple daily outcomes into one weekly resolution.

### Practice Files
Non-timed and lower-pressure play for learning, archive exploration, and extra engagement.

### Hard Mode later
A stricter version of the daily experience for players who want fewer assists or tighter guess limits.

### Event Cases later
Temporary themed content, alternate clue formats, or seasonal investigations.

---

## Core Gameplay Loop

The WordCase daily loop should look like this:

1. open the app
2. land on today’s case or resume the active case instantly
3. read the case frame
4. enter a guess
5. read the feedback
6. refine understanding
7. solve or exhaust the allowed attempts
8. see result, rewards, and caseboard progress
9. leave feeling closure and anticipation

The loop should stay simple on the surface, even if the content system becomes more sophisticated underneath.

---

## Daily Puzzle Structure

The exact final implementation may evolve, but the intended v1 product direction is:

- each daily case has a single 5-letter target answer
- the answer is a valid word under the current dictionary rules
- the player has a bounded number of attempts
- each attempt produces clear feedback
- optional assists may exist, but they must not lie or create confusion
- the puzzle should feel deducible, not random
- success should feel earned
- failure should still feel understandable rather than insulting

Phrase-based case formats may exist later, but they are not part of the v1 daily puzzle definition unless the docs are intentionally updated.

WordCase should avoid puzzles that can only be solved by trivia knowledge, genre-specific jargon, or dictionary abuse.

---

## Weekly Caseboard Loop

The weekly layer should reinforce the daily ritual without overwhelming it.

A weekly caseboard should:
- collect evidence from completed daily cases
- reveal connections, not just reward icons
- culminate in a satisfying weekly resolution
- make the player feel like they are building toward something

The weekly caseboard is a retention and fantasy layer.
It should remain readable and emotionally rewarding, not mechanically exhausting.

---

## First-Time User Experience

The first-time user experience should be short, clean, and confidence-building.

### First launch goals
- explain the fantasy quickly
- teach the basic action immediately
- give the player a first win fast
- avoid overwhelming them with systems
- show that the game is both readable and stylish

### Recommended first-launch flow
1. app open
2. logo / title
3. guest or sign-in choice with guest allowed by default
4. accessibility quick picks if needed
5. one short guided starter case
6. first success
7. reveal home and today’s case structure
8. only then surface deeper progression and settings

The player should not be hit with:
- store popups
- aggressive push prompts
- season pass clutter
- too many currencies
- social pressure before understanding the game

---

## Session Types

WordCase should support several session shapes.

### A. Daily ritual session
A player opens the app specifically to solve today’s case.

Target feeling:
- focused
- satisfying
- short
- intelligent

### B. Resume session
A player returns mid-case after interruption.

The app must resume cleanly and confidently.

### C. Catch-up / archive session
A player wants more play after the daily.
Archive or practice content should satisfy this without diluting the importance of the daily.

### D. Weekly closure session
A player returns to complete the weekly caseboard payoff.

---

## Main App Surfaces

WordCase should eventually have these primary surfaces:

### 1. Startup / title
Fast, stylish, not overlong.

### 2. Home
The main hub.

Home should prioritize:
- continue / today’s case
- weekly caseboard progress
- archive or practice access
- event visibility later
- profile and settings access

### 3. Daily Case screen
This is the most important screen in the app.

It should focus on:
- clue frame
- guess input
- feedback
- hint access if allowed
- pause / settings access
- zero unnecessary clutter

### 4. Weekly Caseboard
A visual investigation board showing accumulated progress and the current larger case state.

### 5. Archive / Practice
Older files and extra content.

### 6. Results screen
A clean moment of closure after solve or fail.

### 7. Social / compare later
Spoiler-safe result comparison and lightweight friend features later.

### 8. Profile
Stats, streaks, collection progress, unlocked content, achievements if used.

### 9. Settings
Audio, haptics, accessibility, notifications, account, privacy, dictionary region if applicable.

---

## UI and Interaction Principles

WordCase UI should prioritize:
- readability on phone screens
- one-handed interaction where practical
- clear hierarchy
- minimal clutter
- confident feedback
- smooth resume behavior

The game should avoid:
- tiny tap targets
- overloaded screens
- unnecessary modal spam
- style that reduces readability
- flashy transitions that slow down repeat use

The interface should feel premium because it is thoughtful, not because it is loud.

---

## Tone, Visual Direction, and Audio Feel

WordCase should aim for a **modern noir dossier** tone.

Visual direction:
- dark but readable
- restrained highlights
- signal/radio/evidence motifs
- sharp typography
- clue cards and case panels
- subtle texture, not grime overload

Recommended emotional feel:
- mysterious
- intelligent
- calm
- focused
- slightly dramatic at the right moments

Audio direction:
- soft radio/static-inspired UI accents
- crisp confirmation sounds
- subtle success stingers
- no constant noise layer that tires the player

Haptics:
- light confirmation on input
- stronger but still restrained solve feedback
- never obnoxious

Everything should still work well with sound off and haptics off.

---

## Fairness and Trust Rules

WordCase must be trustworthy.

Important rules:
- answers must come from a clearly defined validation policy
- clue feedback must be deterministic and explainable
- daily answers must not silently change after publish
- hints must help honestly
- accepted and rejected guesses must follow documented rules
- failure states must still feel fair
- the player must be able to understand why they won or lost

If WordCase ever feels arbitrary, it loses its core strength.

Fairness should be protected above:
- gimmicks
- monetization pressure
- surprise rule changes
- “gotcha” puzzle design

---

## Difficulty Philosophy

WordCase should challenge through deduction and clarity, not through nonsense.

Difficulty should come from:
- interpreting clue feedback
- narrowing plausible answers
- reading patterns
- making efficient guesses
- understanding the case context

Difficulty should not come mainly from:
- obscure vocabulary
- cheap ambiguity
- punishing input friction
- unreadable UI
- hidden rules

The game should be inviting to new players while still supporting mastery over time.

---

## Progression and Retention

WordCase should use progression to support the puzzle, not replace it.

Important progression layers:
- daily streaks
- weekly case completion
- archive growth
- solved-case history
- profile stats
- themed collections or journals later
- light cosmetic or presentation rewards later

Retention should come from:
- wanting to solve the next case
- seeing weekly progress accumulate
- enjoying the game’s tone and habit loop
- comparing results with friends
- preserving a record of solved investigations

Retention should not depend on:
- guilt-heavy punishment
- endless red notification spam
- manipulative scarcity walls
- making the player babysit too many systems

---

## Social and Sharing Philosophy

WordCase should prefer light, spoiler-safe social features.

Strong social directions:
- shareable result cards
- streak or solve summaries
- friend comparison later
- daily completion visibility later
- weekly case performance comparisons later

Weak directions for early development:
- real-time multiplayer
- chat-heavy community systems
- features that require coordination just to enjoy the core game

The best social loop for WordCase is:
- easy to share
- easy to compare
- impossible to spoil accidentally

---

## Monetization Philosophy

WordCase should respect the player’s thinking time.

Allowed monetization directions:
- ad-free purchase
- cosmetic theme packs later
- optional hint-related monetization handled carefully
- premium archive or special case packs later if justified

Rules:
- do not interrupt active puzzle solving with ads
- do not make the player watch junk just to preserve basic dignity
- do not make the game feel pay-to-solve
- do not damage fairness to improve monetization metrics

Any monetization layer must support the product, not deform it.

---

## Notifications Philosophy

Notifications should be useful, calm, and optional.

Good uses:
- today’s case is ready
- weekly case is nearing reset
- event case ending later
- reminder after long inactivity if enabled

Bad uses:
- spam
- guilt pressure
- fake urgency
- repeated nags in the same day

WordCase should invite players back, not harass them.

---

## Offline, Save, and Resume Behavior

WordCase should be reliable on mobile.

Important behavior:
- autosave current case state after meaningful actions
- resume exactly where the player left off
- support clean background/resume behavior
- preserve unsent or unsynced non-critical progress appropriately
- keep core solo play reliable even with inconsistent connection where possible

The player should trust that:
- their solve was recorded
- their current case is still there
- a brief interruption will not ruin the session

For a mobile daily game, this is not optional polish.
It is part of the core product quality.

---

## Account and Sync Direction

WordCase should allow players to start fast.

Initial account philosophy:
- guest play should be allowed
- account linking should be optional early
- cloud save and restore should exist when the product is ready
- daily completion integrity should remain trustworthy
- leaderboard or comparison integrity should not depend on fragile client assumptions

The product should not force account friction before the player understands why the game is worth keeping.

---

## Accessibility and Device Principles

WordCase should be playable and readable by default.

The game should support:
- high readability
- clear contrast
- usable large text options where practical
- reduced motion
- sound-off usability
- vibration-off usability
- touch targets that work on ordinary phones
- clear differentiation that does not depend on color alone

Accessibility is part of quality, not a bonus feature.

---

## Technical Direction

WordCase should prefer a conservative, maintainable stack that fits a puzzle-heavy mobile product.

### Preferred stack direction
- **Client app:** React Native with Expo
- **Language:** TypeScript-first
- **Shared logic:** TypeScript packages where useful
- **Local persistence:** a reliable mobile-safe local storage layer, likely SQLite-based for important state
- **Backend later / where needed:** a conservative TypeScript API and worker layer for accounts, cloud save, content delivery, events, and leaderboard/social support

### Why this direction
This keeps the product:
- practical for AI-assisted development
- easier to maintain
- cross-functional across app and tooling
- flexible enough for future expansion
- less dependent on heavy engine complexity that the game does not require

WordCase is not trying to be a physics-heavy action game.
It should not adopt heavier technology than it needs.

---

## Architecture Approach

WordCase should be built around a clear separation of concerns.

### Client responsibilities
- render the UI
- manage moment-to-moment interaction
- store and restore local session state
- execute trusted local puzzle behavior where appropriate
- present results and progress clearly

### Backend responsibilities later
- account and cloud synchronization
- daily/weekly content distribution where needed
- event scheduling
- leaderboard or social comparison integrity
- notification support
- analytics ingestion
- admin/content tooling support

### Important rule
Core gameplay rules should not be duplicated carelessly across multiple layers.

WordCase should keep:
- puzzle rules clear
- validation rules centralized where practical
- content data structured rather than hardcoded into random screens

---

## Content Strategy

WordCase should treat content as product infrastructure, not filler.

Daily and weekly content should be:
- reviewed
- tagged by difficulty
- checked for fairness
- checked for ambiguity
- checked for accidental spoilers or bad phrasing
- stable once published except for true correction cases

The content system should eventually support a mix of:
- handcrafted cases
- tool-assisted case generation
- human review before publish

WordCase should not rely on purely unreviewed generation for live daily content.

---

## LiveOps Direction

LiveOps should exist to support the game’s rhythm, not overrun it.

Planned long-term live content may include:
- themed weeks
- seasonal case files
- alternate challenge formats
- archive packs
- occasional special investigations

However, WordCase should first prove:
- the daily case is fun
- the weekly caseboard matters
- the archive loop is worthwhile
- the game can retain players without needing constant gimmicks

---

## Data, Analytics, and Product Learning

WordCase should measure what matters.

Important product signals later:
- first-session completion
- tutorial completion
- daily solve completion
- average attempts to solve
- hint usage
- fail rate
- resume success
- share rate
- archive engagement
- D1 / D7 / D30 retention
- weekly completion rate

Analytics should be used to learn and improve, not to justify making the game more annoying.

---

## Repository Direction

WordCase should be organized so both humans and AI contributors can work cleanly.

Suggested top-level structure:

```text
/apps
  /mobile
  /api
  /worker
  /content-tools

/packages
  /game-rules
  /validation
  /ui
  /audio
  /analytics
  /utils

/docs

/assets
  /art
  /audio
  /fonts
  /marketing
```

### Structure notes
- `mobile` is the Android-first client
- `api` handles server-side features when needed
- `worker` handles scheduled or background tasks later
- `content-tools` supports internal content creation and review workflows later
- `game-rules` and `validation` should protect game truth from drifting
- `docs` should become the written source of truth for product and engineering behavior

---

## Documentation Direction

WordCase should follow a documentation-first approach similar to a serious product, not a loose toy repo.

The planned doc set should eventually cover:
- product/game rules
- modes and session states
- screen behavior
- dictionary and validation rules
- save/sync/account behavior
- content pipeline and liveops
- progression and monetization rules
- audiovisual style guide
- analytics and experimentation rules
- engineering standards
- milestone implementation plan
- AI contributor guidance

The purpose of these docs is to keep:
- product behavior stable
- AI contributors aligned
- engineering choices conservative
- future growth understandable

---

## Product Roadmap

WordCase should be built in controlled phases.

### Milestone 0 — Project foundation
Goal:
- establish repo structure
- app shell
- formatting/lint/test/build expectations
- configuration conventions
- initial documentation
- asset organization rules

### Milestone 1 — Core vertical slice
Goal:
- a fully playable single-case experience
- startup
- home
- one case screen
- result screen
- settings basics
- autosave/resume
- first-pass audio/visual direction

### Milestone 2 — Daily case system
Goal:
- real daily content flow
- publish/update model
- daily rollover behavior
- streak tracking
- result sharing
- archive foundations

### Milestone 3 — Weekly caseboard
Goal:
- evidence accumulation
- weekly progression
- weekly resolution flow
- clearer profile/progress surfaces

### Milestone 4 — Account and sync
Goal:
- guest/account linking
- cloud save
- restore behavior
- integrity around daily completion and profile data

### Milestone 5 — Social and retention layer
Goal:
- friend comparison basics
- spoiler-safe social sharing improvements
- notification rules
- deeper profile stats

### Milestone 6 — Monetization and content expansion
Goal:
- ad-free purchase
- carefully handled hint/economy rules if used
- archive packs or event content
- polished content operations flow

### Milestone 7 — Beta hardening
Goal:
- performance
- fairness audits
- accessibility hardening
- puzzle/content QA
- retention tuning
- bug fixing
- launch readiness evaluation

---

## What to Postpone Until Later

The following should be delayed until the core daily and weekly loops are strong:

- real-time multiplayer
- elaborate user-generated content systems
- heavy narrative scenes
- web-first versions
- elaborate guild/social systems
- battle pass style layers
- complex currency ecosystems
- broad platform expansion
- flashy experimental modes that distract from the main game
- anything that makes the core harder to understand

WordCase should first become:
1. fun
2. fair
3. stable
4. habit-forming
5. expandable

That order matters.

---

## Immediate Planning Priority

Before deep implementation work begins, WordCase should continue to define:

1. the exact daily puzzle rule set
2. dictionary and validation policy
3. screen-by-screen behavior
4. weekly caseboard behavior
5. save/resume and account rules
6. content pipeline and review rules
7. milestone-by-milestone implementation order
8. audiovisual style direction
9. monetization boundaries
10. analytics boundaries

These artifacts should stay aligned with this README so the project grows in one deliberate direction.

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

- lock the product direction first
- protect fairness and clarity
- define screen and state behavior before drifting into feature sprawl
- build a focused vertical slice
- prove the daily ritual works
- grow carefully from there

If WordCase follows that path, it has a real chance to become a smart, distinctive daily word game instead of just another clone with a cool name.