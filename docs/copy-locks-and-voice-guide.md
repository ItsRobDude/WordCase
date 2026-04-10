# WordCase Copy Locks and Voice Guide

This document is the authoritative source for shared player-facing semantic strings in WordCase.
Its purpose is to separate meaning from tone so that interaction truth remains consistent without preventing flavor variations.

---

## 1. Locked vs. Flexible Copy

WordCase uses two classes of strings: **Locked** strings and **Flexible** strings.

### Locked Strings
Any text that affects player understanding of rules, progress, rewards, failure, or availability is **exact copy** and must be implemented verbatim.

This includes:
- rules
- onboarding instructions
- validation/error messages
- state transitions
- reward explanations
- streak messaging
- fail/success outcomes
- economy and gating text

### Flexible Strings
Flavor copy may vary within the voice guide as long as semantic meaning is unchanged.

This includes:
- celebratory flavor
- atmospheric lines
- optional banter
- rotating non-instructional headers

---

## 2. Voice Guidelines for Flexible Copy

Flexible copy must align with the WordCase voice:
- **Sharp and calm**: Do not sound like a noisy casino or a frantic host.
- **Fair and respectful**: Respect the player's intelligence. Do not patronize or belittle them for making a mistake.
- **Atmospheric**: Lean into the mystery, case-file, and detective theme without burying the puzzle in excessive lore.

---

## 3. Locked Copy Table (Appendix)

This table defines the mandatory strings that must be implemented exactly as written.

| String ID | Exact Text | Variables | Usage Context | Notes |
| :--- | :--- | :--- | :--- | :--- |
| `rule_hint_locked_letter` | Hint-locked letter cannot be changed. | None | Displayed when a user attempts to edit a letter fixed by a hint. | Shown as a non-blocking toast. |
| `action_abandon_carryover` | Abandon Carryover Case | None | Pause overlay menu item | Only shown for an unresolved Protected Carryover. |
| `dialog_abandon_title` | Abandon this carryover case? | None | Title of the abandon confirmation dialog. | |
| `dialog_abandon_body` | This will mark yesterday's carryover case as missed and cannot be undone. Today's case will become your active daily. | None | Body text of the abandon confirmation dialog. | |
| `action_mark_missed` | Mark Missed | None | Primary destructive button in abandon dialog. | |
| `action_keep_solving` | Keep Solving | None | Secondary safe button in abandon dialog. | |
| `result_solved_assisted` | Solved (Assisted) | None | Result screen class tag | Used when a hint was used. |
| `result_solved_unassisted`| Solved (Unassisted) | None | Result screen class tag | Used when no hint was used. |
| `cta_view_weekly_evidence`| View Weekly Evidence | None | Primary CTA on result screen | Used when the weekly board changed meaningfully. |
| `cta_return_home` | Return Home | None | Primary CTA on result screen | Used when weekly board did not change meaningfully. |
| `cta_share_result` | Share Result | None | Secondary CTA on result screen | |
| `cta_retry_weekly` | Retry Weekly Resolution | None | Primary CTA for a failed weekly run | |
| `home_label_todays_case` | Today's Case | None | Label for the primary case card | |
| `home_label_weekly_case` | This Week's Case | None | Label for the weekly case card | |
| `home_label_archive` | Archive | None | Label for the archive entry card | |
| `cta_start_case` | Start Today's Case | None | Button to begin the daily case | |
| `cta_continue_case` | Continue Case | None | Button to resume an active daily case | |
| `cta_review_result` | Review Result | None | Button to view a failed case result | |
| `cta_browse_archive` | Browse Archive | None | Button to enter the archive | |
| `case_meta_5_letters` | 5 Letters | None | Pre-start case info | |
| `case_meta_6_attempts` | 6 Attempts | None | Pre-start case info | |
| `link_how_feedback_works` | How feedback works | None | Link shown to new players | Surfaced for the first 3 live daily cases. |

*Note: More locked strings will be added to this table as implementation proceeds.*
