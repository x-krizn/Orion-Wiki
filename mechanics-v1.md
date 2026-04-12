# PROJECT ORION — COMBAT MECHANICS
### Weapon Systems, Action Schema, and Cooldown Reference
*Version: 1.3 | Updated: 2026-04-11 | Status: Active*
*Expanded by: SystemRevision001.md, Session 2026-04-11*

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

1. [Core Concepts](#1-core-concepts)
2. [Action System](#2-action-system)
3. [Skill System](#3-skill-system)
4. [Reload System](#4-reload-system)
5. [Weapon Swap](#5-weapon-swap)
6. [Cooldown System](#6-cooldown-system)
7. [Stealth Rules](#7-stealth-rules)
8. [CC Duration Table](#8-cc-duration-table)
9. [Base Combat Unit](#9-base-combat-unit)
10. [Reactor Energy Principle](#10-reactor-energy-principle)
11. [Weapon Class — Boltcaster](#11-weapon-class--boltcaster)
12. [Weapon Class Taxonomy](#12-weapon-class-taxonomy)
13. [Action Bar Taxonomy](#13-action-bar-taxonomy)
14. [Slot Taxonomy](#14-slot-taxonomy)
15. [Weapon Set Rules](#15-weapon-set-rules)
16. [Twin-Link Rules](#16-twin-link-rules)
17. [Cross-Class Link Rules](#17-cross-class-link-rules)
18. [Aux Weapon Rules](#18-aux-weapon-rules)
19. [Frame Critical State](#19-frame-critical-state)
20. [Booster Kit — Evade System](#20-booster-kit--evade-system)
21. [Weapon Definition Schema](#21-weapon-definition-schema)
22. [Reference Example — Gladius](#22-reference-example--gladius)

---

## 1. CORE CONCEPTS

*LOCKED*

### Action
The atomic unit of a moveset. Has a base duration and a typed payload. Never modified at the definition level — modifications are applied at the skill slot reference level, leaving the source action unchanged.

### Action Modifier
Applied to a skill slot's action reference, not the action definition itself. Controls duration override, repeat count, and timing split. The source action is always preserved.

### Library
An optional pool of swappable action variants scoped to a weapon. Used when a skill needs dynamic or replaceable actions. If no library is referenced, actions in the skill sequence are inline and fixed.

### Skill
One of five slots per weapon set, bound to actions 1-5 on the action bar.
- **Skill 0 / Action 1** — Auto-attack. Single or multi-action chain. Repeatable.
- **Skills 1-4 / Actions 2-5** — Unique, weapon-specific. Variable type and behavior.

### Moveset
The complete set of five actions for a weapon set. Defines the weapon set's combat identity. Skills can be altered by weapon modifications.

### Cooldown
A timed lockout on a specific slot, state, or system. Governed by a strict isolation rule — see §6.

### Repair = Heal
For machine actors (mechs), repair and heal are mechanically identical. All heal systems restore frame integrity or layer durability. No distinction is made between the two terms in any mechanical context.

---

## 2. ACTION SYSTEM

*LOCKED*

### Action Definition

```
action {
  id:             string
  type:           enum [strike, burst, beam, special]
  base_duration:  float (sec)
  payload:        type-specific (see below)
}
```

### Payload Types

```
// Strike — melee
strike_payload {
  anim_tag:     string
  hit_count:    int
  damage_mult:  float
}

// Burst — ranged
burst_payload {
  shot_count:   int
  damage_mult:  float
}

// Beam — sustained energy
beam_payload {
  duration:     float
  damage_mult:  float
}
```

### Action Slot Reference

```
action_ref {
  action_id:          string
  repeat:             int     // default: 1
  duration_override:  float | null
}
```

### Double Strike Pattern

When a modifier adds a second hit to a strike action, the action is referenced
twice in the same slot at half duration each. Total slot time is preserved.
Two independent interrupt windows are created instead of one.

```
// Base
{ action_id: "swing_1", repeat: 1 }
// → 1 hit at 0.5 sec | 1 interrupt window

// With double-strike modifier
{ action_id: "swing_1", repeat: 2, duration_override: null }
// → 2 hits at 0.25 sec each | 2 interrupt windows
// → total time unchanged: 0.5 sec
```

---

## 3. SKILL SYSTEM

*LOCKED*

### Skill Definition

```
skill {
  index:     int [0..4]
  type:      enum [chain, single, held, cooldown]
  sequence:  action_ref[]
  cooldown:  float | null     // null for skill_0
  library:   library_id | null
}
```

### Library Definition

```
actions_library {
  id:       string
  actions:  action[]
}
```

---

## 4. RELOAD SYSTEM

*LOCKED*

Reload is an automated cooldown state. It is not a skill slot.

### Duration Calculation

**Mode: `per_shot` (default)**
```
reload_duration = shots_fired_since_last_reload × scalar
```

**Mode: `forced_full`**
```
reload_duration = max_shots × scalar
```

### Reload Cooldown Definition

```
reload_cooldown {
  id:         "reload_{weapon_id}"
  type:       cooldown
  mode:       enum [per_shot, forced_full]
  scalar:     float
  max_shots:  int | null
  trigger:    enum [auto, manual, both]
  keybind:    string
}
```

---

## 5. WEAPON SWAP

*LOCKED*

```
weapon_swap_cooldown {
  id:             "weapon_swap"
  type:           cooldown
  base_duration:  5.0 sec
  scope:          global
}
```

### Persistence Rule
Cooldowns are bound to the weapon, not the active slot. Weapon swap cannot reset reload cooldowns.

---

## 6. COOLDOWN SYSTEM

*LOCKED*

### Isolation Rule
Cooldowns are independent of all other game systems. They cannot be paused, interrupted, reset, or altered except by explicitly cooldown-typed modifiers.

### Modifier Whitelist

```
cooldown_modifier {
  type:     enum [reduction, increase]
  value:    float (0.0 – 1.0)
  window: {
    duration:  float (sec)
    trigger:   string
  }
  target:   string
  stacking: multiplicative
}
```

### Stacking Formula

```
effective_cooldown = base × (1 - r1) × (1 - r2) × (1 - r3) ...
```

| Reductions Active | Value Each | Result on 3.0 sec base | Absolute Gain |
|---|---|---|---|
| 1× 50% | 0.50 | 1.500 sec | −1.500 sec |
| 2× 50% | 0.50 | 0.750 sec | −0.750 sec |
| 3× 50% | 0.50 | 0.375 sec | −0.375 sec |
| 4× 50% | 0.50 | 0.188 sec | −0.188 sec |
| 1× 100% | 1.00 | 0.000 sec | −3.000 sec |

---

## 7. STEALTH RULES

*PROPOSED — pending formalization into implementation*

| Rule | Value | Notes |
|---|---|---|
| Maximum duration | 8 sec base | Extendable by gear only, not feats |
| Attack behavior | Stealth breaks after first hit resolves | Ambush advantage preserved |
| Movement detection | Walk speed or below: undetected. Above: acoustic signature generated | Applies to AI and PvP equally |
| Cooldown timing | Begins on stealth entry, not exit | Cannot chain stealth with zero downtime |
| AoE reveal | Buildable counter — Guardian and Cleric spec | Specific skills reveal stealthed targets |

---

## 8. CC DURATION TABLE

*PROPOSED — pending lock as universal reference*

| CC Type | Duration | Behavior | Notes |
|---|---|---|---|
| Stun | 1.5 sec | Hard lock on all actions | Hardest CC, shortest duration |
| Knockback | 0.75 sec recovery | Retains directional input during flight | Duration is recovery time after landing |
| Root | 2.0 sec | No movement, all other actions allowed | Can still fire, use skills, activate shields |
| Silence | 3.0 sec | Skills locked, Skill 0 allowed | Longer than stun — trades scope for duration |
| Blind | 2.5 sec | Accuracy penalty, not full disable | Penalty value: UNDEFINED (OD-004) |
| Slow | 4.0 sec | 50% movement speed reduction | Longest duration, least disruptive |

CC does not deal damage. A 1.5-second stun equals one full Gladius chain duration.
A skilled attacker lands exactly one chain during a stun. This is intentional.

---

## 9. BASE COMBAT UNIT

*LOCKED — all damage, healing, and feat multiplier values must validate against this*

### Reference Values

```
Reference weapon:      Gladius (Boltcaster)
Reference damage type: Impact (bolt — boltcaster class default)
Shots per chain:       9
Base damage per shot:  6          (static, never changes)
Chain damage (BCU):    54         (static, never changes)
Chain duration:        1.5 sec
Reload (full chain):   9 × 0.35 = 3.15 sec
Full attack cycle:     4.65 sec

Player frame integrity (HP): 200 (static, never changes)
HP : chain ratio:             200 : 54  ≈  4:1
TTK at parity (auto only):   ~18.6 sec
```

### Validation Rule

Any heal, feat multiplier, or damage modifier must be a multiple or fraction
of BCU (54) checked against the 4:1 ratio. A single effect reducing a target
from full HP to zero in fewer than two engagement cycles without the commitment
required by Combat Law 4 fails validation.

### BCU Reference Examples

| Effect | Value | BCU Fraction | Assessment |
|---|---|---|---|
| Reactive Healing (Cleric T1): 15% HP | 30 HP | 0.55× | Covers half a chain. Buys one mistake. Passes. |
| Arcane Annihilation: 800% damage | 432 dmg | 8× | One-shots any build. Requires Law 4 commitment. |
| Grail Regen (Percival): 80 HP/sec | 80/sec | 1.5×/sec | Unkillable if unchecked. Must be interruptible. |

---

## 10. REACTOR ENERGY PRINCIPLE

*LOCKED*

### Principle

All ranged weapons in Orion are arcane focuses — magitech instruments that channel reactor energy to propel projectiles or sustain energy output. The reactor provides the energy character of the output. The weapon class determines the projectile type and base damage type.

```
Damage type      = f(weapon class)   // class default, modifiable post-Belldor
Energy character = f(reactor)        // ichor, mana, ether — affects world interactions
                                     // and spell effect affinities, not base damage type
```

Reactor energy character and damage type are independent. Changing reactor does not change the base damage type of a boltcaster's bolt at Belldor tier.

### Reactor Energy Character

| Reactor | Character | World Interaction Profile |
|---|---|---|
| Mana | Precision and control | UNDEFINED — pending OD-001 |
| Ichor | Neutral to extreme — tunable intensity | UNDEFINED — pending OD-001 |
| Ether | Volatile and wild | UNDEFINED — pending OD-001 |

### Trait Line Default Reactors

*LOCKED*

| Trait Line | Default Reactor |
|---|---|
| Warrior | Ichor |
| Marauder | Ichor |
| Guardian | Mana |
| Rogue | Ether |
| Wizard | Ether |
| Cleric | Mana |

---

## 11. WEAPON CLASS — BOLTCASTER

*LOCKED*

### Definition

Boltcasters are arcane focus weapons that fire **bolts** — physical projectiles propelled by magic. The bolt is the fundamental projectile type of the class. **Class default damage type: Impact.**

### Class Identity

The Boltcaster is the straight sword of Orion. Reliable, balanced, all-purpose. No single weakness. No single specialty. Capable of filling any role at any range within its design envelope.

### Modifications

Bolts are the class default, not a permanent lock. Item modifications and crafting introduced post-Belldor can alter projectile type, damage type, and energy expression. These systems are UNDEFINED until after Belldor progression is complete.

---

## 12. WEAPON CLASS TAXONOMY

*LOCKED*

### Ranged Kinetic Family — Belldor Tier

All three classes share impact damage type, physical projectile, and similar engagement philosophy. Cross-class links within this family are valid. Links outside this family are incompatible at Belldor tier.

| Class | Name | Character | Skill Count | Weight |
|---|---|---|---|---|
| Bolter | Rifle-type | Burst fire, controlled cadence | 3 | Medium |
| Polybolos | Light autocannon | Sustained fire, pressure | 3 | Medium |
| Ballista | Standard autocannon | Long range, deliberate | 4 | Heavy |

### Named Weapons — Warrior Default

| Weapon | Class | Damage Type | Reactor | Notes |
|---|---|---|---|---|
| N-55 Gladius | Bolter | Impact | Ichor | BCU reference weapon |
| M-300 Hasta | Polybolos | Impact | Ichor | Sustained pressure |
| M-450 Pilum | Ballista | Impact | Ichor | Long range anchor |

---

## 13. ACTION BAR TAXONOMY

*LOCKED*

```
Action 1:     Weapon set (active) — auto attack
Action 2:     Weapon set (active)
Action 3:     Weapon set (active)
Action 4:     Weapon set (active)
Action 5:     Weapon set (active)
Action 6:     Utility slot
Action 7:     Aux / Belt pool
Action 8:     Aux / Belt pool
Action 9:     Aux / Belt pool
Action 10:    Aux / Belt pool

Actions 1-5 change entirely when weapon set is swapped.
Action 6 is always present — populated by utility slot item.
  If utility slot is empty: no skill, slot empty.
Actions 7-10 are populated by aux slots 1-4 and belt slots 1-4
  subject to stacking and linking rules.
  Maximum 4 action slots regardless of aux/belt item count.

A build with zero populated aux/belt actions is valid.
```

---

## 14. SLOT TAXONOMY

*LOCKED*

```
Weapon set 1:     2 slots (Main 1 + Main 2) or 1 slot (2H Main 1)
Weapon set 2:     2 slots (Main 3 + Main 4) or 1 slot (2H Main 3)
Utility:          1 slot — sustain items only (heal, repair)
                  populates action 6
                  starts filled with class default item at L6
                  consumable heal occupies belt slot pre-L6
Aux slots 1-4:    specialized equipment — offensive, defensive, or passive
                  populates actions 7-10 (shared pool with belt)
Belt slots 1-4:   consumables
                  populates actions 7-10 (shared pool with aux)

Total slots:      13 (12 if both sets are 2H)

Action slot consumption rules:
  Stacking:   identical items stack — 1 action, N charges
  Linking:    named linked pairs produce fewer actions than slots
              defined at item level, not player choice
  Passive:    items producing no action consume 0 action slots
```

---

## 15. WEAPON SET RULES

*LOCKED*

```
Active set:     all five actions (1-5) are defined by the active set
                swapping sets changes all five actions simultaneously
                swap cooldown: 5.0 sec global

Mainhand:       always owns actions 1, 2
                may also claim 3, 4, 5

Offhand:        always owns action 5
                may also claim 3, 4

Priority:       mainhand wins all conflicts

Action distribution by mainhand skill count:
  Mainhand 4 skills:    m, m, m, m, o
  Mainhand 3 skills:    m, m, m, o, o
  Mainhand 2 skills:    m, m, o, o, o
  Mainhand 1 skill:     m, o, o, o, o
  2H mainhand:          m, m, m, m, m (offhand impossible)

All subject to exemption by modifier sources (feats, etc).
```

### Offhand Skill Assignment

Offhand skills are defined specifically for the offhand role. They are not leftover mainhand skills. An offhand weapon has a dedicated set of skills appropriate to its supplementary position.

---

## 16. TWIN-LINK RULES

*LOCKED*

```
Twin-link requires:   matching weapon CLASS in both set slots
                      not matching weapon identity

Valid links:
  Bolter + Bolter
  Polybolos + Polybolos
  Ballista + Ballista

Invalid links:
  Any cross-class combination within a set

Link strength:
  Bolstered:    matching class AND matching identity (e.g. Gladius + Gladius)
                named skill — output elevated
                e.g. Gladius + Gladius → Twin Salvo (bolstered)

  Standard:     matching class, different identity (e.g. Gladius + Spatha)
                class-named skill — output unbolstered
                e.g. Gladius + Spatha → Bolter Link

Link slot position:
  Determined by mainhand skill count
  Mainhand 4 skills → link at action 4
  Mainhand 3 skills → link at action 3
  Mainhand 2 skills → link at action 2
  Mainhand 1 skill  → no link granted

Naming convention:
  Bolstered: [Weapon Name] + [Weapon Name]  (e.g. Gladius + Gladius)
  Standard:  [Class] + [Class]              (e.g. Bolter + Bolter)
```

---

## 17. CROSS-CLASS LINK RULES

*LOCKED*

Cross-class links apply when mainhand and offhand are different weapon classes within the same family.

```
Cross-class link output:    1× BCU (between standard and bolstered)
Character:                  sourced from mainhand class
                            offhand modifies the expression
Link slot:                  determined by mainhand skill count

Incompatible classes:       produce mainhand skill only
                            offhand contributes no link benefit
                            e.g. Bolter + Howitzer
```

### Ranged Kinetic Family Cross-Class Links

| Mainhand | Offhand | Link Slot | Name | Output | CD | Identity |
|---|---|---|---|---|---|---|
| Bolter | Polybolos | 3 | Burst and Sustain | 1× BCU | 18 sec | Two instances — sequential armor resolution |
| Bolter | Ballista | 3 | Breach and Pin | 1× BCU | 20 sec | Conditional slow — burst must connect |
| Polybolos | Bolter | 3 | Sustained Burst | 1× BCU | 16 sec | Held — scalable heat commitment |
| Polybolos | Ballista | 3 | Layered Fire | 1× BCU | 20 sec | Range-dependent — both must connect |
| Ballista | Polybolos | 4 | Overwatch | 1× BCU | 22 sec | Position anchor — space control |
| Ballista | Bolter | 4 | Lock and Breach | 1× BCU | 18 sec | Mark exploit — sequential resolution |

### Exception Design Intent

Exceptions to link rules, action bar rules, and structural rules exist to serve:

```
Gap filling:    covering a build's known weakness without
                redesigning the core system

Stat dump:      min/max optimization surface for players
                pushing a single axis hard

Balance patch:  targeted correction where base rules produce
                an unintended hole at a specific level band
```

Exception validation requirements:
- Identifies which gap it fills OR which stat axis it dumps
- Does not make a non-exception build strictly inferior
- Narrower in scope than the rule it exempts
- Cannot stack with another exception covering the same gap

---

## 18. AUX WEAPON RULES

*LOCKED*

```
Aux weapons:    self-contained skill sets
                skills defined by the specific item
                not by weapon class category

Pairing:        specific to named item combinations
                not class-based
                e.g. Turbine Khopesh + Turbine Khopesh
                is a named pair with a named linked skill
                a different weapon in the second slot
                produces no link benefit

Modification:   aux weapon behavior is the most
                modification-receptive surface in the game
                feats, reactors, other equipment can
                extend aux weapon function significantly
                base definition leaves clear modification hooks
```

---

## 19. FRAME CRITICAL STATE

*LOCKED*

```
Trigger:        frame integrity ≤ 40 HP (≤ 20% of 200)

Mobility penalty:
  Effect:       -30% movement speed
  Duration:     until frame integrity recovers above threshold

Weapon malfunction:
  Effect:       each shot in a chain has 15% chance to misfire
                shot does not fire — reload timer still accumulates
  Expected:     on 9-shot chain, ~1.35 misfires
                chain damage drops from 54 → ~46 expected
  Mitigation:   shorter bursts and managed reloads reduce impact
                skill expression — not a stat fix

Critical state vs Orc pair:
  Chain Fire:   ~9.2 effective per chain expected (vs 10.8 normal)
  Movement:     -30% — kiting harder, Scout harder to avoid
  Message:      finish this or disengage
```

---

## 20. BOOSTER KIT — EVADE SYSTEM

*LOCKED*

### Basic Dash (all mechs, always available)

```
Function:       repositioning only
I-frames:       none — grapple connects regardless of timing
Heat:           generated on use
Energy:         consumed on use
Weight effect:  heavier frame = higher energy cost per dash
```

### Grapple Hit Window Reference

```
Grapple windup:     0.5 sec (30 frames) — visible tell
Grapple hit window: 10 frames (~167ms)
Player must commit to evade during windup, not hit window
```

### Booster Kit I-Frame Table

Applies to both consumable (Poe reward) and mounted system (PNSY UF-9):

| Frame Weight | I-frames | Duration at 60fps | Margin vs Grapple Window |
|---|---|---|---|
| Superheavy | 11 frames | ~183ms | +1 frame — minimum viable |
| Heavy | 14 frames | ~233ms | +4 frames |
| Medium | 18 frames | ~300ms | +8 frames |
| Light | 22 frames | ~367ms | +12 frames |

### Energy Cost Per Booster Dash

| Frame Weight | Energy Cost | Heat Generated |
|---|---|---|
| Superheavy | 40 | 10 |
| Heavy | 30 | 10 |
| Medium | 20 | 10 |
| Light | 12 | 10 |

Heat is flat across all weights — the booster is the same kit regardless of frame.

### Consumable vs Mounted

```
Consumable (Poe reward):    belt slot item
                             3 charges
                             finite — burns out
                             available at L2

Mounted (PNSY UF-9):        aux slot item
                             persistent — 8 sec cooldown
                             recharges between uses
                             available when aux slot configurable
```

---

## 21. WEAPON DEFINITION SCHEMA

*LOCKED*

```
weapon {
  name:           string
  class:          string    // Bolter, Polybolos, Ballista, Melee, Launcher, Lance, Special
  damage_type:    enum [pierce, impact, blast, beam, pulse, wave]
  energy_subtype: enum [mana, ichor, ether] | null
  weight:         enum [light, medium, heavy]
  skill_count:    int       // determines link slot position

  library:        actions_library | null

  reload_cooldown: {
    id:         "reload_{weapon_id}"
    type:       cooldown
    mode:       enum [per_shot, forced_full]
    scalar:     float
    max_shots:  int | null
    trigger:    enum [auto, manual, both]
    keybind:    string
  }

  mainhand_skills: {
    skill_0: skill    // action 1 when mainhand
    skill_1: skill    // action 2 when mainhand
    skill_2: skill    // action 3 when mainhand (if skill_count ≥ 3)
    skill_3: skill    // action 4 when mainhand (if skill_count = 4)
  }

  offhand_skills: {
    offhand_0: skill  // always action 5 when offhand
    offhand_1: skill  // action 4 when offhand (if mainhand skill_count ≤ 3)
  }
}
```

---

## 22. REFERENCE EXAMPLE — GLADIUS

*LOCKED — BCU reference weapon*

### Identity

The Gladius is the reference boltcaster — the Dark Souls longsword of Orion. Balanced, reliable, all-purpose. Fast enough to punish, heavy enough to matter. It does everything at parity and nothing at ceiling. Every other boltcaster is defined in relation to it. Its numbers do not change.

### Ichor Gladius — Full Definition

```
weapon {
  name:        "Gladius"
  class:       Bolter
  damage_type: impact
  energy_subtype: ichor
  weight:      medium
  skill_count: 3

  mainhand_skills: {
    skill_0 (action 1): Chain Fire
      type:     chain
      shots:    9 × 6 = 54 damage (1× BCU)
      duration: 1.5 sec
      reload:   per_shot, 0.35 scalar (3.15 sec full chain)
      heat:     low per shot, moderate accumulated
      tell:     none — auto attack

    skill_1 (action 2): Hammer Shot
      type:     single
      shots:    3 × 18 = 54 damage (1× BCU)
      duration: 0.75 sec
      cooldown: 10 sec
      heat:     medium
      tell:     weapon pulls back 0.3 sec before firing

    link (action 3): Twin Salvo [bolstered — Gladius + Gladius]
      type:     single
      shots:    9 from each weapon = 18 × 6 = 108 damage (2× BCU)
      duration: 1.5 sec
      cooldown: 25 sec
      heat:     very high
      tell:     both weapons charge simultaneously
                player immobile during 0.75 sec windup
  }

  offhand_skills: {
    offhand_0 (action 5): Suppression Burst
      type:     single
      shots:    6 × 4 = 24 damage (0.44× BCU)
      pattern:  60° spread
      duration: 0.75 sec
      cooldown: 14 sec
      heat:     low
      tell:     weapon splays wide 0.3 sec before firing
      effect:   slow — 25% movement/attack/reload, 4.0 sec

    offhand_1 (action 4): Reposition Fire
      type:     single
      shots:    4 × 6 = 24 damage (0.44× BCU)
      duration: 1.0 sec
      cooldown: 8 sec
      heat:     low
      tell:     offhand raises to firing stance 0.2 sec
      effect:   no movement penalty while firing
  }
}
```

### Ichor Hasta — Full Definition

```
weapon {
  name:        "Hasta"
  class:       Polybolos
  damage_type: impact
  energy_subtype: ichor
  weight:      medium
  skill_count: 3

  mainhand_skills: {
    skill_0 (action 1): Suppressing Fire
      type:     chain (held)
      shots:    continuous — 6 per sec × 9 damage = 54/sec (1× BCU/sec)
      spinup:   0.4 sec before first shot
      spindown: 0.2 sec after input released
      heat:     low per shot, escalates with duration
                high heat band entered after 3 sec sustained
      tell:     barrel rotation visible during spinup
      ichor:    heat escalation is intensity dial
                high heat increases tick damage (multiplier UNDEFINED)

    skill_1 (action 2): Overclock Burst
      type:     single
      shots:    12 × 6 = 72 damage (1.33× BCU)
      duration: 1.0 sec
      cooldown: 12 sec
      heat:     very high spike — cannot use at heat ceiling
      tell:     barrel accelerates visibly 0.3 sec before burst

    link (action 3): Sustained Lock [Polybolos mainhand — single weapon]
      type:     held (up to 3.0 sec cap)
      shots:    continuous — 9 × 4 = 36 damage (0.67× BCU/sec)
      cooldown: 14 sec (begins on release)
      heat:     medium, constant while held
      tell:     targeting reticle narrows 0.3 sec
      effect:   accuracy increases per second held on same target
                second 1: standard spread
                second 2: 50% spread reduction
                second 3: pinpoint — accelerates local durability damage
  }

  offhand_skills: {
    offhand_0 (action 5): Sweep
      type:     single
      shots:    6 × 3 = 18 damage (0.33× BCU) per target in arc
      pattern:  90° sweep arc
      duration: 0.75 sec
      cooldown: 16 sec
      heat:     low
      tell:     barrel rotates to sweep start 0.3 sec
      effect:   hits all targets in arc independently

    offhand_1 (action 4): Covering Fire
      type:     single
      shots:    8 × 4 = 32 damage (0.59× BCU)
      duration: 1.0 sec
      cooldown: 10 sec
      heat:     low
      tell:     barrel spins up briefly 0.2 sec
      effect:   suppresses target — 20% miss chance on target shots, 2.0 sec
  }
}
```

### Ichor Pilum — Full Definition

```
weapon {
  name:        "Pilum"
  class:       Ballista
  damage_type: impact
  energy_subtype: ichor
  weight:      heavy
  skill_count: 4

  mainhand_skills: {
    skill_0 (action 1): Bolt Shot
      type:     chain
      shots:    3 × 18 = 54 damage (1× BCU)
      duration: 1.5 sec
      reload:   per_shot, 0.5 scalar
      heat:     medium per shot
      tell:     weapon extends to firing position 0.3 sec
      effect:   optimal range band: medium-long
                below optimal: -20% damage
                above optimal: -10% damage
      ichor:    heat buildup per shot — high heat increases damage (multiplier UNDEFINED)

    skill_1 (action 2): Armor Pierce
      type:     single
      shots:    1 × 54 = 54 damage (1× BCU)
      duration: 0.75 sec
      cooldown: 14 sec
      heat:     high
      tell:     weapon retracts then extends — 0.5 sec windup
      effect:   ignores armor T layer entirely
                resolves against R only
                durability damage doubled on this hit

    skill_2 (action 3): Suppression Shot
      type:     single
      shots:    1 × 36 = 36 damage (0.67× BCU)
      duration: 1.0 sec
      cooldown: 16 sec
      heat:     medium
      tell:     weapon barrel widens visibly 0.3 sec
      effect:   root on hit (2.0 sec — CC table)

    link (action 4): Overcharge Round [Ballista mainhand — single weapon]
      type:     single
      shots:    1 × 108 = 108 damage (2× BCU)
      duration: 2.0 sec
      cooldown: 30 sec
      heat:     maximum — guaranteed heat ceiling
      tell:     weapon glows visibly during 1.0 sec immobile charge
                audio cue escalates during charge
      effect:   heat ceiling after firing — 4.0 sec mandatory weapon lockout
                cannot fire any weapon during lockout
  }

  offhand_skills: {
    offhand_0 (action 5): Brace Shot
      type:     single
      shots:    1 × 36 = 36 damage (0.67× BCU)
      duration: 1.0 sec
      cooldown: 18 sec
      heat:     medium
      tell:     stabilizers extend visibly 0.3 sec
      effect:   player immobile for duration
                damage taken reduced 40%
                knockback immunity during firing

    offhand_1 (action 4): Ranging Shot
      type:     single
      shots:    1 × 27 = 27 damage (0.5× BCU)
      duration: 0.75 sec
      cooldown: 8 sec
      heat:     low
      tell:     weapon extends 0.2 sec
      effect:   marks target 4.0 sec
                marked target takes +15% damage from all hits
                mark consumed after 3 hits or duration
  }
}
```

---

## OPEN DECISIONS

| # | Decision | Blocks |
|---|---|---|
| OD-004 | Blind CC accuracy penalty value | CC table finalization |
| OD-008 | Armor threshold and rating values | Enemy stat blocks, full armor implementation |
| OD-MECH-01 | Boltcaster modification system — post-Belldor | Item modification design |
| OD-MECH-02 | Skills 1–4 for Gladius in set 2 configurations | Warrior set 2 full definition |
| OD-MECH-03 | Other boltcaster weapons — class expression across weight classes | Armory weapon definitions |
| OD-MECH-04 | Ichor Hasta and Pilum heat damage multiplier — exact value | Weapon damage at high heat |
| OD-MECH-05 | Standard link output — same class, different identity (e.g. Gladius + Spatha) | Cross-class balance |

*See SystemRevision001.md for full open decisions list.*

---

*mechanics-v1.md | Project Orion internal documentation*
*Version 1.3 — Action bar taxonomy added. Slot taxonomy added. Weapon class taxonomy added (Bolter, Polybolos, Ballista). Twin-link rules locked. Cross-class link rules locked. Aux weapon rules locked. Frame critical state locked. Booster kit evade system locked. Repair = heal for machines locked. Full ichor weapon definitions added (Gladius, Hasta, Pilum). Offhand skill model revised — offhand skills are role-specific, not leftover mainhand skills.*
