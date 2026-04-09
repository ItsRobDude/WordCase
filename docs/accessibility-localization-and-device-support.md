# WordCase Accessibility, Localization, and Device Support

This document defines the accessibility, localization, and device-support rules for WordCase.

Its purpose is to keep WordCase readable, usable, and trustworthy across ordinary Android devices while staying practical to build and maintain.

This is a product behavior and support document.
It is not just a checklist.

When implementation details or mockups disagree with this document, this document should be treated as the source of truth for player-facing accessibility, language, and device-support behavior until intentionally updated.

---

## 1. Core Philosophy

WordCase should be usable by ordinary players in ordinary real-life conditions.

That means the app should remain understandable when:
- the player is using one hand
- the phone screen is not huge
- the player has sound off
- the player has haptics off
- the player uses larger text
- the player is interrupted and resumes
- the player needs higher-contrast or color-independent state cues

WordCase should not rely on:
- perfect vision
- tiny text
- fast twitch input
- loud audio
- hidden color-only meaning
- complex gestures
- landscape-only layouts
- premium/flagship phones only

Accessibility in WordCase is part of product quality, not an optional bonus.

---

## 2. Scope

This document defines:
- minimum Android support assumptions
- device/layout expectations
- large text behavior
- color-independent feedback rules
- screen reader goals
- physical keyboard support
- orientation policy
- language/localization policy
- how non-US spelling support is presented
- practical QA expectations for these areas

This document does not define:
- exact puzzle rules
- exact content pipeline schemas
- exact audio design details
- exact visual palette/tokens

Those belong in other docs.

---

## 3. Minimum Android Version and Device Assumptions

### Minimum Android version / device assumptions
For v1, WordCase should target:

- **minimum supported Android version:** **Android 10 (API 29)**
- **recommended Android version:** Android 12 or newer
- **primary target device type:** ordinary portrait phone
- **minimum practical screen class:** roughly modern 720p-class phones and above
- **recommended RAM target:** 4 GB or better for a comfortable experience

### Why this support floor
This is the best balance between:
- real user reach
- manageable QA surface
- lower layout/device weirdness
- lower maintenance burden for a small AI-assisted team

### V1 device assumptions
WordCase is designed first for:
- portrait phone use
- one-handed use
- short sessions
- touch-first interaction

V1 is **not** required to ship:
- a custom tablet-first layout
- foldable-specific layouts
- Chromebook-optimized layouts
- landscape-optimized gameplay layouts

### Large-screen rule
If the app runs on a tablet or other large Android screen in v1:
- it should remain functional
- content should stay centered and readable
- the UI should not stretch into a fake tablet dashboard
- large-screen polish may come later

### Future-proofing rule
If Expo/platform requirements later force a higher Android floor, the doc should be updated before the product behavior silently drifts.

---

## 4. Accessibility Defaults

WordCase should default to accessible-enough behavior before the player opens settings.

### Default baseline rules
- body text should be readable at normal phone distance
- touch targets should be comfortably tappable
- color should never be the only important state signal
- sound should never be the only important state signal
- haptics should never be the only important state signal
- motion should remain restrained by default
- the main daily board should remain visually dominant and stable

### Quick access rule
The first-launch flow may offer lightweight accessibility quick picks:
- larger text
- reduced motion
- haptics off
- sound off

Do not bury those behind several settings menus before the player can even start.

---

## 5. Large Text Behavior

### Large text behavior
WordCase should respect Android/system font scaling and provide a usable experience at larger text sizes.

### Required support target
The app should fully support:
- **system font scaling up to 130%**

The app should still remain usable beyond that where practical, but 130% is the required layout target for v1.

### Rules for large text
- critical labels must remain visible or wrap cleanly
- buttons must not clip text
- important CTAs must remain tappable
- case title / frame / clue blocks may wrap to additional lines
- the daily board must remain usable without collapsing into unreadable tiles
- if necessary, secondary metadata may wrap or reduce density before primary puzzle controls break

### What must not happen
- the main Start / Continue / Submit flow must not become blocked by text overflow
- result screens must not hide primary actions off-screen with no scroll
- the settings screen must not become unusable
- text enlargement must not make the puzzle board impossible to interpret

### Daily board scaling rule
For v1:
- letter tiles may use controlled scaling rather than unconstrained text scaling
- the board must preserve clarity and spacing first
- if system text scaling is very large, explanatory text may grow more than tile letters

This is a practical compromise and the right one for a board-based puzzle UI.

---

## 6. Colorblind-Safe Feedback Rules

### Colorblind-safe feedback rules
WordCase must not rely on color alone to communicate puzzle meaning.

This applies especially to:
- confirmed letters
- present-elsewhere letters
- ruled-out letters
- important warning/error states

### Default rule
Color-independent cues should be present in the core puzzle UI by default.
Do **not** hide them behind a special colorblind mode if that can be avoided.

### Required non-color cues
For v1, each important puzzle state should use at least one additional cue beyond color, such as:
- border treatment
- marker shape
- icon/indicator
- tile pattern treatment
- text/state legend where appropriate

### Recommended state cue system
For the core puzzle board and legend:
- **Confirmed:** distinct solid state + strong border treatment + dedicated marker
- **Present Elsewhere:** distinct alternate border/marker treatment clearly different from Confirmed
- **Ruled Out:** clearly muted state + separate non-color indicator, not just gray

The exact visual marker shape can be defined by implementation, but the state differences must remain obvious without depending only on hue perception.

### Keyboard rule
Keyboard/key state feedback must also avoid color-only meaning.
If a key reflects state, that state must still remain distinguishable through:
- outline
- icon/marker
- or another non-color cue

### Help rule
The How Feedback Works surface must show the state legend using:
- labels
- color
- and non-color cues together

---

## 7. Screen Reader Goals

### Screen reader goals
WordCase should aim for **functional end-to-end TalkBack usability for the core daily flow** in v1.

That means a screen-reader user should be able to:
- complete first launch
- complete the starter case
- navigate Home
- open the Daily Case
- enter guesses
- use hint/help
- submit guesses
- hear result state
- return to Home/next action

### Required screen reader behavior
- interactive controls must have clear accessible labels
- decorative elements must not steal focus
- focus order must follow a logical top-to-bottom reading path
- the active screen purpose should be obvious
- buttons must be labeled by action, not by vague icon name only
- invalid guess feedback should be announced clearly
- result state should be announced clearly
- state changes should not rely on visual-only transitions

### Daily board accessibility goal
The daily board does not need to feel ultra-fast under TalkBack in v1, but it must be understandable and usable.

At minimum:
- submitted guesses should be readable
- each guess row should expose a readable summary
- tile state meaning should be available through accessible text
- submit/delete/hint controls must be labeled clearly

### Avoid
- unlabeled icon buttons
- duplicate focus stops with no value
- hidden content that becomes focusable
- announcing decorative textures or separators as meaningful content

---

## 8. Physical Keyboard Support

### Physical keyboard support
WordCase should support physical keyboard input for the core puzzle flow in v1.

This applies to:
- Bluetooth keyboards
- attached Android keyboards
- Chromebook-like setups where supported by the platform

### Required key behavior
For the Daily Case and compatible modes:
- `A–Z` enters letters
- `Backspace/Delete` removes letters
- `Enter/Return` submits a guess
- repeated invalid submissions should still follow the same validation rules as touch input

### Optional but nice later
- arrow key navigation between fields is not required
- advanced desktop-like shortcuts are not required
- full keyboard-only UI navigation beyond standard Android focus behavior is not required in v1

### Important rule
Physical keyboard support is additive.
The app must remain fully playable touch-first.

---

## 9. Orientation Policy

### Orientation policy
WordCase is **portrait-first** and **portrait-only in v1**.

### Why
This matches:
- one-handed use
- daily ritual usage
- phone-first layout
- reduced layout complexity
- lower QA burden for a small team

### Rules
- the main gameplay experience is designed for portrait
- the app should not require landscape
- the app should not introduce alternate landscape puzzle layouts in v1
- tablets/large screens may still run the app, but portrait remains the intended primary layout behavior

### Foldables / large devices
For v1:
- no foldable-specific layout work is required
- no multi-pane tablet layout is required
- portrait-constrained content is acceptable if the app remains readable and functional

---

## 10. Localization Policy

### English-only UI for v1?
**Yes. WordCase is English-only in v1.**

That means:
- UI copy is English-only
- onboarding/help text is English-only
- dictionary/help explanations are English-only
- the game’s answer dictionary is American English-primary
- no UI language switcher in v1

### Why
This is the correct v1 scope for:
- a small AI-assisted team
- puzzle trust
- consistent validation
- simpler QA
- avoiding half-supported language/localization systems

### Important rule
English-only UI does **not** mean sloppy English copy is acceptable.
Clarity still matters.

---

## 11. How Non-US Spelling Support Appears in UI / Help

### Non-US spelling support in UI/help
WordCase should explain this simply and only where relevant.

### Product rule
For v1:
- official daily answers use **American English** forms
- selected non-US spelling variants may be accepted as **valid guesses only**
- non-US variants are not presented as a separate language mode
- no UK-English UI mode exists in v1

### Where this appears
This should appear in:
- Help / How to Play
- Dictionary / validation info if that help surface exists
- possibly a small note in Settings under gameplay/help info

### Recommended help wording
Use something close to:

> WordCase uses American English answer forms in v1.  
> Some common non-US spelling variants may still be accepted as guesses, but official daily answers use the American English form.

### What should not happen
- do not interrupt the player mid-puzzle with regional spelling popups
- do not create a big localization/settings panel for this in v1
- do not auto-correct the player’s spelling silently
- do not lecture the player when a non-US spelling is accepted or rejected

Keep it calm and documented.

---

## 12. Practical QA Expectations

This document should create real QA expectations, not just ideals.

### Manual checks required before calling the app acceptable in these areas
- test large text at the required target scale
- test TalkBack through the main daily flow
- test sound-off usability
- test haptics-off usability
- test portrait use on an ordinary Android phone
- test physical keyboard entry for the Daily Case
- test that feedback states remain distinguishable without relying only on color
- test that help surfaces explain spelling/feedback rules clearly enough

### Release rule
If any of these areas are obviously broken in the core daily flow, the app should not be treated as polished enough.

---

## 13. Practical Summary

WordCase accessibility, localization, and device support should follow this standard:

- Android-first
- minimum support target: Android 10
- portrait-only in v1
- ordinary phone-first layout
- large text support targeted through 130% scaling
- color-independent feedback cues required
- TalkBack should be able to complete the core daily flow
- physical keyboard support for core puzzle entry is required
- English-only UI in v1
- selected non-US spellings may be accepted as guesses, explained calmly in help

If WordCase follows this guide, it can be meaningfully accessible and realistic to maintain without pretending to support more than the project can honestly deliver.
