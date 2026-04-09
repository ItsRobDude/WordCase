# WordCase Dictionary and Validation Rules

This document defines how WordCase handles dictionary membership, answer eligibility, guess validation, letter evaluation, and related fairness rules.

Its purpose is to make WordCase feel consistent, explainable, and trustworthy before deeper implementation begins.

This is not just a coding document.
It is the product-level source of truth for what counts as a valid answer and a valid guess.

When code and this document disagree, this document should be treated as the intended rulebook until intentionally updated.

---

## 1. Core Philosophy

WordCase should feel fair, clear, and consistent.

Players should lose because they failed to solve the case, not because:
- the game used a trick word
- the validation rules felt random
- the dictionary was narrower than a normal player would expect
- repeated-letter logic behaved inconsistently
- a published answer changed after the puzzle went live

WordCase should follow these principles:

1. guess acceptance should be broader than answer selection
2. answer selection should be narrower, cleaner, and more curated
3. published puzzle truth should remain stable
4. validation behavior should be deterministic
5. moderation and safety rules should not create confusing edge cases for normal players

---

## 2. Launch Scope

Version 1 dictionary and validation behavior should stay intentionally narrow.

### v1 language
- WordCase launch language is **American English primary**, with limited support for non-US spelling variants as valid guesses only where intentionally included in the Guess Lexicon

### v1 daily answer format
- daily cases use **one single-word answer**
- daily answers are **exactly 5 letters** at launch
- daily answers use alphabetic letters only

### v1 mode coverage
The rules in this document apply to:
- daily case mode
- weekly resolution mode
- archive/practice versions of daily cases
- any tutorial puzzle that uses the same guess/feedback system

### deferred scope
The following are intentionally out of scope for v1 and require a later doc update before implementation:
- multi-word phrase answers
- answers with spaces
- answers with punctuation
- answers with apostrophes or hyphens
- non-English dictionaries
- player-created/custom dictionary packs
- full region-switching dictionaries such as UK English mode

---

## 3. Dictionary Model

WordCase should use **three word lists**, not one.

### 3.1 Answer Lexicon
This is the curated list of words that may be selected as actual puzzle answers.

Rules:
- every answer must be in the Answer Lexicon
- every answer must also exist in the Guess Lexicon
- Answer Lexicon words should be curated for fairness, clarity, recognizability, and answer quality

### 3.2 Guess Lexicon
This is the broader list of words the game accepts as valid guesses.

Rules:
- Guess Lexicon should be broader than the Answer Lexicon
- Guess Lexicon should reduce “that should have counted” frustration
- Guess Lexicon may include words that are valid guesses but poor answer choices
- Guess Lexicon may include selected non-US spelling variants where those variants are common and recognizable to English-speaking players

### 3.3 Blocked Terms List
This is a moderation/safety list for words that should never be accepted or surfaced.

Rules:
- blocked terms override other lists
- a blocked term is invalid even if it appears in a source dictionary
- blocked terms should be reviewed carefully and intentionally

---

## 4. Input Normalization Rules

WordCase input should be normalized in a simple and predictable way.

### Character rules
In v1:
- only letters A through Z are valid input characters
- no spaces
- no punctuation
- no apostrophes
- no hyphens
- no numbers
- no emoji
- no accented characters as distinct playable input

### Case handling
- input is case-insensitive
- internal comparison should use one canonical form consistently
- UI may display guesses in uppercase for readability and style consistency

### Whitespace handling
- leading and trailing whitespace should be trimmed before validation
- internal guess strings should not preserve stray whitespace

### Keyboard behavior
Where practical:
- the game keyboard should prevent invalid characters from being entered
- physical keyboard input should still be validated by the same rules

### Autocorrect behavior
- mobile autocorrect and smart punctuation should not alter the guess
- the game should treat the player’s typed word exactly as normalized by these rules

---

## 5. Guess Submission Rules

A guess is valid only if all of the following are true:

1. it matches the required answer length for the active puzzle
2. it contains only valid characters for the active dictionary rules
3. it exists in the Guess Lexicon
4. it is not blocked by the Blocked Terms List
5. it has not already been submitted in the current puzzle

### Duplicate guess rule
- a player may not submit the exact same guess twice in one puzzle
- the game should show a friendly “already guessed” style message
- duplicate guesses are rejected in every mode that uses the standard guess/feedback engine, including Daily Case, Weekly Resolution, Starter Case, Archive, and Practice
- duplicate guess rejection must not consume an attempt

### Invalid guess handling
Invalid guesses should fail clearly and consistently.

Preferred failure reasons:
- wrong length
- already guessed
- not in word list
- guess not allowed

The game should avoid confusing or overly specific moderation messages for harmful terms.
In those cases, a generic invalid/guess-not-allowed response is preferred.

---

## 6. Answer Eligibility Rules

Not every valid guess should be a valid answer.

WordCase answers should be curated more strictly than guesses.

### Every answer must be:
- a single word
- exactly 5 letters in v1 daily mode
- alphabetic only
- valid under the active dictionary rules
- reasonably recognizable to a general American English player
- spelled in one clear standard answer form
- fair for deduction-based play rather than trivia-based recognition

### Answers must not be:
- proper nouns
- personal names
- place names
- brand names
- acronyms
- initialisms
- abbreviations
- contractions
- possessives
- hyphenated forms
- apostrophe forms
- numerals or alphanumeric strings
- slurs or hate terms
- highly explicit profanity
- extremely obscure “Scrabble bait” words
- archaic forms unless intentionally approved later
- highly domain-specific jargon unless it is widely common in normal speech
- non-US spelling variants where a common US answer form exists

### Answer tone rule
WordCase may use mystery/crime/noir-themed vocabulary when it is still common and fair.

Words like these may be acceptable if they are common enough:
- clue
- crime
- trace
- alarm
- knife
- dusk

Words should not be chosen just because they fit the theme if they are niche, archaic, or unfair.

### Inflection rule
Inflected forms such as plurals or verb tenses should be used sparingly as answers.

Preferred answer style:
- clean standalone base words
- natural everyday vocabulary
- words that feel like real human guesses, not technical list-mining

An inflected word may be used as an answer only if:
- it is very common
- it feels natural as a standalone word
- it is not likely to make players feel tricked

---

## 7. Guess Eligibility Rules

Guesses should be broader than answers, but not chaotic.

### Valid guesses may include:
- common standalone words
- common inflected forms
- common plural forms
- common verb tense variations
- common everyday words that are too plain, awkward, or repetitive to use as answers
- selected non-US spelling variants where intentionally included in the Guess Lexicon

### Valid guesses should not include:
- slurs
- hate terms
- common non-slur profanity at launch
- proper nouns in v1
- abbreviations
- acronyms
- contractions
- apostrophe forms
- hyphenated forms
- alphanumeric strings
- ultra-obscure dictionary oddities that normal players would not reasonably expect

### Profanity rule
For v1:
- common non-slur profanity is out at launch
- profanity is not valid as answers
- profanity is not valid as guesses at launch
- profanity should be revisited only through a later explicit lexicon review if needed
- profanity must never appear in hints, help text, promotional copy, or social share templates

---

## 8. Proper Nouns, Regional Spellings, and Variant Forms

### Proper nouns
For v1:
- proper nouns are not valid guesses
- proper nouns are not valid answers

This keeps the rules simple and avoids debates around capitalization and cultural familiarity.

### Regional spellings
For v1:
- WordCase may accept selected non-US spelling variants as valid guesses
- non-US spelling variants should not be used as official daily answers in v1
- inclusion of non-US spelling variants in the Guess Lexicon should be intentional rather than automatic

Day-one allowed UK spelling guesses:
- fibre
- litre
- metre
- mould
- odour
- sabre

No broad automatic UK-variant import is allowed for launch.

### Variant spelling rule for answers
If a word has multiple common spellings or variant forms:
- prefer the most common US answer form when one exists
- avoid using the word as an answer if the spelling dispute would feel unfair or ambiguous
- allow alternative spelling variants as guesses where that reduces avoidable player frustration

---

## 9. Repeated Letter Evaluation Rules

Repeated letters are a major fairness issue and must be handled exactly.

WordCase should evaluate guesses in two passes.

### Pass 1: Exact-position matches
- mark every letter that matches the answer in the correct position
- each matched letter consumes one occurrence of that letter from the answer’s available count

### Pass 2: Present-elsewhere matches
- for the remaining unmatched guess letters, mark a letter as present-elsewhere only if the answer still has an unused occurrence of that letter available
- each present-elsewhere mark also consumes one occurrence from the remaining answer count

### Extra duplicate letters
- if the guess contains more copies of a letter than the answer actually contains, the extra copies must be marked absent

### Determinism rule
This evaluation must be identical across:
- gameplay code
- tests
- analytics interpretation where relevant
- any future backend verification logic

No screen or mode may use a different repeated-letter algorithm.

---

## 10. Hint and Clue Integrity Rules

Hints must never feel dishonest.

### Hint rules
- hints must be derived from the real answer
- hints must never contradict prior guess feedback
- hints must not point toward a different acceptable word
- hints must not silently change the answer
- hints must not expand the definition of a valid guess

### Positional hint rule
If a hint reveals a letter position:
- that position should be treated as confirmed truth in the current puzzle state
- the UI should present that information clearly and consistently

### Theme clue rule
If a puzzle includes case flavor text, clue cards, or theme clues:
- those clues should support the answer
- those clues should not depend on alternate spellings or obscure definitions to make sense

---

## 11. Daily Publication and Versioning Rules

WordCase daily puzzles must be stable.

Each published daily case should be pinned to:
- a puzzle ID
- an answer word
- an answer length
- a dictionary version
- a feedback algorithm version
- a hint/clue data set if the mode uses them

### Stability rule
Once a daily puzzle is live:
- its answer should not silently change for players who have already started it
- its validation behavior should not drift mid-session

### Dictionary update rule
Future dictionary updates may:
- expand or refine the Guess Lexicon for future puzzles
- refine answer candidate pools for future content

Future dictionary updates must not:
- silently rewrite a completed puzzle result
- change an in-progress player’s answer without an explicit corrective process

### Critical correction rule
If a major content mistake ever forces a correction:
- it must be treated as an explicit hotfix event
- it must be logged and reviewed
- the change should be as limited as possible
- player trust should be prioritized over convenience

---

## 12. Validation Source of Truth

WordCase should validate locally for responsiveness, but validation data still needs a source of truth.

### Canonical editable source files
The canonical editable lexicon source files are:
- `apps/content-tools/data/lexicons/en-US/answer-lexicon-v1.txt`
- `apps/content-tools/data/lexicons/en-US/guess-lexicon-v1.txt`
- `apps/content-tools/data/lexicons/en-US/blocked-terms-v1.txt`
- `apps/content-tools/data/lexicons/en-US/lexicon-manifest-v1.json`

### Generated runtime snapshot
Runtime validation uses the generated snapshot:
- `packages/validation/src/generated/en-US/validation-snapshot-v1.json`

### Lexicon source file format rules
All canonical source lexicon files must be:
- lowercase ASCII
- one word per line
- sorted
- no duplicates
- free of inline comments
- normalized exactly as runtime validation expects

### Local validation
The client should perform local validation for:
- fast input feedback
- offline play on already-downloaded puzzles
- immediate guess acceptance/rejection
- stable resume behavior

### Source-of-truth data
Published puzzle content should come from a trusted content source such as:
- a signed manifest
- bundled approved content
- versioned live content with integrity checks

### Pinned-session rule
An active puzzle session should use the validation snapshot associated with that puzzle’s published version.

This prevents validation drift during a solve.

### Weekly Resolution validation rule
Weekly Resolution uses the same validation snapshot family as the related week’s daily content, including:
- the same `dictionaryVersion`
- the same normalization rules
- the same blocked terms version
- the same duplicate-letter algorithm version

Do not create a special weekly-only validation regime unless a later document explicitly defines it.

---

## 13. Offline Rules

Offline play should feel normal, not broken.

### Allowed offline behavior
If the required puzzle content and dictionary snapshot are already available on device:
- the player should be able to play normally offline
- validation should work exactly the same as online
- solve state should save locally and sync later if needed

### Not allowed offline behavior
If the required puzzle or dictionary data is not present:
- the app should not invent fallback answers
- the app should not guess at validation behavior
- the player should be shown a clean unavailable state instead

---

## 14. Error Messaging Rules

Validation errors should be useful and calm.

Preferred message styles:
- Not enough letters.
- Too many letters.
- Already guessed.
- Not in word list.
- That guess is not allowed.

WordCase should avoid:
- technical jargon
- inconsistent wording for the same problem
- moderation messages that surface harmful language unnecessarily
- vague failure states with no reason

---

## 15. QA and Dispute Review Rules

Dictionary disputes are normal and should be handled intentionally.

### Review rules
- disputed guesses should be loggable for review
- disputed answers should trigger a content review process
- additions/removals to the Guess Lexicon should be tracked
- additions/removals to the Answer Lexicon should be tracked
- moderation list changes should be reviewed carefully

### Human review rule
The product owner or lead content curator is the final authority for Answer Lexicon curation.

No answer enters the Answer Lexicon without human approval.
No daily answer should go live without explicit human approval from the curated answer pool.

Automated filtering may assist.
Automated filtering should not be the final judge of fairness.

### Non-retroactive rule
A future dictionary decision should not casually rewrite the truth of a past published daily puzzle.

---

## 16. Deferred Topics

These topics are intentionally deferred and require a later doc update before implementation:

- multi-word answers
- phrase-level validation
- multilingual support
- community puzzle creation
- user-entered custom dictionaries
- adult/mature language mode
- full region-switching dictionaries
- alternate answer acceptance for special modes

---

## 17. Summary Rule

WordCase should use a broad-enough guess dictionary, a stricter curated answer dictionary, and a deterministic validation algorithm.

The game should feel:
- fair
- stable
- understandable
- hard because of deduction, not because of gotchas

When in doubt, WordCase should prefer:
- player trust
- consistent rules
- common vocabulary
- explicit versioning
- human-reviewed answers

over novelty, obscurity, or clever edge cases.
