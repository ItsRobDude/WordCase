# WordCase Progression, Economy, and Monetization

This document defines how WordCase handles player progression, unlocks, optional economy systems, and monetization boundaries.

Its purpose is to make WordCase rewarding enough to build habit and attachment without deforming the core puzzle into a noisy free-to-play machine.

This is a product behavior document.
It is not just a revenue brainstorm.

When implementation details, offers, or progression systems disagree with this document, this document should be treated as the source of truth for progression, economy, and monetization behavior until intentionally updated.

---

## 1. Core Philosophy

WordCase should reward:
- daily participation
- puzzle mastery
- weekly follow-through
- clean historical progress
- light identity-building

WordCase should **not** reward:
- paying to bypass fairness
- paying to fix punishment loops
- babysitting multiple currencies
- grinding busywork that buries the daily ritual

### Primary rules
- progression supports the puzzle; it does not replace it
- the daily should still feel meaningful even with no purchase
- the weekly should feel like payoff, not obligation
- monetization must never damage fairness, clarity, or concentration
- boring, direct monetization is better than manipulative monetization
- meaningful paid content is preferred over currency-heavy monetization systems

---

## 2. V1 Progression Scope

For the first real playable versions of WordCase, progression should stay intentionally light.

### Core progression layers in early WordCase
- daily solve streak
- canonical daily result history
- weekly evidence accumulation
- weekly completion history
- solved/failed counts
- archive growth once archive is real
- light profile identity later

### V1 rule
WordCase does **not** need an RPG-like progression system to be fun.

The daily ritual, solve quality, and weekly closure are already the primary progression loop.

---

## 3. Are There Any Currencies at All in v1?

**No. There are no currencies in v1.**

That means:
- no coins
- no gems
- no tickets
- no premium currency
- no XP bar
- no chest keys
- no booster inventory

### Why
This is the best fit for WordCase's current identity:
- cleaner first-time experience
- less UI clutter
- fewer implementation traps
- less sync complexity
- less temptation to warp the game toward monetization-first decisions

### Important rule
The absence of a currency in v1 is intentional, not a missing feature.

---

## 4. What Does a Solve Reward Beyond Streak / Evidence / History?

Beyond streak, weekly evidence, and canonical history, a successful solve should reward:

### Immediate rewards
- a clean solve/result moment
- visible attempts-used summary
- assisted/unassisted solve classification
- progress movement on the weekly board
- the psychological reward of closure and competence

### Profile-facing rewards
- solved case count increments
- assisted/unassisted record updates
- weekly participation visibility later
- preserved solved-case identity in history

### Optional light identity reward later
Once profile/collection surfaces are deeper, a solve may also contribute to:
- resolved-case badges
- dossier stamps
- themed weekly record cards
- archive visibility growth

### What a solve does **not** reward in early WordCase
- currency
- purchasable consumables
- randomized loot
- reward-wheel spins
- chest timers

### Solve reward principle
The solve itself should feel rewarding before any economy layer exists.

---

## 5. What Does Weekly Completion Reward?

Weekly completion should feel more meaningful than a single solve, but still not turn into a giant reward machine.

### Weekly completion reward
A completed weekly case should reward:
- a resolved weekly board state
- a permanent weekly completion record in profile/history
- a stronger case-closed presentation beat than an ordinary daily
- a visible increase in weekly-completion stats
- a collectible-style weekly record later, such as:
  - a resolved case card
  - a dossier seal
  - a weekly file entry

### Preferred long-term weekly reward style
Weekly rewards should be:
- identity-driven
- history-driven
- collection-adjacent
- low-maintenance to produce

This means WordCase should prefer:
- stamps
- seals
- badges
- file cards
- subtle unlock markers

over:
- currencies
- random drops
- gear/loot systems

### Optional later reward
In later milestones, weekly completion may unlock:
- a curated bonus practice file
- a themed archive entry
- a profile cosmetic marker

But this is optional and should not be required for the weekly system to feel complete.

---

## 6. Hint Rules and Economy Relationship

### Are hints always free-per-case, or ever tied to economy later?

For canonical core play:
- **Daily Case hints are always free-per-case**
- **Weekly Resolution hints are always free-per-case**
- the one-hint rule remains part of puzzle design, not economy design

That means:
- no paid extra canonical daily hints
- no paid weekly hints
- no ad-watched extra canonical hint on a failed daily
- no “buy more chances” logic

### Why
WordCase should not make the player wonder whether the puzzle is tuned around purchases.
The canonical daily must feel clean and fair.

### Later optional exception
If WordCase ever introduces a non-canonical practice convenience economy later, economy-linked hints may exist **only for Archive / Practice content**, with strict limits:
- clearly labeled non-canonical
- never changing streak
- never changing weekly evidence
- never changing canonical solve/fail truth

Even then, this should be approached cautiously.

---

## 7. What Unlocks Exist in Profile / Archive / Collections?

### V1 / early unlocks
In early WordCase, unlocks should stay simple and data-light.

#### Profile
Profile may unlock/show:
- streak visibility
- total solves
- total weekly completions
- assisted/unassisted stats
- solved-case history growth

#### Archive
Archive may unlock/show:
- starter replay
- practice files
- past daily cases once archive rules allow
- weekly history later

#### Collections
Collections in the early product should stay light and thematic, not massive.
The best early collection model is:
- weekly case records
- dossier-style seals/stamps
- solved-file archive entries

### Later optional unlocks
Later, WordCase may add:
- theme packs
- profile cosmetic marks
- curated case-file shelves
- solved-week sets
- special investigation pack entries

### Important rule
Unlocks should be mostly presentation/history/identity based.
Do not build a grind-heavy meta layer that competes with the daily ritual.

---

## 8. Economy Direction Beyond v1

### Preferred long-term economy model
The preferred long-term model is:

- **no premium currency**
- **at most one soft currency later, only if truly needed**
- direct purchases where possible instead of multiple abstract wallets

If a soft currency is ever added later, it should be:
- simple
- non-essential
- non-canonical
- not required for the daily to feel good

### If a soft currency is ever added
It should be used for:
- cosmetic/profile presentation items
- optional archive/practice convenience only
- maybe unlocking curated non-canonical extras

It should **not** be used for:
- canonical daily fairness
- buying extra daily attempts
- buying weekly evidence
- buying streak protection
- buying answer reveals for live canonically active play

### Preferred name style if one soft currency is ever needed
Keep it simple and in-theme, such as:
- `Case Notes`
- `Signal Marks`
- `Dossier Stamps`

Do **not** create:
- 3–4 currencies
- layered upgrade economies
- premium-currency laundering systems

---

## 9. Monetization Scope by Milestone

### What monetization is in scope for milestone 2/3/4?

### Milestone 2
**No player-facing monetization.**
Allowed:
- none, beyond internal future-proofing if invisible to players

### Milestone 3
**No player-facing monetization.**
Allowed:
- none, beyond internal entitlement/model prep if invisible and non-disruptive

### Milestone 4
**No player-facing monetization.**
Account linking and sync should remain focused on trust and merge integrity, not commerce.

### First milestone where player-facing monetization belongs
Per the current repo direction, player-facing monetization belongs in **Milestone 6**, after:
- the daily loop is proven fun
- the weekly loop is real
- account/sync behavior is trustworthy
- content operations are repeatable

### Recommended first monetization items
When WordCase does monetize, the first things should be:
1. **one-time ad-free purchase**
2. **cosmetic theme packs**
3. **premium archive / special investigation packs**
4. **campaign-style DLC / themed mission packs** for players who want more meaningful content without waiting for the daily cadence

### Paid content philosophy
WordCase should prefer meaningful paid content over manipulative small purchases.

Strong examples of acceptable later paid content:
- themed case packs
- premium archive collections
- short campaign/investigation packs
- alternate visual themes

Weak examples for WordCase:
- endless consumables
- solve boosters
- progression skips
- currency bundles

---

## 10. What Is Explicitly Forbidden to Monetize?

These are the hard red lines.

### Explicitly forbidden to monetize
- extra canonical daily attempts
- extra canonical weekly attempts
- extra canonical daily hints
- extra canonical weekly hints
- buying streak repair
- buying weekly evidence
- buying weekly unlock thresholds
- buying answer reveals for live canonical play
- paying to convert a fail into a solve
- forced ads during active puzzle solving
- forced ads before the player can read daily feedback/results
- accessibility settings
- large text support
- reduced motion support
- sound/haptics toggles
- ordinary save/restore dignity
- basic profile/history the player already earned

### Strongly discouraged
- aggressive rewarded ads in the early product
- monetized push pressure
- fake “limited time” urgency on basic product dignity
- monetizing common frustrations caused by the game itself

### Product-protection rule
If a monetization idea would make the player ask:
“Is this puzzle fair, or is it trying to sell me something?”
that monetization idea should be rejected.

---

## 11. Ads Policy

WordCase should be very conservative with ads.

### Early-phase rule
Do **not** put ads in the first real vertical slice, daily system, or weekly system milestones.

### Later rule
If ads are added later:
- no interstitials during active solving
- no interstitial immediately before result clarity
- no ad gate for basic fairness/help
- rewarded ads, if ever used, should happen **after closure** and only for non-essential optional value

### Preferred ad stance
The best long-term ad stance for WordCase is:
- very few ads
- or no ads if direct purchase support is enough

---

## 12. Profile / Collection Presentation Rules

Because WordCase should not become a fake overbuilt game-economy monster, profile progression should stay light and durable.

### Best low-production-value collection model
Use collectible records that are cheap to produce and easy to maintain:
- weekly case cards
- solved-week seals
- archive file entries
- profile badges
- theme ownership markers

These are much better than:
- animated loot drops
- dozens of consumables
- rare-tier gear logic
- high-maintenance collectible art sets

This is also much easier to support with the current tooling reality.

---

## 13. Practical Monetization Recommendation

If WordCase proves fun, this is the monetization path to recommend:

### Phase 1
- no monetization

### Phase 2
- one-time **Ad-Free** purchase

### Phase 3
- optional **Theme Pack(s)**:
  - alternate palette
  - alternate card texture set
  - alternate subtle SFX flavor set if feasible

### Phase 4
- optional **Premium Archive / Special Investigation Packs**
- optional **campaign-style DLC / themed mission packs** that provide meaningful extra content for players who want more play outside the daily cadence

That gives WordCase monetization without turning the whole app into economy management.

### What to avoid even long-term
- subscriptions
- battle passes
- premium currencies
- timed offer spam
- purchasable canonical puzzle power

For WordCase, those would create more long-term maintenance pain than value.

---

## 14. Practical Summary

WordCase progression, economy, and monetization should follow this standard:

- no currencies in v1
- no premium currency by default long-term
- solves reward closure, stats, and progress—not coins
- weekly completion rewards history/identity and subtle collection progress
- canonical daily and weekly hints stay free-per-case
- profile/archive unlocks stay light and thematic
- no player-facing monetization in milestone 2/3/4
- first recommended monetization is milestone 6:
  - ad-free
  - cosmetic theme packs
  - premium archive/special case packs
  - campaign-style DLC / themed mission packs
- no pay-to-solve systems
- no monetization that interrupts the thinking moment

If WordCase follows this approach, it can eventually monetize **without betraying the exact thing that makes it worth playing**.
