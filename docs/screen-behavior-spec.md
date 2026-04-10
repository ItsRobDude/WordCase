# WordCase Screen Behavior Spec

This document defines how WordCase screens should behave from the player's point of view.

Its purpose is to turn the current README and product direction into concrete screen-by-screen behavior so the app does not drift into vague or conflicting UI decisions later.

This is a product behavior document.
It is not a visual mockup document and not a code document.

When implementation details, screen mockups, or code disagree with this document, this document should be treated as the player-facing UI behavior source of truth until intentionally updated.

---

## 1. Scope

This document defines:

- startup and entry flow
- first-time user flow
- home behavior
- Daily Case screen behavior
- result behavior
- Weekly Caseboard behavior
- Archive / Practice behavior
- Profile behavior
- Settings behavior
- global navigation rules
- global overlay and toast behavior
- background and resume behavior
- loading, error, and empty-state behavior
- which screens are active in the current vertical slice versus defined for later

**Normative vs Flexible Scope**

This screen spec is normative for interaction behavior, state changes, feedback behavior, transition order, and availability rules.
It is non-normative for cosmetic animation style, decorative presentation, and final art treatment, which remain flexible unless locked elsewhere.

This document does **not** define:

- exact dictionary rules -> refer to `docs/dictionary-and-validation-rules.md`
- exact hint economy / progression math -> refer to `docs/progression-economy-and-monetization.md`
- exact analytics schema -> refer to `docs/analytics-and-experimentation.md`
- exact content pipeline behavior -> refer to `docs/content-pipeline-and-liveops.md`
- final visual art direction / motion / sound / haptics -> refer to `docs/audio-visual-style-guide.md`

---

## 2. Current Assumptions for This Spec

This document assumes the following based on the current README and AGENTS direction:

- WordCase is Android-first and portrait-first.
- The Daily Case is the main ritual and the most important screen flow in the app.
- The v1 daily puzzle uses a single 5-letter target answer with bounded attempts.
- Guest play is allowed.
- Active case progress autosaves after meaningful actions.
- Active puzzle-solving must not be interrupted by ads.
- Later features should stay hidden until actually supported.

If later docs intentionally change any of those assumptions, this screen spec should be updated to match.

---

## 3. Global Screen Principles

### 3.1 Daily-first priority
The app should always make it fast and obvious to:

- start today's case
- resume an active case
- see whether today's case is solved

### 3.2 One strong primary action
Each major screen should have one clear main action.
The player should not have to scan a crowded screen to figure out what to do next.

### 3.3 Repeat-use speed matters
WordCase is a habit product.
Common repeat actions should be fast:

- open app
- resume case
- enter guesses
- finish case
- share result
- leave

### 3.4 Readability over decoration
No important information should be dependent on tiny text, subtle color differences, or decorative effects.

### 3.5 Hidden beats unfinished
If a screen, mode, or feature is not ready, it should not be exposed as a fake button, fake tab, or dead-end card.

### 3.6 Sound and haptics are additive
All important UI meaning must remain clear with sound off and haptics off.

### 3.7 Semantic UI copy
Any text that affects player understanding of rules, progress, rewards, failure, or availability is exact copy and must be implemented verbatim. Flavor copy may vary within the voice guide as long as semantic meaning is unchanged.

For the exact semantic strings and voice guidelines, refer to `docs/copy-locks-and-voice-guide.md`.

---

## 4. Navigation Model

### 4.1 Primary navigation approach
WordCase uses a small bottom navigation in normal app flow.

Archive bottom-nav behavior is milestone-gated and must follow one exact rule set for Milestones 0/1/2:

- **Milestone 0:** bottom nav is **Home + Profile** only. Archive tab does **not** exist.
- **Milestone 1:** bottom nav remains **Home + Profile** by default. Archive is **hidden entirely** unless unlock criteria are met; **visible-but-disabled Archive is not allowed**.
- **Milestone 2:** same gating rule as Milestone 1. Archive is either fully visible and playable or fully hidden; **visible-but-disabled Archive is not allowed**.

Archive visibility unlock criteria (Milestones 1 and 2):
- Archive appears only when there is at least **2 real playable entries**:
  1. a replayable starter case, and
  2. at least one additional real practice/archive case.
- If this minimum is not met, Archive stays fully hidden (no tab, no dead-end route, no disabled placeholder).

When this section and `docs/milestone-implementation-plan.md` differ on Milestone 0/1/2 Archive gating, **`docs/milestone-implementation-plan.md` is the source of truth and this spec must be updated to match it**.

### 4.2 Why Weekly Caseboard is not a main tab
*Availability: Milestone 3+ (Hidden and completely unavailable in Milestones 0–2)*

The Weekly Caseboard is a major surface, but not a bottom-tab destination in the first version.

Reason:

- the Daily Case is the core ritual
- weekly exists to deepen the daily, not compete with it
- placing weekly on the Home screen keeps the hierarchy clear

The Weekly Caseboard should be opened from:

- the Home screen weekly card
- the Results screen after a solve or fail
- occasional progress prompts when relevant

Weekly visibility and route availability are milestone-gated.
Use `docs/milestone-implementation-plan.md` section **“Surface Availability Matrix (Milestone Source of Truth)”** as the controlling source for:
- whether the Home weekly card appears
- whether the Weekly screen route is accessible
- whether weekly progress visuals are shown
- what weekly elements must remain hidden

### 4.3 Settings access
Settings is not a bottom tab.
Settings should be reached from:

- Profile screen
- Pause overlay where relevant

### 4.4 Social and event surfaces
Social, event, and store surfaces do not appear in primary navigation in the first real product slice.

---

## 5. App Entry Behavior

### 5.1 Cold launch behavior
On cold launch, the app should:

1. show a brief branded startup or title moment
2. restore necessary local state
3. route the player to the correct next screen

The startup or title moment should feel polished, but should never become a long intro that slows repeat use.

### 5.2 Routing rules after launch
After initial load, routing must follow this strict decision tree:

1. **Primary target state**
   - If first launch ever: First-Time Flow.
   - Otherwise, choose the first valid state in this exact order:
     1. unfinished starter case
     2. Daily result screen not yet acknowledged
     3. unresolved Protected Carryover Daily
     4. active in-progress current-day Daily Case
     5. Home
2. **Fallback when target data is unavailable or corrupt**
   - route to the next valid state in the same ordered list
   - never synthesize missing puzzle/session data to preserve a higher-priority route
3. **Final safe fallback screen**
   - Home, with a clear lightweight recovery message if any higher-priority state failed restoration

Normative clarification:
- **ready-and-unviewed Weekly Resolution is a Home CTA state, not an app-open routing target.**
- On launch/resume, Weekly Resolution should be surfaced from Home once higher-priority unresolved states are cleared.

### 5.3 Hard failure behavior
If required case content fails to load:

- show a dedicated load-failure screen
- explain that today's case could not be prepared
- offer Retry
- offer Home if partial app usage is still possible

Do not dump the player into a broken blank screen.

### 5.4 Concrete entry-routing examples
1. **Cold start**
   - Primary target state: unacknowledged result screen for the most recent concluded daily on this device.
   - Fallback when target data is unavailable/corrupt: unresolved Protected Carryover Daily when its state block is valid.
   - Final safe fallback screen: Home.

2. **Warm resume**
   - Primary target state: exact active Daily Case screen that was open before backgrounding.
   - Fallback when target data is unavailable/corrupt: next valid unresolved daily state (carryover first, then Home).
   - Final safe fallback screen: Home.

3. **Post-result reopen**
   - Primary target state: same result screen until acknowledgment.
   - Fallback when target data is unavailable/corrupt: concluded Home state with a non-blocking **View Results** entry when canonical solve/fail exists.
   - Final safe fallback screen: Home (which may present **Resolve Weekly Caseboard** CTA when ready-and-unviewed).

---

## 6. First-Time Flow

### 6.1 Purpose
The first-time flow exists to:

- explain the fantasy quickly
- teach the core action immediately
- earn the player's confidence fast
- avoid overwhelming them

### 6.2 First-time sequence
Recommended sequence:

1. Brief title or startup
2. Welcome card
3. Guest or Sign-in choice
4. Optional quick accessibility setup
5. Guided Starter Case
6. First success result
7. Home reveal
8. Today's Case card introduced

### 6.3 Guest or Sign-in screen
Behavior:

- **Continue as Guest** is the primary button
- sign-in is secondary
- no account wall before first solve
- explain that account linking can protect progress later

### 6.4 Accessibility quick setup
This should be a lightweight optional screen or bottom sheet, not a long wizard.

Recommended quick picks:

- larger text
- reduced motion
- haptics off
- sound off

### 6.5 Guided Starter Case
The starter case should:

- be shorter and easier than a normal daily
- teach guess entry and feedback
- produce a first win quickly
- not use tricky edge-case vocabulary

### 6.6 Notification permission timing
Do **not** ask for push permission before the player understands the game.

Recommended timing:

- after the first successful solve
- or after the player returns another day
- always optional

---

## 7. Home Screen

### 7.1 Purpose
Home is the main hub.
It should answer one question instantly:

**What should I do right now?**

### 7.2 Main layout
Home should contain:

- top app bar
- primary Today or Continue card
- Weekly Caseboard progress card
- Archive or Practice entry card when archive content is available

### 7.3 Top app bar
Recommended elements:

- WordCase wordmark or compact title treatment
- current streak indicator
- small settings or profile access only if it does not clutter the layout

Do not overload the top bar with currencies, banners, or multiple icons.

### 7.4 Primary Today card
This is the most important card on Home.

It should change state based on progress.

**Before today is started**
- show **Today's Case** label
- show case title
- show one-line teaser
- show **5 Letters**
- show **6 Attempts**
- primary button: **Start Today's Case**

**Mid-case**
- show **Today's Case** label
- show case title
- show progress summary (such as attempts used)
- show hint-used status if relevant
- primary button: **Continue Case**

**After solve**
- show **Today's Case** label
- show case title
- show solved badge
- show attempts used
- show Assisted or Unassisted tag
- primary button: **View Results** or **View Weekly Evidence**

**After fail**
- show **Today's Case** label
- show case title
- show concluded badge
- primary button: **Review Result**
- do not reveal the answer on Home before result review

### 7.5 Weekly Caseboard card
*Availability: Milestone 3+*

This card is only shown in milestones where the weekly Home card is enabled by `docs/milestone-implementation-plan.md` section **“Surface Availability Matrix (Milestone Source of Truth)”**.

This should appear directly below the Today card.

It should show:

- **This Week's Case** label
- weekly case title
- evidence count
- resolution status: locked, ready, or resolved
- tap target to open the full Weekly Caseboard screen

### 7.6 Archive or Practice card
*Availability: Milestone 1+ (Only when unlock criteria are met)*

This card remains secondary and uncluttered.
It shows:

- **Archive** label
- playable count or simple descriptor
- **Browse Archive** CTA

### 7.7 Event slot behavior
Event content should not reserve permanent empty space before events exist.
If no event is active, nothing event-related appears on Home.

### 7.8 Home screen states
Home should support these states:

- normal pre-solve
- mid-case return
- post-solve
- post-fail
- no network
- partial sync pending later
- content load retry

---

## 8. Daily Case Screen

### 8.1 Purpose
This is the most important screen in WordCase.

It should feel:

- focused
- readable
- calm
- premium
- fast to use repeatedly

### 8.2 Core layout
Recommended structure from top to bottom:

1. top app bar
2. case title or transmission frame
3. clue panel
4. guess history or board
5. optional assist or hint strip
6. on-screen keyboard or input controls

### 8.3 Top app bar
Recommended contents:

- left: back or home exit
- center: case title or compact daily label
- right: pause or help

The top bar must stay minimal.

### 8.4 Transmission and clue area
The upper content area stays tight and readable.
It includes:

- case title
- one short transmission or case-frame line
- one short clue line
- no long lore blocks on the main case screen

The clue area should not push the main puzzle too far down the screen.

### 8.5 Guess board or history area
This is the central play area.
It should show:

- submitted guesses
- feedback clearly attached to each guess
- remaining rows or attempt slots
- current active row if applicable

The board must be readable at a glance.

### 8.6 Input area
For v1, input should use an on-screen keyboard or equivalent direct touch input.

Requirements:

- large tap targets
- obvious delete key
- obvious submit or enter behavior
- disabled or inactive states should be visually clear
- input should feel responsive

### 8.6.1 Keyboard state aggregation (normative)
On-screen keyboard keys aggregate to the strongest earned state in the current puzzle and never downgrade.

Priority order:
- **Confirmed** > **Present Elsewhere** > **Ruled Out** > **Default**

Mandatory aggregation rules:
- an invalid guess does not change keyboard state
- a duplicate-guess rejection does not change keyboard state
- if duplicate-letter situations create mixed local outcomes across rows, keyboard state still keeps the highest-confidence global state
- a hint-revealed fixed letter promotes that key to at least **Confirmed**
- fixed-cell locking remains a board/input behavior contract and is not a separate keyboard-truth layer
- keyboard state treatment must use more than color alone (for example color + fill/border/icon/text treatment)

### 8.6.2 Daily Case input behavior with fixed hint positions (normative)

When a hint reveals a fixed letter position, input handling must follow these mandatory rules:

- fixed hint positions are locked and cannot be edited directly in the active row
- fixed hint letters are auto-populated in every future active guess row
- submit validation must reject any guess payload that attempts to override a fixed hint letter, even if that payload is injected through non-standard input paths
- this rejection must not consume an attempt and must not mutate board history

Player-facing feedback for override attempts must be exact and immediate:

- keep focus on the active input row
- show inline field-state emphasis on the conflicting fixed cell(s)
- show a non-blocking toast with the message: **Hint-locked letter cannot be changed.**
- preserve all other editable letters in the row so the player can correct and resubmit quickly

### 8.6.3 Example turn: before and after hint use
Example answer (hidden from player): **CRANE**

Before hint use:

- Row 1 submitted: `SLATE` → feedback shown
- Row 2 active input: `_ _ _ _ _`
- No fixed hint cells

Hint used (deterministic left-to-right first unrevealed correct position):

- Position 1 is revealed and fixed as `C`

After hint use:

- Row 1 remains unchanged with original feedback
- Row 2 active input becomes: `C _ _ _ _` with cell 1 locked
- Row 3+ (future rows) initialize as `C _ _ _ _` with cell 1 locked
- If player attempts `B` in cell 1 via any input path, submission is rejected with **Hint-locked letter cannot be changed.** and no attempt is consumed


### 8.6.4 Tile feedback reveal sequence and cue order (normative)
After a valid guess is submitted:

1. the submitted row locks immediately
2. the valid-submit cue fires once
3. feedback reveals left-to-right using:
   - per-tile state transition duration: **160ms**
   - per-tile stagger: **80ms**
4. after the final tile resolves, terminal solve/fail cue and result routing may fire if the guess is terminal

Mandatory guardrails:
- input remains locked during reveal
- no extra per-letter sound effects in v1
- if the guess is non-terminal, input becomes active again immediately after the final tile resolves
- with reduced motion enabled, all five tile states reveal together with minimal fade (no staggered motion)

### 8.7 Hint or assist access
Hint access must stay secondary.
It should not dominate the main solving area.
If hints exist, they should appear as:

- a small button or chip
- a bottom sheet when opened
- clearly labeled cost or consequence if any exists later

### 8.8 Daily Case screen states
The screen must support:

**Fresh state**
- no guesses entered yet

**Active state**
- one or more guesses entered

**Invalid guess state**
- rejected input shown through clear inline or toast feedback

**Hint-used state**
- visible but not noisy indication that an assist has been used

**Solved state**
- transition to results after solve confirmation

**Failed state**
- transition to results after final allowed attempt or failure trigger

**Offline state**
- play remains stable where possible
- do not imply cloud sync is required for every guess

### 8.9 Invalid guess behavior
Invalid guess feedback should:

- be immediate
- be readable
- never feel insulting
- explain the problem at the correct level

Examples:

- not enough letters
- word not accepted
- case already completed

### 8.10 Back behavior from Daily Case
If the player leaves the screen mid-case:

- progress must already be saved
- return to Home without punishment
- Today card should now read Continue Case

Back behavior should not silently reset or discard progress.

### 8.11 How Feedback Works visibility
How Feedback Works is taught explicitly in the starter case.

In live daily play:
- show a small dismissible **How feedback works** link on the Daily Case screen for the player's first 3 live daily cases
- stop surfacing that link automatically after those first 3 live daily cases
- keep How Feedback Works permanently accessible from Pause/Help

---

## 9. Pause or Case Menu Overlay

### 9.1 Purpose
The pause overlay gives the player safe control without cluttering the main case screen.

### 9.2 Recommended actions
For the live daily:

- Resume
- How Feedback Works
- Accessibility or quick settings
- Exit to Home
- **Abandon Carryover Case** (only when a Protected Carryover Daily exists)

### 9.3 No live daily restart button
The live Daily Case should not expose a casual reset or restart button in the pause menu.

If replay or reset behavior is ever allowed, it should be intentional, documented, and not casually available where it can muddy fairness.

### 9.4 Pause overlay style
Use a clean modal or sheet.
Do not fully visually detach the player from the case context.

### 9.5 Explicit abandon action (Protected Carryover only)
The explicit abandon action is only for an unresolved **Protected Carryover Daily**.

Location and visibility rules:
- show **Abandon Carryover Case** at the bottom of the pause/case menu in a destructive section
- do not show this action for same-day live dailies that are not in Protected Carryover state
- do not expose abandon from Home cards or from ordinary back navigation

Confirmation copy pattern:
- title pattern: **Abandon this carryover case?**
- body pattern: **This will mark yesterday's carryover case as missed and cannot be undone. Today's case will become your active daily.**
- primary destructive button: **Mark Missed**
- secondary safe button: **Keep Solving**

Post-action routing:
- after confirmation, close the dialog and route to Home
- Home should foreground the newly active current daily with **Open Today's Case** (or **Continue Daily Case** if already started)
- the abandoned carryover should no longer resume as active daily state

---

## 10. Results Screen

### 10.1 Purpose
The results screen is the closure moment after solve or fail.

It should:

- confirm outcome
- explain what happened clearly
- show immediate progress impact
- offer one strong next action
- transition in and out briefly and with restrained motion

### 10.2 Success result layout
Success results show:

- solved headline
- case title
- correct answer
- attempts used as X/6
- solve class tag: Assisted or Unassisted
- weekly evidence gained
- current weekly progress summary
- primary CTA: **View Weekly Evidence** if the weekly board changed meaningfully, otherwise **Return Home**
- secondary CTA: **Share Result**

### 10.3 Failure result layout
Failure results show:

- fail or unsolved headline
- case title
- correct answer reveal
- attempts used as 6/6
- clear weekly evidence outcome (normally no evidence gained)
- weekly progress summary if helpful
- primary CTA: **Return Home**
- secondary CTA: **Share Result**

### 10.4 Primary action rules
Primary CTA rules:

- use **View Weekly Evidence** only when the weekly board changed meaningfully
- otherwise use **Return Home**

Do not overload the result screen with five competing buttons.

### 10.5 Share behavior
Sharing should always be spoiler-safe.
The share entry point should be visible but secondary.

If streak impact is shown, keep it low-emphasis.
Do not add currencies, ad prompts, premium upsells, chest rewards, or similar clutter to the result screen.

### 10.6 Return routing
Leaving results should go to:

- Weekly Caseboard if chosen
- Home otherwise

On another device that has synced a concluded daily, the app may show concluded Home status and offer View Results without forcing the same blocking result flow, unless that device itself has a local pending unacknowledged result.

---

## 11. Weekly Caseboard Screen
*Availability: Milestone 3+*

### 11.1 Purpose
This screen turns multiple daily solves into a larger payoff.

It should feel meaningful, but not like a second job.

### 11.2 Main contents
Recommended layout:

- weekly case title
- current week timer or status
- evidence board visual
- unlocked evidence elements
- locked future slots
- weekly completion state if reached

### 11.3 Behavior before enough progress exists
Before enough daily progress is earned:

- show partial board clearly
- make locked items legible but inactive
- avoid cluttering with filler rewards

### 11.4 Behavior after new evidence is earned
If entered from Results:

- animate or highlight the newly added evidence
- make the change clear
- keep the moment brief and satisfying

### 11.5 Weekly resolution state
When the weekly case is complete:

- reveal final board state
- show weekly closure card
- preserve access in archive or history later

### 11.6 Weekly resolution retry contract
If a Weekly Resolution run fails during the active week:

- show a clear failed-run state on the Weekly Caseboard screen
- primary action: **Retry Weekly Resolution**
- failed run remains recorded as failed for history/analytics and is not resumed
- retry instantiates a new run with full attempts and a fresh per-run hint allowance
- retries are unlimited until weekly rollover
- copy should state that daily evidence remains intact

### 11.7 Weekly resolution run HUD expectations
While a Weekly Resolution run is active, the run HUD should mirror Daily Case clarity:

- show attempts remaining for the current run only
- show hint availability for the current run only
- do not display prior failed-run attempts as carryover debt

### 11.8 Exit behavior
Exiting the Weekly Caseboard returns to Home.

---

## 12. Archive or Practice Screen
*Availability: Milestone 1+ (Only when unlock criteria are met)*

### 12.1 Purpose
Archive or Practice exists to support extra play without weakening the daily ritual.

### 12.2 Main contents
Recommended sections:

- Starter Files
- Past Cases
- Practice Files
- special packs later when real

### 12.3 Item presentation
Each archive item should show:

- title
- type
- solved or unsolved state
- difficulty tag later if used
- availability state

### 12.4 What should not appear
Do not show:

- unreleased future dailies
- broken placeholders
- empty premium shelves before those systems exist

### 12.5 Archive behavior in early milestones
Archive is visible only when it contains real playable content.

Rules:
- do not show an empty "coming soon" Archive in a real public or playtest build
- for early internal or dev builds, Archive may remain hidden until content exists
- for real public or playtest builds, show Archive only when it has actual playable content, such as starter replay and/or at least one practice file

---

## 13. Profile Screen

### 13.1 Purpose
Profile is a quiet stats, history, and settings surface.

It should not become a trophy mall.

### 13.2 Main contents
Recommended contents:

- current streak
- total cases solved
- weekly cases completed
- archive progress
- simple achievements later if justified
- settings entry
- account entry
- ad-free or purchases later if real

### 13.3 Early milestone profile behavior
For the early build, Profile can be minimal:

- streak
- cases solved
- account state
- settings entry

That is enough.

---

## 14. Settings Screen

### 14.1 Purpose
Settings should be practical, readable, and easy to change quickly.

### 14.2 Main sections
Recommended sections:

- Audio
- Haptics
- Accessibility
- Notifications
- Account
- Privacy or legal
- support or help later

### 14.3 Audio section
- music on or off
- sound effects on or off

### 14.4 Haptics section
- haptics on or off
- optional strength later if worth supporting

### 14.5 Accessibility section
- larger text
- reduced motion
- high clarity or contrast support when available
- color-independent indicators

### 14.6 Notifications section
- daily reminder
- weekly reset reminder
- all notifications off

### 14.7 Account section
- guest or linked status
- sign in or link account
- restore or sync later when supported

### 14.8 Settings availability from case screen
A limited quick-settings entry may appear in pause, but the full settings screen remains the authoritative settings surface.

---

## 15. Global Overlays, Sheets, and Toasts

### 15.1 Toasts
Use toasts for:

- resumed case
- invalid word
- setting saved
- offline warning if brief
- share copied or sent confirmation

Toasts should be brief and never hide critical puzzle information.

### 15.2 Sheets or modals
Use sheets or modals for:

- hint details
- pause menu
- account prompts
- accessibility quick setup
- notification permission education prompt

### 15.3 Confirmation dialogs
Use confirmations sparingly.
Only confirm actions that matter, such as:

- leaving certain guided flows
- disabling an important accessibility setting if needed
- account unlink actions later

Do not ask for confirmation on ordinary navigation.

---

## 16. Background, Resume, and Interruption Behavior

### 16.1 Autosave rule
Autosave after meaningful actions such as:

- guess submitted
- hint used
- major result state reached
- settings changed if relevant

### 16.2 Resume rule
When the player returns after interruption:

- restore exact active case state
- restore board, guesses, and relevant clue state
- do not force the player back through Home unless no active case exists

### 16.3 Phone interruption tolerance
Short interruptions such as:

- app switching
- call interruption
- notification pull-down
- temporary backgrounding

must not damage active progress.

### 16.4 Daily completion integrity
If today's case was already solved:

- reopening the app should not briefly present it as unsolved
- state restoration must be consistent and trustworthy

---

## 17. Loading, Empty, and Error States

### 17.1 Loading states
Loading states should be:

- brief
- themed
- readable
- never ambiguous about whether the app is working

### 17.2 Empty states
Empty states should appear for:

- profile with no long-term stats yet
- weekly caseboard before enough evidence exists

Empty states should feel intentional, not unfinished.

For Archive visibility rules, follow Section 12.5:
- real public/playtest builds do not expose an empty Archive surface
- internal/dev builds may hide Archive entirely until playable content exists

### 17.3 Error states
Every major screen that depends on loadable content should have a defined error state with:

- simple explanation
- retry button
- safe exit path

---

## 18. Hidden and Deferred Screens

The following surfaces may be documented later but should remain hidden until real:

- social comparison screen
- friend list
- event hub
- premium store
- theme pack browser
- leaderboard screen
- advanced achievements screen

Do not expose these as dead buttons or "coming soon" clutter in the first real product slice.

---

## 19. Current Implementation Priority from This Spec

The screens that should be treated as **fully required** for the first strong playable slice are:

- Startup or app entry
- First-Time Flow
- Home
- Daily Case
- Pause overlay
- Results
- Settings
- minimal Profile

The screens that should be **behaviorally defined now but can be shallower in first implementation** are:

- Weekly Caseboard
- Archive or Practice (shown only when playable content exists)

The screens that should be **hidden entirely for now** are:

- social comparison
- events
- store or premium
- leaderboard
- advanced collection systems

---

## 20. Summary Rule

If WordCase has to choose between:

- more screens
- louder presentation
- extra tabs
- more buttons
- more systems

or instead:

- a cleaner Home
- a stronger Daily Case
- faster resume
- clearer results
- more trustworthy state behavior

WordCase should choose the second set every time.

The best WordCase screen behavior is:

- calm
- clear
- fast
- readable
- fair
- daily-first
