# WordCase Design Clarifications and Edge Cases

This document records resolutions to identified edge cases, inconsistencies, and implementation ambiguities in the WordCase ruleset. It acts as an addendum and authoritative patch to the original design documents.

## 1. Hint Selection and Already Guessed Letters
**Context:** The hint algorithm scans left-to-right for the first unrevealed position. What if a player has already correctly guessed a letter in its exact position (a "Confirmed" green match), but didn't use a hint on it?
**Resolution:**
The hint will **not** be wasted on an already correctly guessed and confirmed position. The algorithm will scan from left to right and skip any position that is already confirmed (green) from a prior valid guess, revealing the first position that the player has not yet solved.

## 2. Late Weekly Resolution Unlock (Carryover Edge Case)
**Context:** A player finishes Sunday with 3 evidence fragments. On Monday (after weekly rollover), they solve their Sunday Protected Carryover Daily, earning their 4th fragment for the *prior* week.
**Resolution:**
We will provide a **short grace window** (e.g., 24 hours) after the weekly rollover for the player to play the late-unlocked Weekly Resolution for the previous week.
*Future Consideration:* In later phases, this window could be extended via a microtransaction or special item to allow players to go back and complete missed Weekly Resolutions.

## 3. Offline Time Verification vs. Local Clock
**Context:** The game relies on authoritative server UTC time to prevent cheating, but offline play is allowed when content is cached.
**Resolution:**
To verify time while offline without blindly trusting the local device clock:
*   **Technical Implementation:** When the game successfully syncs with the server, it records a `serverTimeOffset` (`serverTimeOffset = server_UTC_timestamp - local_device_timestamp`).
*   During offline play, the game calculates current authoritative time as `current_local_device_timestamp + serverTimeOffset`.
*   If the user manually changes their device clock while offline, the offset ensures the calculated time remains relative to the last known honest server sync, preventing them from artificially triggering daily rollovers or restoring expired dailies.
*   If the local clock diverges massively from reasonable uptime (e.g., jumps forward 5 years), the offline session should invalidate and demand a server sync before continuing canonical play.

## 4. Starter Case Constraints
**Context:** The starter case needs to be simpler to ensure a fast first win. Should it break the core rules (e.g., use 4-letter words or unlimited attempts)?
**Resolution:**
The starter case will **strictly adhere to the basic rules** of the game. It will feature a 5-letter word and a 6-attempt limit. The "simpler" aspect will come from using extremely common vocabulary, a very strong clue, and a guided UI tutorial overlay, rather than changing the mechanical constraints of the engine.

## 5. First-Day Missed Puzzles (Grace Period)
**Context:** If a player installs the game and finishes the starter case 5 minutes before midnight UTC, they might immediately incur a "Missed" daily case if they don't submit a guess in time, which is a punishing first experience.
**Resolution:**
We will grant a **First-Day Grace Period** for new players.
*   **In Play:** A new player will not have their very first Daily Case pulled away from them immediately at midnight if they just started it. They will have a fair amount of time to complete their first real puzzle.
*   **Technical Implementation:** The game will automatically grant **"Protected Carryover Daily"** status to the player's *first* Daily Case upon opening it, even if they have submitted 0 valid guesses. (Normally, a Protected Carryover requires at least 1 valid guess). This ensures that when the midnight UTC rollover happens shortly after install, their active case safely carries over into their second day until they resolve it.
