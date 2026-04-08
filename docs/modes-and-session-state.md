# WordCase Modes and Session State

This document defines the major player-facing modes in WordCase and the state rules that control startup, onboarding, active play, pause, background/resume behavior, results, daily rollover, weekly progression, archive/practice access, and related session transitions.

Its purpose is to make WordCase feel stable, understandable, and trustworthy across ordinary mobile use.

This is a product behavior document.
It is not just a coding note.

When code and this document disagree, this document should be treated as the intended source of truth for mode and session behavior until intentionally updated.

---

## 1. Core Philosophy

WordCase should feel fast to enter, easy to resume, and clear about what the player is doing right now.

A player should never feel like the app:
- forgot their place
- silently swapped their active case
- lost progress because they switched apps
- forced them through extra menus to get back to the puzzle
- changed the meaning of a case mid-session
- buried the main daily ritual under too many competing surfaces

WordCase should follow these principles:

1. the Daily Case is the primary mode
2. the Weekly Caseboard deepens the daily ritual rather than replacing it
3. active session state must be stable and resumable
4. daily rollover must not destroy in-progress trust
5. the home surface should always make the next best action obvious
6. mobile interruptions are normal and must be handled gracefully

---

## 2. Scope

This document defines state and transition behavior for:
- first launch
- onboarding / starter case
- home
- Daily Case
- Weekly Caseboard
- archive / practice files
- pause
- results
- background / resume
- interrupted sessions
- daily rollover
- unavailable / offline states

This document does not define:
- exact dictionary policy
- exact guess validation rules
- exact answer selection rules
- detailed screen layout
- detailed monetization rules
- full save/account/cloud-sync rules

Those belong in their own focused documents.

---

## 3. Launch Assumptions

Unless later docs intentionally change these rules, v1 should assume:

- the primary live mode is the **Daily Case**
- Daily Case answers are **single-word, 5-letter answers**
- a player may use the app as a guest
- the player should be able to solve the core daily without mandatory sign-in
- the app should prioritize fast entry and clean resume over system sprawl
- a player should not lose an in-progress daily simply because the calendar day changed while they were away

---

## 4. Main Modes

WordCase should be organized around a small number of clearly distinct modes.

### 4.1 First-Launch Starter Case
This is the guided first-use experience.

Purpose:
- teach the core loop quickly
- give the player a first win fast
- establish tone and confidence
- avoid overwhelming the player with systems

This mode should be short, controlled, and easier than normal daily play.

### 4.2 Home
This is the main hub.

Purpose:
- show the player the most important current action
- expose Daily Case status
- show Weekly Caseboard progress
- allow entry to archive/practice
- provide access to profile and settings

Home should not compete with the Daily Case for attention.
Home should mainly route the player back into the core loop.

### 4.3 Daily Case
This is the primary live gameplay mode and the center of the product.

Purpose:
- present today’s active case
- allow bounded guesses
- provide trustworthy feedback
- end in a clear solve or fail state
- feed the Weekly Caseboard

### 4.4 Weekly Caseboard
This is the meta-layer that turns multiple daily outcomes into a larger payoff.

Purpose:
- accumulate evidence or progress from daily play
- create a stronger weekly reason to return
- provide a weekly closure moment

The Weekly Caseboard should support the daily ritual, not overshadow it.

### 4.5 Archive / Practice
This is the optional non-primary play surface.

Purpose:
- let players revisit old content
- provide extra play without disrupting the daily
- support learning, practice, and catch-up engagement

Archive/Practice should remain clearly secondary to the live daily.

### 4.6 Settings / Profile / Secondary Utility Surfaces
These are supporting surfaces, not core gameplay modes.

They should be accessible, but should not interrupt entry into the Daily Case.

---

## 5. Session Priority Rules

When the app needs to decide what to foreground, WordCase should use a consistent priority order.

Default priority:
1. unfinished first-launch starter case
2. active in-progress Daily Case
3. Daily Case result screen not yet acknowledged
4. Weekly Caseboard resolution that is ready and unviewed
5. home with today’s Daily Case featured
6. in-progress archive/practice case if resumed intentionally
7. ordinary home fallback

Important rule:
- archive/practice should not automatically steal priority from an in-progress Daily Case
- optional side content should never bury the core daily ritual

---

## 6. First-Launch State

### 6.1 First launch entry
On true first launch, the app should enter the **First-Launch Pending** state.

Allowed steps:
1. app open
2. logo/title
3. optional guest/sign-in choice with guest allowed by default
4. optional quick accessibility picks if presented
5. starter case begins

The player should not be shown:
- aggressive push notification permission prompts
- shop offers
- social pressure
- event clutter
- monetization screens before the first meaningful interaction

### 6.2 Starter case active
Once the starter case begins, the app enters **Starter Case In Progress**.

Rules:
- the starter case should autosave like any other core case
- backgrounding should not discard it
- the starter case should remain resumable until completed or intentionally restarted
- the starter case should be simpler and more guided than the real daily

### 6.3 Starter case completion
When the starter case is solved:
- the player sees a short success/result state
- home is unlocked as the main routing surface
- the current Daily Case is then introduced

### 6.4 Starter case skip or dismissal
If the product later supports skipping:
- the skip must be explicit
- the player must still land somewhere understandable
- the daily should remain playable without being blocked by the tutorial forever

For v1, a short guided starter case is preferred over a full skip-first flow.

---

## 7. Home State Rules

Home is not a dumping ground.
It must always answer: **what should I do now?**

### 7.1 Home primary card
The main card on home should be one of:
- Continue Daily Case
- Open Today’s Case
- View Daily Result
- Resolve Weekly Caseboard
- Continue Starter Case

### 7.2 Home secondary content
Secondary cards may include:
- Weekly Caseboard progress
- Archive / Practice
- Profile progress
- event content later

### 7.3 Home after daily solve
After the Daily Case is solved:
- home should reflect that status clearly
- home should show Weekly Caseboard progress or next meaningful action
- home should not pretend today’s case is still unsolved

### 7.4 Home after daily fail
After the Daily Case is failed:
- home should show that the daily has concluded
- the player should still have a clean path to results, archive, or weekly context
- the game should not loop the player back into a now-closed live daily as if it were still active

---

## 8. Daily Case States

The Daily Case should have explicit states.

### 8.1 Daily Case Unopened
The player has not yet entered today’s case.

Behavior:
- home should show Open Today’s Case
- no attempt history exists
- no active puzzle session exists yet

### 8.2 Daily Case Opened / Intro Viewed
The player has entered today’s case and seen the case frame.

Behavior:
- the daily is now the player’s active case for that day
- the current session becomes resumable
- startup/resume should now favor this case until it reaches an end state or the player leaves intentionally

### 8.3 Daily Case In Progress
The player has started interacting meaningfully with today’s case.

Meaningful interaction may include:
- making a valid guess
- consuming a hint
- revealing clue state if supported
- otherwise mutating puzzle state

Behavior:
- state autosaves after each meaningful action
- backgrounding should preserve exact current progress
- reopening the app should return to Resume Daily Case rather than generic home where practical

### 8.4 Daily Case Solved
The player has solved today’s case within the allowed rules.

Behavior:
- the active puzzle becomes complete
- result state should be shown
- the solved daily should contribute the appropriate progress to the Weekly Caseboard
- the daily should not revert to in-progress later
- the player may still revisit solved details through archive/history surfaces later if supported

### 8.5 Daily Case Failed
The player has exhausted the allowed solve path without solving the answer.

Behavior:
- result/failure state should be shown clearly
- the player should understand that today’s live daily is concluded
- the game should not silently reopen the live daily for more guesses
- any weekly consequence should be consistent and documented elsewhere if needed

Default v1 assumption:
- a failed daily is still recorded as completed history, but should not grant the same primary weekly evidence payload as a solved daily unless later docs explicitly change that rule

### 8.6 Daily Case Result Acknowledged
The player has seen and dismissed the result screen.

Behavior:
- home should now present the next best action
- today’s daily remains marked solved or failed
- the live session is no longer foregrounded as an active unresolved puzzle

### 8.7 Daily Case Expired / Archived
A daily may later move from live daily identity into archive/history identity.

Behavior:
- its historical result should remain preserved
- archive access should not change the meaning of the original outcome
- archived play, if replayable, must be clearly separated from the canonical live result record

---

## 9. Weekly Caseboard States

The Weekly Caseboard should be simple enough to understand at a glance.

### 9.1 Weekly Board Hidden / Not Yet Introduced
Before the player completes the starter case or before the weekly layer is revealed, the Weekly Caseboard may remain visually minimized.

### 9.2 Weekly Board Available
The weekly board exists for the current week but has not yet received meaningful progress.

Behavior:
- it may be visible as a small card on home
- it should not distract from the live daily

### 9.3 Weekly Board In Progress
The player has contributed some meaningful progress to the current weekly board.

Behavior:
- progress should be visible and understandable
- the player should feel accumulation, not clutter
- the board should remain secondary to the live daily unless it becomes ready to resolve

### 9.4 Weekly Board Ready to Resolve
The player has reached the threshold required for the weekly closure moment.

Behavior:
- home may elevate this as a secondary or primary action after the daily is done
- the board should not force itself in front of an unfinished live daily
- the resolution should feel like a payoff, not mandatory paperwork

### 9.5 Weekly Board Resolved
The player has completed the weekly closure.

Behavior:
- the weekly board should show resolved status until the next cycle begins
- the player should retain a historical record of that week if history surfaces exist
- the app should not repeatedly nag the player to revisit a fully resolved weekly board

### 9.6 Weekly Cycle Expired
When a new weekly cycle begins:
- the old weekly board becomes historical
- the new one becomes the current active weekly board
- past results should remain readable if history supports it

---

## 10. Archive / Practice States

Archive/Practice should behave differently from the live daily.

### 10.1 Archive Available
The player may browse old or side content.

### 10.2 Practice Case Selected
The player intentionally opens a practice/archive case.

Behavior:
- this should be clearly labeled as archive/practice content
- it should not be mistaken for today’s live daily
- result handling should remain separate from live daily records

### 10.3 Practice Case In Progress
The player has begun a practice/archive case.

Behavior:
- it should autosave if the product supports in-progress practice resume
- it should not override the priority of an active unresolved Daily Case on future app opens unless the player returns intentionally from within archive/practice

### 10.4 Practice Case Solved / Ended
When a practice case ends:
- the result may be shown
- any practice-only stats may update
- live daily or canonical weekly records must not be rewritten by practice results

For v1, archive/practice should remain clearly separated from live canonical daily progression.

---

## 11. Pause State Rules

WordCase should support a lightweight pause state where needed.

### 11.1 Pause availability
The player should be able to open pause from an active case.

Pause may include:
- resume
- restart case where appropriate
- how to play
- settings
- leave case / return home

### 11.2 Pause behavior
Pause should not:
- discard progress
- change the puzzle state
- mutate the current case unless the player explicitly chooses a destructive action

### 11.3 Destructive actions from pause
If pause offers restart or abandon:
- the action must be explicit
- confirmation should be used when the action would erase meaningful in-progress state
- the game should not make it easy to accidentally wipe a case

For the live Daily Case, restart/abandon rules should remain conservative and fair.
If restart is disallowed for live daily integrity, the pause menu should not imply otherwise.

---

## 12. Results State Rules

Results should feel like closure.

### 12.1 Result entry
A result state should appear immediately after a Daily Case reaches solved or failed state.

### 12.2 Result content
The result screen may include:
- solve/fail status
- answer reveal where appropriate
- attempts used
- weekly contribution
- share action
- next best action

### 12.3 Result priorities
The result screen should provide closure before routing away.
It should not instantly dump the player back to home without acknowledgment.

### 12.4 Result dismissal
Once the player dismisses the result:
- the daily should be treated as concluded
- home should present the next best action
- the result should remain viewable later if the product supports that history surface

---

## 13. App Lifecycle States

Mobile interruptions are normal and must be treated as normal.

### 13.1 Cold start
A cold start occurs when the app is launched fresh.

Preferred behavior:
- fast startup
- restore the highest-priority unresolved state
- do not force unnecessary loading sequences before routing the player

### 13.2 Warm resume
A warm resume occurs when the app returns from background without being killed.

Preferred behavior:
- return the player to the exact same surface and state where practical
- do not bounce them through home unless there is a real reason
- preserve text/input context and current case presentation

### 13.3 Restored after termination
If the app process was killed by the OS:
- WordCase should restore from autosaved state
- the player should land back in the current relevant session if possible
- the app should not behave as if the player never opened the case

### 13.4 Interrupted by external activity
Examples:
- phone call
- notification shade
- app switching
- lock screen
- brief connectivity drop

Behavior:
- active case state should remain intact
- the player should not lose guesses, hints, or current result state
- UI should recover cleanly

---

## 14. Resume Rules

Resume behavior is one of the most important trust behaviors in the app.

### 14.1 Resume target
When the player returns, the app should route them to the most relevant unfinished or newly completed state based on session priority.

### 14.2 Resume from active daily
If a Daily Case is in progress:
- reopening the app should return to that Daily Case directly where practical
- the player should not be forced through home first

### 14.3 Resume from result state
If the player solved or failed the daily and backgrounded before dismissing the result:
- reopening should return to the result state
- the player should still get the intended closure moment

### 14.4 Resume after starter case interruption
If the starter case is unfinished:
- reopening should favor resuming the starter case until it is completed or explicitly skipped if that is later supported

### 14.5 Resume after archive/practice interruption
If an archive/practice case is in progress but a live Daily Case is also unresolved:
- default app-open priority should still favor the Daily Case
- the practice case should remain resumable from within archive/practice

This protects the main ritual.

---

## 15. Daily Rollover Rules

Daily rollover must protect trust.

### 15.1 Active daily identity
Each daily case should be identified by a stable daily case ID.

### 15.2 New-day availability
When a new calendar day becomes active for content selection:
- a new Daily Case becomes available as the current day’s case
- the old daily becomes part of history/archive status according to product rules

### 15.3 In-progress rollover protection
If a player has an in-progress daily from the prior day:
- that in-progress case must remain resumable
- the app must not silently replace it mid-session with the new daily
- the player should finish or otherwise conclusively exit the prior active session before the new daily takes over as the default resume target

### 15.4 Post-rollover routing
After the old in-progress daily is concluded:
- home may then foreground the new current daily
- the transition should feel explicit and understandable

### 15.5 Result preservation across rollover
If a player solved or failed a daily but did not dismiss the result before rollover:
- reopening should still show the pending result state first
- the new daily should not erase the old closure moment

---

## 16. Unavailable and Error States

Unavailable states should be clean and calm.

### 16.1 Content unavailable
If required daily or archive content is not currently available:
- show a clear unavailable state
- do not fake a case
- do not drop the player into a broken blank screen

### 16.2 Offline with cached content
If the content is already present locally:
- the player should be able to continue normally where allowed

### 16.3 Offline without required content
If the required content is missing:
- the player should see a clear unavailable or download-required state
- the app should not invent fallback case data

### 16.4 Save restoration issue
If restoration partially fails:
- recover as conservatively as possible
- do not pretend lost state never existed
- prefer an honest fallback over a misleading reset

---

## 17. Navigation and Transition Rules

### 17.1 Main legal transitions
Allowed common transitions include:
- Startup → Starter Case
- Startup → Resume Daily Case
- Startup → Result Screen
- Startup → Home
- Home → Daily Case
- Home → Weekly Caseboard
- Home → Archive / Practice
- Daily Case → Pause
- Pause → Daily Case
- Daily Case → Result
- Result → Home
- Home → Settings / Profile
- Archive → Practice Case
- Practice Case → Result
- Result → Archive or Home

### 17.2 Transition discipline
Transitions should be:
- quick
- obvious
- low-friction
- consistent

The app should avoid:
- extra confirmation screens for harmless navigation
- modal spam
- routing loops that obscure where the player is

---

## 18. Canonical State Recording Rules

Mode/state transitions should be structurally meaningful.

The system should record enough state to answer:
- has the player opened today’s case
- is the daily in progress
- is the daily solved
- is the daily failed
- has the result been acknowledged
- what weekly board is current
- what weekly board progress exists
- is the starter case still pending
- is there a resumable practice/archive case

Important rule:
- canonical puzzle outcome should not depend on transient UI state alone
- view-layer state and game-truth state must remain separable

---

## 19. Deferred Topics

These topics are intentionally deferred and require later docs or updates before implementation:

- hard mode state differences
- timed challenge states
- live event case variants
- real-time social session states
- user-generated case states
- multi-word case modes
- cross-device session conflict resolution details
- more advanced notification-return flows

---

## 20. Summary Rule

WordCase should have a small number of clear modes, stable active-session behavior, and trustworthy startup/resume logic.

The product should feel:
- fast to enter
- easy to resume
- centered on the Daily Case
- deepened by the Weekly Caseboard
- safe against interruption
- clear about what is live, what is historical, and what is optional

When in doubt, WordCase should prefer:
- preserving player progress
- preserving the meaning of an in-progress or completed case
- routing the player back to the most relevant state
- keeping the Daily Case primary
- making the session flow obvious and calm

over novelty, clever routing, or extra mode complexity.
