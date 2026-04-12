# PROJECT ORION — UI & GAME STRUCTURE
### Genre Definition, Menu Flow, and Incursion System
*Version: 0.1 | Updated: 2026-03-29 | Status: Active*

---

## STATUS KEY

| Label | Meaning |
|---|---|
| LOCKED | Finalized. Do not revise without explicit design session. |
| PROPOSED | Under discussion. Pending confirmation. |
| DERIVED | Inferred from other locked documentation. |
| UNDEFINED | Known gap. Blocks dependent systems. |

---

## TABLE OF CONTENTS

1. [Genre Definition](#1-genre-definition)
2. [Platform & Controls](#2-platform--controls)
3. [Main Menu](#3-main-menu)
4. [New Game Flow](#4-new-game-flow)
5. [Load Game Flow](#5-load-game-flow)
6. [Æther Incursion System](#6-æther-incursion-system)
7. [Queued Modes](#7-queued-modes)
8. [Open Decisions](#8-open-decisions)

---

## 1. GENRE DEFINITION

*LOCKED*

Project Orion is a **persistent MOBA-inspired action game.**

### Reference Points

| Reference | What It Contributes |
|---|---|
| Vainglory | UI layout, mobile control model, MOBA readability |
| DOTA 2 | UI hierarchy, deliberate control feel, ability weight |
| Guild Wars 2 | Action combat logic, positional play, skill expression |
| Dark Souls | Weapon identity philosophy, meaningful engagement weight |

### What "Persistent MOBA-Inspired" Means

- Fixed isometric perspective
- MOBA-style UI — ability bar, minimap, clear unit state indicators
- Deliberate control feel — every action has readable weight
- Action combat pacing — marginally slower than GW2, not turn-based
- No session resets — the world persists, progression persists
- Story and authored PvE content — not procedural, not run-based
- PvPvE as the persistent post-Belldor state — not an extraction mode

### What It Is Not

- Not a roguelike — no run resets, no random loadout generation
- Not a traditional MMO — no stat inflation, no gear treadmill
- Not a battle royale — no shrinking zones, no last-player-standing
- Not a pure MOBA — no lanes, no base destruction as primary objective

---

## 2. PLATFORM & CONTROLS

*LOCKED*

| Field | Value |
|---|---|
| Primary platform | Mobile — Samsung S24+ and equivalent |
| Secondary platform | PC |
| Perspective | Fixed isometric |
| Control reference | Vainglory — mobile MOBA control model |
| Engine | Babylon.js |

Touch and mouse/keyboard controls share the same input model. No platform-exclusive mechanics.

---

## 3. MAIN MENU

*LOCKED — structure. PROPOSED — exact labels and ordering.*

Simple list layout. No splash animations blocking access. Clear hierarchy.

| Entry | Function |
|---|---|
| New Game | Opens trait line selection → name entry |
| Load Game | Opens character select screen |
| Settings | Opens settings overlay |
| [Additional entries] | UNDEFINED — pending OD-UI-01 |

---

## 4. NEW GAME FLOW

*LOCKED — sequence. PROPOSED — preview content per trait line.*

### Step 1 — Trait Line Selection

All six trait lines presented simultaneously as preview cards.
Player can browse all before selecting.

| Trait Line | Reactor | Default Skill 0 |
|---|---|---|
| Warrior | Ichor | Impact |
| Marauder | Ichor | Impact |
| Guardian | Mana | Pierce |
| Cleric | Mana | Pierce |
| Rogue | Ether | Blast |
| Wizard | Ether | Blast |

Preview card content: UNDEFINED — pending OD-UI-02.

### Step 2 — Name Entry

Text entry for pilot name. Follows trait line selection.
Validation rules: UNDEFINED — pending OD-UI-03.

### Step 3 — Game Start

Player enters Belldor starting zone (M00-THE-BOG) at Level 1 with default loadout for selected trait line.

---

## 5. LOAD GAME FLOW

*LOCKED — structure. PROPOSED — display fields per character entry.*

Character select screen. Lists existing pilots.
Display fields per entry: UNDEFINED — pending OD-UI-04.

---

## 6. ÆTHER INCURSION SYSTEM

*LOCKED*

### Lore Basis

Pilots are æther-immune (codex.md §V). Mechs can be projected across the æther into other worlds. This is not a game-convention shortcut — it is a mechanically coherent extension of established lore.

### Incursion State

Player-controlled binary toggle, available post-first-incursion.

```
incursion_state: enum [on, off]
```

**ON:**
- Player is eligible to be selected as a defender at any time
- Player can queue as an attacker
- All incursion content accessible

**OFF:**
- All incursion activity disabled
- Player cannot be selected as defender
- Player cannot queue as attacker
- Cosmetic incursion content locked
- No mechanical penalty
- No stat disadvantage
- No matchmaking impact on other players

### First Incursion — Consent Event

*LOCKED*

The first incursion is opt-in. Triggered at a specific location post-Belldor. This is the consent moment — after this event, incursion_state defaults to ON and the player enters the incursion ecosystem.

Location: UNDEFINED — pending OD-UI-05.

### Roles

**Attacker**
- Active queue entry required
- Projected into another player's active world
- Intent: harm the world's primary player

**Defender**
- No queue entry required
- Selected by system from eligible active players
- Projected into another player's active world
- Intent: assist the world's primary player against active attackers
- Attacker queue entry overrides defender eligibility — queuing to attack removes the player from the defender pool for that session

**Primary Player**
- The player whose world is being entered
- Not a queued role — they are simply playing
- Defenders are supplementary to them, not replacements

### Matchmaking

*LOCKED — model. PROPOSED — weighting values.*

One queue: the attacker queue. No separate defender queue.

```
Attacker side:  assembled from attacker queue (solo + parties)
Defender side:  selected by system from eligible active players
                assembled to balance attacker side by skill and count

Balance inputs: incursion history + PvE progression (dual signal)
Constraints:    max party size 3
                max 4 parties total per matchup (12 players maximum)
```

Matchmaking is skill-primary, player-count-secondary. The system is mathematically capable of producing asymmetric matchups (e.g. 1v11) in extreme population or skill distribution cases. This is expected behavior, not a failure state.

**Reference case (locked):**
Party of 2 attackers + solo queue attacker matched against party of 3 defenders = 3v3. System assembled both sides.

**Asymmetric skill handling:**
If attacker skill significantly exceeds available single-defender equivalents, the system adds defenders to cover the gap. Defenders fill both skill gaps and player count gaps in a single matchmaking pass.

### Incursion Content Exclusion

Players with incursion_state OFF are informed at the toggle point of what content they cannot access. Excluded content is cosmetic only. This is communicated clearly and without penalty framing.

---

## 7. QUEUED MODES

*LOCKED — structure. PROPOSED — specific mode rules.*

Queued PvP modes are unlocked by exploration, not progression level. Discovery gates access.

| Mode | Format | Unlock Method |
|---|---|---|
| 3v3 | Earliest available | Exploration — location UNDEFINED |
| 5v5 | Earliest available | Exploration — location UNDEFINED |
| [Additional modes] | UNDEFINED | UNDEFINED |

Server architecture: UNDEFINED — global server intent noted, feasibility unconfirmed. Pending OD-UI-06.

---

## OPEN DECISIONS

| # | Decision | Blocks |
|---|---|---|
| OD-UI-01 | Additional main menu entries — credits, social, server select, etc. | Main menu implementation |
| OD-UI-02 | Trait line preview card content — art, stat summary, playstyle blurb | New game flow implementation |
| OD-UI-03 | Pilot name validation rules — character limit, forbidden strings | New game flow implementation |
| OD-UI-04 | Load game character entry display fields | Load game implementation |
| OD-UI-05 | First incursion trigger location — which zone, post-Belldor | Incursion system implementation |
| OD-UI-06 | Server architecture — global single server feasibility | Matchmaking implementation |
| OD-UI-07 | 3v3 and 5v5 unlock locations | Queued mode implementation |
| OD-UI-08 | Win/loss conditions for incursions and queued modes | All PvP implementation |
| OD-UI-09 | Belldor protection — can new players be invaded before first trigger event | Incursion system implementation |
| OD-UI-10 | HUD layout — ability bar position, minimap corner, health layer indicators | In-game UI implementation |
| OD-UI-11 | Incursion cosmetic content — what specifically is locked behind incursion_state ON | Reward system design |

---

*ui-v0.md | Project Orion internal documentation*
*Version 0.1 — Genre definition. Main menu and new game flow. Æther Incursion system. Queued modes stub.*
