# WordCase Content Pipeline and LiveOps

This document defines how WordCase daily, weekly, starter, and practice content should be authored, reviewed, versioned, published, and corrected.

Its purpose is to keep live puzzle truth structured, reviewable, fair, and stable.

This is a product behavior and operations source-of-truth document.
When tools, scripts, or implementation details disagree with this document, this document should be treated as the intended content operations rulebook until intentionally updated.

---

## 1. Core Principles

WordCase content operations should be:
- deterministic
- reviewable
- boring to operate
- safe against accidental truth mutation
- fair to players
- daily-first

Published canonical puzzle truth must not be casually overwritten.

---

## 2. Published Package Schemas

### 2.1 Published Daily Case package schema
A published Daily Case package must include these required fields:
- `id`
- `kind` = `daily_case`
- `publishDateUtc`
- `publishWindow` `{ startsAtUtc, endsAtUtc }`
- `weekId`
- `dayIndexInWeek`
- `contentVersion`
- `answer`
- `title`
- `caseFrame`
- `clueText`
- `hintData` `{ kind, maxHints, unlockAfterValidGuesses }`
- `dictionaryVersion`
- `feedbackAlgorithmVersion`
- `difficulty` `{ tier, flags }`
- `isCanonical`
- `status`

### 2.2 Published Weekly Resolution package schema
A published Weekly Resolution package must include these required fields:
- `id`
- `kind` = `weekly_resolution`
- `weekId`
- `publishWindow` `{ startsAtUtc, endsAtUtc }`
- `unlockThreshold` = `4`
- `contentVersion`
- `answer`
- `title`
- `caseFrame`
- `clueText`
- `hintData`
- `dictionaryVersion`
- `feedbackAlgorithmVersion`
- `difficulty` `{ tier, flags }`
- `isCanonical`
- `status`

### 2.3 Weekly Resolution v1 engine rule
For v1, Weekly Resolution uses the same core 5-letter guess/feedback engine as Daily Case.

Rules:
- no separate weekly-only validation regime
- no separate weekly-only feedback regime unless a later document intentionally changes that

---

## 3. Frame and Clue Field Rules

`caseFrame` and `clueText` are separate fields.

Field roles:
- `caseFrame` = flavor/context wrapper
- `clueText` = gameplay-supporting nudge

Do not merge these into a single blob field.

### 3.1 Text limits
Required limits:
- `title` hard max: 40 characters, target 18–32
- `caseFrame` hard max: 120 characters, target 50–100
- `clueText` hard max: 90 characters, target 35–75

Rules:
- no manual line breaks
- no long lore blocks on the main Daily screen

---

## 4. Content Lifecycle and Workflow

### 4.1 Official lifecycle statuses
Use these statuses:
- `draft`
- `review_ready`
- `fairness_reviewed`
- `approved`
- `scheduled`
- `published`
- `archived`
- `hotfix_pending`
- `corrected_exception`

### 4.2 Daily and weekly workflow
Official workflow:
1. `draft`
2. automated validation
3. `review_ready`
4. `fairness_reviewed`
5. `approved`
6. `scheduled`
7. `published`
8. `archived`

Published truth must be pinned and must not be casually overwritten in place.

---

## 5. Fairness Sign-Off Authority

Final authority is the product owner or lead curator.

Rules:
- AI/content tools may assist
- explicit human approval is required before any daily answer goes live
- a second human reviewer is recommended later, but not required for launch

---

## 6. Difficulty Tags

### 6.1 Primary tier
Primary `difficulty.tier` values:
- `gentle`
- `standard`
- `challenging`

### 6.2 Secondary flags
`difficulty.flags` may include:
- `repeated_letters`
- `rare_letter_mix`
- `soft_clue`
- `strong_clue`
- `common_word`
- `theme_forward`
- `narrow_solution_space`
- `broad_solution_space`
- `onboarding_safe`

### 6.3 Assignment rules
Assignment flow:
- content tool suggests provisional tier/flags
- human reviewer finalizes tier/flags
- product owner signs off

Usage guardrails:
- starter cases must be `gentle`
- most live dailies should be `standard`
- `challenging` must be intentional

---

## 7. Storage and Versioning Paths

Paths should follow a boring content-tools structure such as:
- `apps/content-tools/data/cases/starter/...`
- `apps/content-tools/data/cases/practice/...`
- `apps/content-tools/data/cases/daily/...`
- `apps/content-tools/data/cases/weekly/...`
- `apps/content-tools/data/manifests/content-manifest-v1.json`

Bundled bootstrap content should live in:
- `apps/mobile/assets/content/bootstrap/bootstrap-content-v1.json`

### 7.1 Starter and practice package family rules
Starter/practice content uses the same content-package family with:
- stable `id`
- `contentVersion`
- `dictionaryVersion`
- `feedbackAlgorithmVersion`
- `kind` = `starter_case` or `practice_file`
- `isCanonical` = `false`

---

## 8. Hotfix and Rollback Rules

### 8.1 Severity classes
Hotfix severity classes:
- `cosmetic`
- `fairness risk`
- `broken truth`

### 8.2 Cosmetic hotfix
Cosmetic hotfix is allowed with:
- same `id`
- incremented `contentVersion`
- no answer/dictionary/feedback truth changes

### 8.3 Fairness or truth issue in v1
For fairness-risk or broken-truth issues in v1:
- do not silently replace the live answer mid-day
- mark the case as `corrected_exception`
- preserve or grant player-favoring streak/evidence protection as required by game rules
- keep history honest
- fix forward for future content

### 8.4 Rollback behavior
Rollback rules:
- keep last-known-good manifest
- never invent replacement puzzle truth on the fly
- never delete pinned local sessions from active players

---

## 9. Summary Rule

WordCase content operations should keep published truth explicit, versioned, human-approved, and fair.

When in doubt, prefer:
- pinned canonical truth
- explicit lifecycle status
- conservative hotfix behavior
- transparent historical integrity

over silent mutation, ad hoc overrides, or schema drift.
