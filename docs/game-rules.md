# WordCase Game Rules

This document defines the plain-English gameplay rules for WordCase.

Its purpose is to describe how WordCase should behave for players before technical implementation details, UI mechanics, storage details, or backend behaviors are discussed elsewhere.

This is the product-level source of truth for gameplay behavior.

If future code, mockups, or implementation shortcuts disagree with this document, this document should be treated as the gameplay rulebook until intentionally updated.

---

## 1. Core Philosophy

WordCase should be a daily mystery word game built around:
- clarity
- fairness
- short but meaningful sessions
- stylish restraint
- a strong daily ritual
- a weekly layer that deepens commitment without becoming homework

WordCase should make the player feel like they are:
- intercepting something
- narrowing possibilities
- connecting clues
- solving a case
- getting a little smarter each day

WordCase should not feel like:
- a noisy level grinder
- an ad trap
- a clone that depends on obscure dictionary abuse
- a bloated meta-progression machine that buries the puzzle

When rule decisions conflict, WordCase should prefer:
1. fairness
2. readability
3. determinism
4. player trust
5. maintainable simplicity

over novelty, surprise, or monetization pressure.

---

## 2. Core Gameplay Terms

The following terms should be used consistently across WordCase.

### Daily Case
The main puzzle for the day.

### Active Daily Case
The one Daily Case that currently counts for streaks, weekly evidence, and canonical daily completion.

### Case Frame
The visible mystery wrapper around a case, such as the case title, transmission text, and other non-spoiler context presented before or during play.

### Guess
A player-submitted answer attempt.

### Attempt
One consumed valid guess in a Daily Case.

### Valid Guess
A guess that matches the current case rules and passes the documented validation policy.

### Invalid Guess
A guess that is rejected before submission because it does not meet the current case rules or validation policy.

### Feedback
The truthful response the game gives after a valid guess.

### Hint
An optional assist provided under the rules of the current case type.

### Solve
A successful completion of a case by entering the correct answer before attempts are exhausted.

### Fail
An unsuccessful completion of a case after all allowed attempts are used without solving.

### Missed
A Daily Case that was not solved before its active daily window ended and therefore did not count as a completed daily solve.

### Assisted Solve
A solve completed after using a hint in that case.

### Unassisted Solve
A solve completed without using a hint in that case.

### Weekly Caseboard
The weekly investigation layer that collects evidence from Daily Cases.

### Weekly Resolution
The final weekly payoff case that becomes available when the player has earned enough evidence that week.

### Archive Case
A past Daily Case that is no longer active and no longer counts toward current daily progression.

### Practice File
A non-canonical case used for learning, replay, or extra play outside the active daily.

### Canonical Result
The official recorded outcome of an active Daily Case for the player's history, streak, and weekly evidence.

---

## 3. Standard Daily Case Rules

The **Standard Daily Case** is the current core ruleset for WordCase.

Future case types may add variations later, but they should not silently redefine the Standard Daily Case.

### Standard Daily Case format
A Standard Daily Case should have:
- one target answer
- one case title
- one visible Case Frame
- one visible answer length
- a bounded number of attempts
- deterministic feedback after each valid guess

### Current standard answer format
For the Standard Daily Case:
- the answer is one English word
- the answer length is **five letters**
- the answer must follow the documented validation policy

Short phrases, alternate answer lengths, or special case structures are out of scope for the Standard Daily Case unless separately documented later.

### Current standard attempt count
For the Standard Daily Case:
- the player has **six** attempts
- only valid guesses consume attempts
- invalid guesses do not consume attempts

### What the player should know up front
Before the first guess, the player should be able to understand:
- that this is today's active case
- that the answer is five letters long
- how many attempts are available
- where to enter a guess
- that feedback will appear after each valid guess

The player should not have to discover hidden rules through trial and error.

---

## 4. Guess Submission Rules

### Standard guess requirements
A guess is valid only if it:
- contains exactly five letters
- follows the documented dictionary and validation policy
- meets any other explicitly documented Standard Daily Case restrictions

### Invalid guess behavior
If a guess is invalid:
- the game should reject it immediately
- the player should receive a readable explanation or clear rejection signal
- the invalid guess should not consume an attempt
- the puzzle state should not change

### Editing before submission
Before a guess is submitted, the player may change or clear their current input freely.

### No rewind after valid submission
Once a valid guess has been submitted and feedback has been shown:
- that attempt is final
- that attempt remains part of the case history
- the player cannot undo it for canonical play

---

## 5. Feedback Rules

Feedback is one of the most important trust systems in WordCase.

It must be deterministic, consistent, and explainable.

### Standard feedback states
After a valid guess, each letter in the guess must resolve into one of these states:

#### Confirmed
The letter is in the answer and in the correct position.

#### Present Elsewhere
The letter is in the answer but not in that position.

#### Ruled Out
The letter does not match any remaining unmatched instance in the answer after confirmed and present-elsewhere allocation is applied.

### Feedback truth rule
The same valid guess under the same case rules must always produce the same feedback.

There should be no hidden randomness, adaptive cheating, or presentation-layer tricks that change puzzle truth.

### Duplicate-letter rule
Duplicate letters must be handled honestly.

If the answer contains fewer instances of a letter than the guess does:
- only the appropriate number of instances may receive Confirmed or Present Elsewhere feedback
- extra instances must be marked Ruled Out

Confirmed matches should be allocated before Present Elsewhere matches.

This rule must be applied consistently.

### Case Frame versus feedback truth
The Case Frame may add tone, flavor, or contextual orientation.

However:
- the Case Frame must not contradict the actual answer
- the Case Frame must not make false claims about the solution
- feedback remains the authoritative truth during the solve process

---

## 6. Solve and Fail Rules

### Solve rule
A case is solved when the player submits the exact correct answer before the allowed attempts are exhausted.

### Solve result behavior
On solve:
- the case is recorded as solved
- the attempt count used is recorded
- hint usage is recorded
- the player receives a clear result screen
- the result contributes to the Weekly Caseboard according to weekly rules

### Fail rule
A case is failed when the player uses all allowed valid attempts without submitting the correct answer.

### Fail result behavior
On fail:
- the case is recorded as failed
- the correct answer may be revealed
- the player should be able to understand the final outcome
- the failed case does not grant the normal weekly evidence reward

### Canonical lock rule
Once an active Daily Case is solved or failed:
- its canonical result is locked for that daily window
- the player should not be able to re-run it for a different canonical outcome

Review or replay behavior may exist later in archive or practice contexts, but it should not rewrite the original daily result.

---

## 7. Hint Rules

Hints are allowed only when they remain fair, readable, and secondary to deduction.

### Standard Daily Case hint allowance
For the Standard Daily Case:
- the player may use **one** hint per case
- the hint becomes available only after at least one valid guess has been submitted

### Standard hint behavior
The Standard Daily Case hint should:
- reveal one currently unrevealed correct letter
- place that letter in its correct position
- never provide false or ambiguous information

### Hint result classification
If a player uses a hint and later solves the case:
- the result is recorded as an Assisted Solve

If a player solves without using a hint:
- the result is recorded as an Unassisted Solve

### Hint fairness rules
Hints must not:
- lie
- change the answer
- alter prior feedback
- create fake challenge
- secretly modify attempt history

### Hint progression rule
Hints may reduce eligibility for certain performance labels or future prestige metrics if those systems are added later.

However:
- a hinted solve should still count as a legitimate solve
- a hinted solve should still preserve normal streak credit
- a hinted solve should still contribute weekly evidence unless a later documented mode explicitly changes that rule

---

## 8. Daily Window and Rollover Rules

### One active daily at a time
WordCase should have one Active Daily Case at a time.

### Daily window rule
The Active Daily Case is only eligible for canonical daily completion during its active daily window.

### Rollover rule
When a new Daily Case becomes active:
- the previous Active Daily Case stops counting as the current daily
- the previous case no longer grants daily streak credit if solved later in a non-canonical context
- the player's unfinished prior case is recorded as missed unless it already had a canonical solve or fail result

### Resume-before-rollover rule
During the active daily window, the player should be able to leave and return without losing progress.

### Missed case rule
If the player started a Daily Case but did not finish before rollover:
- that case is recorded as missed for canonical daily purposes
- it may later appear in Archive or Practice under separate rules
- its missed status should remain historically accurate

---

## 9. Weekly Caseboard Rules

The Weekly Caseboard should deepen the daily ritual without overwhelming it.

### Weekly structure
A weekly investigation consists of:
- seven daily evidence slots
- one Weekly Resolution

### Evidence rule
Each solved Daily Case grants one evidence fragment to the current week's Caseboard.

For current rules:
- both Assisted and Unassisted solves grant evidence
- failed Daily Cases do not grant normal evidence
- missed Daily Cases do not grant evidence

### Visible weekly progress
The Caseboard should show weekly progress clearly.

The player should be able to tell:
- how many evidence fragments they have earned
- which daily slots are completed
- whether the Weekly Resolution is unlocked

### Weekly Resolution unlock rule
The Weekly Resolution becomes available when the player has earned at least **four** evidence fragments during that weekly cycle.

### Meaning of stronger weekly participation
Solving more Daily Cases in a week should provide:
- a fuller Caseboard
- a stronger sense of completion
- potentially clearer or richer Weekly Resolution context

However, WordCase should avoid making the weekly layer feel punitive for missing one or two days.

### Weekly Resolution result rule
Completing the Weekly Resolution counts as weekly completion.

It does not rewrite or replace the recorded outcomes of the seven Daily Cases that fed it.

### Incomplete week rule
If the player does not earn enough evidence to unlock the Weekly Resolution:
- that week's board remains incomplete in history
- the game should preserve that result honestly rather than pretending the week was completed

---

## 10. Archive and Practice Rules

Archive and Practice should support the core game, not blur it.

### Archive Case rule
Once a Daily Case is no longer active, it may move into the Archive.

Archive Cases:
- are not the current daily
- do not count toward the current streak
- do not contribute evidence to the current week's Caseboard

### Practice File rule
Practice Files exist for:
- first-time learning
- extra play
- archive replay
- low-pressure engagement outside the active daily

### Replay rule
Archive Cases and Practice Files may be replayable.

However:
- replaying them must not rewrite the player's original canonical daily history
- replaying them must not restore a broken daily streak
- replaying them must not grant current weekly evidence

### Tutorial and starter content rule
Starter Cases and tutorial-like Practice Files should not consume or replace the Active Daily Case.

They should exist to teach and build confidence.

---

## 11. First-Time Player Rules

The first-time player experience should create confidence quickly.

### Starter case rule
On first launch, WordCase should provide a short guided starter case before pushing the player into the live daily structure.

### First-win rule
The starter experience should aim to produce a fast first win.

### Low-friction entry rule
Before the player understands the core game, WordCase should avoid:
- aggressive monetization prompts
- social pressure
- heavy progression clutter
- account coercion
- noisy event spam

### Guest-first rule
Guest play should be allowed by default unless product docs intentionally change that rule later.

---

## 12. Streak and Progress Rules

### Daily solve streak rule
A Daily Solve Streak increases when the player solves the Active Daily Case during its active daily window.

### What breaks a streak
For current rules, a streak breaks if the player:
- fails the Active Daily Case
- misses the Active Daily Case

### What does not extend a streak
The following do not extend or repair the current Daily Solve Streak:
- Archive replays
- Practice Files
- starter/tutorial cases
- late non-canonical solves of past daily cases

### Canonical daily result categories
For the Standard Daily Case, the canonical result should be recorded as one of these:
- Solved Unassisted
- Solved Assisted
- Failed
- Missed

### Profile truth rule
Profile and history surfaces should reflect canonical results honestly.

They should not blur together:
- solved versus failed
- assisted versus unassisted
- live daily results versus practice results

---

## 13. Sharing and Spoiler Rules

WordCase should prefer light, spoiler-safe sharing.

### Share safety rule
Share outputs must not reveal:
- the correct answer
- unrevealed clue text that would directly spoil the case
- hidden Weekly Resolution content
- any other sensitive unrevealed puzzle truth

### Shareable information rule
A share output may include information such as:
- solve or fail status
- attempt count used
- whether the solve was assisted
- an abstract performance pattern that does not expose the answer itself
- the case title only if it is not itself spoiler-heavy

### Comparison rule
Friend comparison or later social systems must remain spoiler-safe by default.

The player should be able to compare performance without being forced to ruin an unsolved case.

---

## 14. Monetization Boundaries

Monetization may exist in WordCase later, but it must stay subordinate to trust and concentration.

### Allowed directions
Reasonable future monetization may include:
- ad-free purchase
- cosmetic themes or presentation packs
- premium archive or special case packs later
- carefully handled optional hint-related monetization later

### Forbidden behaviors
WordCase should not:
- interrupt active puzzle-solving with ads
- make the player feel pay-to-solve
- weaken fairness to improve monetization metrics
- pressure the player with manipulative interruption during concentration
- force ordinary dignity behind a paywall

### Product-protection rule
If a monetization idea would make the puzzle feel less fair, less calm, or less trustworthy, it should be rejected.

---

## 15. Correction and Exception Rules

Because WordCase depends on trust, published cases must be handled carefully.

### Published case stability rule
Once a Daily Case is published:
- its answer should not silently change
- its core puzzle truth should remain stable

### Exceptional correction rule
If a published case is found to be broken, unfair, ambiguous beyond tolerance, or technically invalid:
- a correction may be made only as an intentional exception
- the correction should be documented internally
- the product should favor player trust over rigid metrics purity

### Player-protection rule
If a flawed live case affects player outcomes, WordCase should favor solutions such as:
- preserving streaks
- granting fair completion credit where appropriate
- clearly communicating the correction if needed

WordCase should not punish players for the product's own mistake.

---

## 16. Out of Scope for the Current Standard Rules

The following are not part of the current Standard Daily Case unless later documented:
- alternate answer lengths in the standard daily
- short phrase standard dailies
- real-time multiplayer
- chat-heavy social systems
- heavy narrative scenes between ordinary dailies
- battle-pass-like systems
- complicated multi-currency progression
- user-generated canonical daily content
- hidden hard-mode constraints inside the normal daily rules

These may be explored later only if they do not damage the core identity of WordCase.

---

## 17. Summary Rule

WordCase should default to:
- one fair daily mystery word case
- six valid attempts
- deterministic feedback
- one honest optional hint
- reliable solve/fail history
- a weekly Caseboard that rewards steady play
- spoiler-safe sharing
- archive and practice that do not rewrite canonical history
- monetization that never disrespects the thinking moment

If a future feature makes WordCase less fair, less readable, less trustworthy, or less focused, that feature should not be treated as an improvement.
