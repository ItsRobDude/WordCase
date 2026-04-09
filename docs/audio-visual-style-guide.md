# WordCase Audio-Visual Style Guide

This document defines the visual, motion, haptic, and sound direction for WordCase.

Its purpose is to keep WordCase visually coherent, readable, calm, and premium-feeling while staying practical to build and maintain with the current team and tool reality.

This is a product behavior and presentation document.
It is not just a branding note.

When mockups, generated art, or implementation details disagree with this document, this document should be treated as the intended source of truth for in-app presentation until intentionally updated.

---

## 1. Core Style Philosophy

WordCase should look and feel like:

- a modern noir dossier
- a premium mobile puzzle app
- a calm daily ritual
- a smart investigation surface
- a product that respects concentration

WordCase should **not** look or feel like:

- a neon cyberpunk toy
- a casino app
- a heavy narrative visual novel
- a generic dark skin pasted over a clone
- a loud “gamey” clutter pile

### Primary style rules
- readability beats decoration
- restraint beats spectacle
- one strong visual language beats ten weak ones
- reusable UI systems beat bespoke one-off art
- the main daily puzzle must stay visually dominant
- all meaning must remain clear with sound off and haptics off

---

## 2. Production Reality Rules

This guide should support the actual way WordCase is likely to be built right now.

### Practical production rules
- v1 must **not** depend on custom illustrated characters
- v1 must **not** depend on bespoke scene art for every case
- v1 must **not** depend on advanced After Effects-style motion design
- v1 must **not** depend on advanced sound-design craft to feel good
- the in-app UI should rely on:
  - strong typography
  - strong spacing
  - subtle textures
  - simple iconography
  - restrained motion
  - a small, coherent sound set

### Adobe and AI reality rule
Adobe and AI tools may help create:
- the wordmark/logo
- background textures
- subtle pattern layers
- marketing images
- occasional illustrative promo art later

The app UI itself should **not** require high-skill manual art production to look good.

---

## 3. Exact Palette

WordCase uses a dark dossier palette with one main warm accent and restrained state colors.

### Core neutrals
| Token | Hex | Use |
|---|---:|---|
| `ink-950` | `#0B0E12` | app background, deepest backdrop |
| `ink-900` | `#11161C` | main background surface |
| `surface-850` | `#151C24` | secondary surface |
| `surface-800` | `#1C2530` | cards, sheets, modals |
| `surface-700` | `#243141` | active/elevated surface |
| `border-600` | `#324255` | default borders/dividers |
| `border-500` | `#43566D` | stronger borders/focus outlines |
| `text-100` | `#EAF0F6` | primary text |
| `text-300` | `#B7C2CF` | secondary text |
| `text-500` | `#7E8C9D` | tertiary/muted text |

### Primary accent
| Token | Hex | Use |
|---|---:|---|
| `signal-amber-500` | `#D4A04F` | primary accent, emphasis, selected highlights |
| `signal-amber-300` | `#E7C27D` | softer glow/highlight, selected states |

### Secondary accent
| Token | Hex | Use |
|---|---:|---|
| `signal-blue-500` | `#5F7FA4` | focus states, helpful info, cool balance accent |

### Puzzle state colors
| Token | Hex | Use |
|---|---:|---|
| `confirmed-green-500` | `#3D8C63` | correct letter in correct position |
| `present-amber-500` | `#B98B36` | correct letter, wrong position |
| `ruled-out-slate-500` | `#4E5968` | absent / ruled out |
| `error-red-500` | `#A65454` | destructive/error/warning emphasis only |

### Palette rules
- never use pure black
- never use pure white for large surfaces
- avoid rainbow accents
- avoid neon saturation
- gradients are optional and must remain extremely subtle
- main accent is signal amber, not bright gold or orange
- feedback states must **not** rely on color alone
- if a state is important, pair color with:
  - border change
  - icon or marker
  - tile treatment
  - text/state label where appropriate

### Usage guidance
- daily puzzle board should stay readable on dark surfaces
- the amber accent should feel intentional and rare, not everywhere
- most UI should be neutral, with accent reserved for:
  - primary CTA
  - selected states
  - weekly unlock emphasis
  - key caseboard highlights

---

## 4. Typography Choices

Typography should carry much of the premium feel.
It should not rely on exotic fonts or hard-to-manage branding tricks.

### Official font choices
- **Primary UI font:** `Inter`
- **Secondary mono accent font:** `IBM Plex Mono`

### Why this pair
- both are readable
- both are easy to source and maintain
- both work well on Android
- Inter gives clean premium UI clarity
- IBM Plex Mono adds dossier/signal flavor without requiring a dramatic display font
- this pair is much easier to manage than ornate editorial typography

### Do not use in v1
- decorative serif headlines
- handwritten fonts
- distressed “crime file” fonts
- more than 2 font families

### Typography roles
| Role | Font | Weight | Size / Line Height |
|---|---|---:|---:|
| `display` | Inter | 700 | `28 / 34` |
| `h1` | Inter | 700 | `24 / 30` |
| `h2` | Inter | 600 | `20 / 26` |
| `title` | Inter | 600 | `18 / 24` |
| `body` | Inter | 400 | `16 / 22` |
| `body-strong` | Inter | 500 | `16 / 22` |
| `secondary` | Inter | 400 | `14 / 20` |
| `caption` | Inter | 500 | `12 / 16` |
| `mono-label` | IBM Plex Mono | 500 | `12 / 16` |

### Typography rules
- body text should almost always be `16`
- do not shrink normal UI text below `14` unless it is truly secondary
- use mono only for:
  - metadata labels
  - IDs / case tags / week IDs
  - signal-like micro labels
  - certain timer/status labels
- do not set large paragraphs in mono
- do not overuse all caps
- use letter spacing only lightly and intentionally

---

## 5. Spacing Tokens

WordCase uses a 4-point spacing system.

### Exact spacing tokens
| Token | Value |
|---|---:|
| `space-0` | `0` |
| `space-1` | `4` |
| `space-2` | `8` |
| `space-3` | `12` |
| `space-4` | `16` |
| `space-5` | `20` |
| `space-6` | `24` |
| `space-8` | `32` |
| `space-10` | `40` |
| `space-12` | `48` |
| `space-16` | `64` |

### Default usage
- screen horizontal gutter: `16`
- larger screen gutter where needed: `20`
- card padding: `16`
- compact card padding: `12`
- stack gap between related controls: `12`
- major section spacing: `24`
- large section break: `32`

### Border radius tokens
| Token | Value |
|---|---:|
| `radius-sm` | `8` |
| `radius-md` | `12` |
| `radius-lg` | `16` |
| `radius-pill` | `999` |

### Spacing rules
- use the token system consistently
- do not invent random 7px / 13px / 19px spacing all over the app
- the daily screen should feel deliberate and breathable, not cramped
- cards and sheets should feel like reused systems, not custom layouts every time

---

## 6. Icon Style

Because of production constraints and Android-first priorities, the icon system should be simple and native-feeling.

### Official icon style
- **Material Symbols Rounded** style
- rounded outline-first icons
- monochrome only
- 24dp standard icon size
- 20dp only in dense secondary UI
- 28dp only for major hero/status moments

### Icon rules
- no multicolor icons
- no gradients inside icons
- no skeuomorphic icon illustration
- no icon family mixing in the same product surface
- active state should usually come from:
  - color
  - container fill
  - emphasis
  - not from switching into a completely different visual language

### Why this choice
- easiest to implement consistently
- easiest for Codex/Jules to use
- easiest to maintain
- feels at home on Android
- removes the need for custom icon production in v1

### Icon behavior guidance
Use icons to support, not replace, text.
Avoid icon-only ambiguity in important flows.

---

## 7. Surface Style and Reusable Motifs

### Reusable in-app motifs
The app should reuse a very small motif set:
- dossier cards
- subtle paper-like or scanline texture
- signal/wave line motif
- evidence markers
- restrained divider rules
- amber highlight edge or tab marker

### What v1 should avoid
- large custom illustrations in the puzzle flow
- per-case custom backgrounds
- dramatic cinematic overlays
- messy collage aesthetics
- fake “aged paper” grime that hurts readability

### Surface rules
- cards should feel matte, not glossy
- shadows should be subtle
- borders matter more than heavy shadows
- texture should be subtle enough to disappear if the user is focusing on the puzzle

---

## 8. Motion Rules

Motion should clarify state changes and reward important moments.
It should never slow the repeat-use daily loop.

### Exact motion durations
| Motion type | Duration |
|---|---:|
| `motion-micro` | `90ms` |
| `motion-standard` | `160ms` |
| `motion-emphasis` | `220ms` |
| `motion-sheet` | `280ms` |
| `motion-celebration-max` | `320ms` |

### Easing rules
- enter: ease-out
- exit: ease-in
- state change: ease-in-out

### Allowed motion types
- opacity fade
- small translate Y (`4–12dp`)
- subtle scale (`0.98 -> 1.00`)
- border/background emphasis changes
- board tile state transitions

### Forbidden or strongly discouraged motion
- bounce
- spin
- parallax
- confetti explosions
- looping idle motion on the main daily puzzle screen
- dramatic camera-like zooms
- long transition sequences before the player can interact again

### Motion rules for core surfaces
- Home should feel quick and stable
- Daily Case should feel almost static except where state truly changes
- Results should have a short clean reveal, not a big show
- Weekly unlock may be slightly more expressive than solve, but still restrained

### Reduced motion
If reduced motion is enabled:
- prefer opacity-only transitions
- avoid scale
- avoid larger slide distances
- keep functional clarity identical

---

## 9. Haptic Rules

Haptics are additive.
They should help the app feel tangible, but they must never become noisy or fatiguing.

### Core haptic rules
- obey system settings
- obey in-app haptics toggle
- never make haptics the only source of meaning
- do not vibrate on every tiny state change
- no looping or repeated “buzzing” patterns

### Exact haptic mapping
| Event | Haptic |
|---|---|
| tap on major control / tab / CTA | selection haptic |
| letter key tap | **none by default in v1** |
| invalid guess | subtle warning pattern or two light pulses |
| valid submit | light impact |
| solve | success notification + one medium impact |
| fail | warning notification, softer than solve |
| weekly unlock | success notification + medium impact + light follow-up pulse |

### Why no per-letter haptic by default
- reduces fatigue
- reduces battery drain
- keeps the main input loop quieter
- sound already covers the light-input confirmation role well enough

This is the better balance for a daily puzzle app.

---

## 10. Sound Scope for v1

### V1 sound scope
For the first real product slice, WordCase should rely primarily on:
- core UI sound effects
- optional subtle title/home ambience later if it genuinely helps

The Daily Case should **not** depend on a constant music loop to feel complete.

That means:
- strong SFX is required
- background music is optional in v1
- ship should not be blocked on composing an original soundtrack

---

## 11. Exact Sound Cues

All WordCase sounds should feel:
- clean
- restrained
- slightly analog
- slightly dossier/radio-adjacent
- never arcade-loud
- never casino-cheery

### 11.1 Tap
**Purpose:** lightweight confirmation for major taps and clean UI input

**Cue:**
- short soft tick / paper-button click
- duration: `20–35ms`
- neutral or slightly warm tone
- low prominence

### 11.2 Invalid guess
**Purpose:** communicate “no” without shaming the player

**Cue:**
- muted low double click or soft buzz-like reject
- duration: `70–100ms`
- lower and duller than valid submit
- should feel corrective, not punishing

### 11.3 Valid submit
**Purpose:** confirm a real guess was committed

**Cue:**
- firmer relay-like click with soft confirmation tail
- duration: `60–90ms`
- cleaner and more confident than tap
- should feel mechanical and exact

### 11.4 Solve
**Purpose:** reward competence with clear closure

**Cue:**
- warm ascending 3-note stinger
- duration: `450–700ms`
- slightly richer than ordinary UI sounds
- should feel satisfying, not explosive

### 11.5 Fail
**Purpose:** acknowledge failure clearly without humiliation

**Cue:**
- short muted descending tone or soft low thud
- duration: `250–400ms`
- calmer and less dramatic than solve
- should feel conclusive, not punitive

### 11.6 Weekly unlock
**Purpose:** mark a bigger payoff than a daily solve

**Cue:**
- broader warm reveal tone with a slightly longer rise
- duration: `700–1100ms`
- more expansive than solve
- still restrained and premium
- should feel like meaningful progression, not jackpot theatrics

### Sound mix rules
- no cue should be harsh or piercing
- no cue should sound like an arcade machine
- solve and weekly unlock should be the richest cues
- invalid/fail should stay calmer and lower-energy
- do not use a melody on every tiny interaction

---

## 12. Feedback Pairing Rules

WordCase should pair channels intentionally:

| Event | Visual | Sound | Haptic |
|---|---|---|---|
| tap | button/container state change | tap cue | selection |
| invalid guess | inline/toast + field state | invalid cue | warning light |
| valid submit | board/row commit | valid submit cue | light impact |
| solve | result reveal + board confirmation | solve cue | success + medium |
| fail | result reveal + answer reveal | fail cue | warning |
| weekly unlock | board unlock emphasis | weekly unlock cue | success + follow-up |

### Important rule
Any one of the channels may be disabled.
The meaning must still remain clear.

---

## 13. Accessibility Guardrails

This style guide must support accessibility by default.

### Required rules
- all important text must remain readable at larger text sizes
- feedback states must not rely on color alone
- daily board must remain understandable with sound off
- daily board must remain understandable with haptics off
- contrast must stay strong enough on dark surfaces
- clue/frame text must remain short and readable
- motion must never be required to understand gameplay state

---

## 14. Asset Production Rules for the Current Team Reality

### In-app asset policy for v1
Do not require:
- custom character art
- bespoke illustration for each daily case
- complex particle systems
- high-skill vector drawing
- advanced sound editing workflows

### Allowed and recommended v1 asset sources
- AI-assisted texture generation
- AI-assisted logo exploration
- Photoshop cleanup of textures/backgrounds
- simple reusable overlays
- built-in icon family
- generated/simple UI cues for sound

### Adobe reality rule
Use Adobe tools where they make life easier:
- **Firefly / Boards** for mood and texture exploration
- **Photoshop** for cleanup and export
- **Express** for promo/layout assets
- **Audition** only if needed for trimming or leveling sounds, not for building a big sound-design pipeline
- **Illustrator** is optional, not required for v1 icon production

### The in-app UI must still look good even if:
- you do not draw custom icons
- you do not paint custom art
- you do not hand-design complex animations
- you keep the app mostly typography + surfaces + subtle motifs

That is intentional.
It is the correct constraint for WordCase right now.

---

## 15. Practical Summary

WordCase presentation should follow this standard:

- dark dossier palette
- warm amber accent
- Inter + IBM Plex Mono only
- Material Symbols Rounded style icons
- 4-point spacing system
- restrained motion
- quiet but satisfying haptics
- six core SFX cues only
- no heavy bespoke art dependency
- no flashy spectacle in the core daily flow
- readability and trust first

If WordCase follows this style guide, it can look distinctive and premium **without** requiring a high-skill design pipeline to survive.
