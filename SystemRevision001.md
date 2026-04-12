# PROJECT ORION \u2014 SYSTEM REVISION 001
### Combat & Progression Systems
*Revision session: 2026-03-28*
*Supersedes: mechanics-v1.md (partial), armory.md (partial), codex.md (partial)*

---

## STATUS KEY

| Label | Meaning |
|---|---|
| LOCKED | Agreed and finalized in this revision |
| PROPOSED | Discussed, pending confirmation |
| DERIVED | Not covered in this session \u2014 derived from existing documentation |
| UNDEFINED | Identified gap, not yet resolved |

---

## TABLE OF CONTENTS

1. [Design Laws](#1-design-laws)
2. [Progression Model](#2-progression-model)
3. [Slot Unlock Sequence](#3-slot-unlock-sequence)
4. [Base Combat Unit](#4-base-combat-unit)
5. [EHP Stack](#5-ehp-stack)
6. [Armor System](#6-armor-system)
7. [Shield System](#7-shield-system)
8. [Damage Type Taxonomy](#8-damage-type-taxonomy)
9. [HP vs Damage Matrix](#9-hp-vs-damage-matrix)
10. [Resource System](#10-resource-system)
11. [Load System](#11-load-system)
12. [Stealth System](#12-stealth-system)
13. [CC Duration Table](#13-cc-duration-table)
14. [Tier V Feat Revisions](#14-tier-v-feat-revisions)
15. [Open Decisions](#15-open-decisions)

---

## 1. DESIGN LAWS

*LOCKED \u2014 These govern all combat design decisions. Any proposed mechanic that violates a law requires the law to be explicitly revisited, not quietly ignored.*

### The Five Combat Laws

**Law 1: Every ability has a tell.**
All skills have a windup animation before the hit lands. The windup is the counterplay window. Stealth builds have positioning tells. Beam builds have charge-up tells. No effect is instant with no readable warning.

**Law 2: CC creates opportunity, not guaranteed damage.**
All CC opens a damage window. It does not deal damage itself. Duration is fixed across PvE and PvP \u2014 no separate tables. The CC-into-damage combo is the intended flow.

**Law 3: Sustained healing cannot negate sustained DPS at parity.**
A Cleric build in 1v1 cannot be unkillable against a similarly-geared attacker. Solo healing output must always be less than solo incoming damage at parity gear. Healing advantage requires positioning advantage.

**Law 4: High-damage effects require high commitment.**
Any effect capable of defeating a target in fewer than two engagement cycles must require significant commitment: channel time, immobility, self-damage, or a cooldown long enough to define the build around it.

**Law 5: Presence governs priority, not invulnerability.**
High Presence builds draw focus from AI and force reactions from players. They are not harder to kill \u2014 they are harder to ignore. Neither high nor low Presence is an unkillable state.

### The Progression Law

**Belldor teaches you how to fight. Every other world tests whether you learned.**

Progression on Belldor is vertical \u2014 unlocks, growing tool access, mechanical introduction. Progression everywhere else is horizontal \u2014 loadout expression, mechanical mastery, situational adaptation. The stat ceiling is fixed at Level 20 and never moves. Two Level 20 players have identical stats. The only variable is build and skill.

### The Unification Law

**PvP and PvE share identical rules. No exceptions.**

Any mechanic that requires a separate PvP modifier or a PvP-specific balance track is a broken mechanic. The fix is to redesign the mechanic so it works in both contexts under the same numbers. This constraint is non-negotiable and must be applied during design, not after.

---

## 2. PROGRESSION MODEL

*LOCKED*

### Stat Behavior

Player combat stats are **static values** from Level 1 through Level 20 and permanently thereafter. Stats do not scale with level. Stats do not scale with gear post-cap.

```
Level 1 stats  =  Level 20 stats  =  Level 20 + endgame gear stats
```

This is not a simplification. It is the design. Difficulty in Belldor is mechanical complexity matched to available tools \u2014 not a stat gate.

### What Leveling Does

Leveling unlocks **equipment slots** and **mechanical systems**. It does not increase numbers. See Section 3 for the full unlock sequence.

### Post-Cap Progression

At Level 20 all slots are unlocked and all feat tiers are accessible. Post-cap progression is entirely horizontal:

- **Feat combination mastery** \u2014 discovering which feat combinations create the desired build identity
- **Loadout variation** \u2014 gear provides mechanical variation, not stat inflation. A different weapon has a different moveset, not higher damage.
- **Situational adaptation** \u2014 different worlds require different loadout responses. Adapting without changing stats is the skill expression.

Post-cap gear is a **horizontal choice**, not a vertical upgrade. No item acquired after Level 20 increases base stats. This constraint applies to all future itemization design.

---

## 3. SLOT UNLOCK SEQUENCE

*PROPOSED \u2014 sequence and pacing subject to revision during Belldor zone design*

### Phase 1 \u2014 Levels 1\u20135: Learning The Mech

| Level | Unlock |
|---|---|
| 1 | Torso slot + primary weapon slot 1 + Skill 0 (auto-attack) |
| 2 | Legs slot (mobility options come online) |
| 3 | Weapon slot 2 + weapon swap |
| 4 | Skill slots 1\u20132 on equipped weapons |
| 5 | Generator slot (Heat and Energy systems come online) |

Player focus: movement, attack, swap. No resource management yet.

### Phase 2 \u2014 Levels 6\u201310: Learning The Build

| Level | Unlock |
|---|---|
| 6 | Skill slots 3\u20134 on equipped weapons |
| 7 | Head slot (optics and sensors \u2014 affects targeting) |
| 8 | Arm slot + sidearm slots |
| 9 | First feat slot (Tier I only) |
| 10 | Defensive system slot 1 |

Player focus: build identity, first meaningful feat choice.

### Phase 3 \u2014 Levels 11\u201315: Learning The Role

| Level | Unlock |
|---|---|
| 11 | Defensive slots 2\u20133 |
| 12 | Second feat slot (Tier I\u2013II) |
| 13 | Weapon slots 3\u20134 (full weapon set) |
| 14 | Third feat slot (Tier I\u2013III) |
| 15 | Presence system comes online |

Presence is locked until Level 15 \u2014 it is a team-facing system with no value before the player understands their own mech.

### Phase 4 \u2014 Levels 16\u201320: Mastery

| Level | Unlock |
|---|---|
| 16 | Fourth feat slot (Tier I\u2013IV) |
| 17 | Core system slot (reactor identity) |
| 18 | Fifth feat slot (Tier I\u2013IV) |
| 19 | Tier V feat access |
| 20 | Full loadout. All slots. All feats available. |

Level 20 is not a reward. It is a declaration that the tutorial is complete.

### Belldor Encounter Design Constraint

Every mandatory encounter in Belldor must be solvable using only the tools available at the player's current level. A mechanic or enemy type that requires a tool locked behind a later level must not be a mandatory encounter before that tool is available. Avoidance is a valid solution when tools are unavailable \u2014 the encounter should communicate this.

---

## 4. BASE COMBAT UNIT

*LOCKED \u2014 all damage values, healing values, and feat multipliers must be validated against this reference*

### Definition

The Base Combat Unit (BCU) is defined as the damage dealt by one full auto-attack chain from the reference weapon (Gladius Rifle) against an unarmored, unshielded target.

```
Reference weapon:     Gladius Rifle (mechanics-v1.md)
Shots per chain:      9
Base damage per shot: 6
Chain damage (BCU):   54
Chain duration:       1.5 sec
Reload (full chain):  9 \u00d7 0.35 = 3.15 sec
Full attack cycle:    4.65 sec
```

### Player Stats

```
Frame integrity (HP):     200     (static, never changes)
Base damage per shot:     6       (static, never changes)
Chain damage:             54      (static, never changes)
```

### Target Engagement Time

```
Chains to defeat at parity (auto only):   ~4 chains
TTK (auto only, parity):                  ~18.6 sec
```

This is the floor TTK. Skilled use of Skills 1\u20134 compresses it. Unskilled play extends it. The gap between floor and skilled TTK is where player skill lives.

### Validation Rule

Any heal value, feat multiplier, or damage modifier must be expressed as a multiple or fraction of BCU (54) and must be checked against the 4:1 ratio (HP 200 : chain damage 54). If a single effect can reduce a target from full HP to zero in fewer than two engagement cycles without the commitment required by Law 4, it fails validation.

---

## 5. EHP STACK

*LOCKED \u2014 layer order and defeat conditions*
*PROPOSED \u2014 specific values per trait line*

### Layer Order

```
Incoming damage \u2192 SHIELDS \u2192 ARMOR \u2192 FRAME \u2192 defeat
```

Damage exhausts each layer completely before the next layer takes any. No bleed-through until the layer is at zero.

### Layer Summary

| Layer | Recovery | Defeat Condition |
|---|---|---|
| Shields | Active recharge / re-cast / repair | Depleted or overloaded |
| Armor | Bolstered by subsystems / repaired by Cleric or kits | Durability reaches zero |
| Frame | Town or out-of-combat only (exception: ally Cleric emergency repair) | Integrity reaches zero \u2192 mech defeated |

### Frame Defeat States

```
Operational:  frame integrity above 20%
Critical:     frame integrity at or below 20% \u2014 mobility penalty, weapon malfunction chance
Defeated:     frame integrity at zero
```

In co-op, defeated state allows ally repair before elimination. In PvP, defeated is elimination.

### Meta Stats

EHP total and layer breakdowns are **not displayed to the player**. Players observe their mech's visible damage state, not numbers. Individual layer indicators (shield active/broken, armor integrity visual) are acceptable. A single combined EHP bar is not.

### Suggested EHP Profiles Per Trait Line

*PROPOSED*

| Trait Line | Shield | Armor | Frame | Total |
|---|---|---|---|---|
| Warrior | 60 energy | 80 reactive | 200 | 340 |
| Marauder | none | 120 hardened | 200 | 320 |
| Guardian | 100 physical | 100 layered | 200 | 400 |
| Rogue | 40 energy | 40 ablative | 200 | 280 |
| Wizard | 80 energy | 60 layered | 200 | 340 |
| Cleric | 60 energy | 80 reactive | 200 | 340 |

Frame is 200 across all builds. Differentiation lives in shields and armor only.

---

## 6. ARMOR SYSTEM

*LOCKED \u2014 model and types*
*PROPOSED \u2014 specific threshold and rating values*

### Model

Armor uses a threshold + rating + type + durability model. Percentage reduction is explicitly rejected as physically incoherent.

```
armor {
  type:           enum [ablative, reactive, hardened, layered]
  threshold:      int     // minimum per-hit damage required to penetrate
  rating:         int     // flat damage absorbed per penetrating hit
  durability:     int     // current \u2014 degrades under fire
  max_durability: int
}
```

### Penetration Rule

```
if incoming_hit < armor.threshold  \u2192  stopped entirely, zero damage to next layer
if incoming_hit \u2265 armor.threshold  \u2192  penetrates, effective damage = hit - armor.rating
```

Rating reduction applies only to hits that penetrate. Stopped hits deal nothing.

### Durability Degradation

Every hit that reaches the armor layer reduces durability regardless of whether it penetrates. Threshold and rating degrade proportionally:

```
threshold_current = base_threshold \u00d7 (durability / max_durability)
rating_current    = base_rating    \u00d7 (durability / max_durability)
```

At zero durability, armor provides no protection. Sustained fire on any armor type will eventually degrade it to nothing.

### Armor Types

| Type | Strong Against | Weak Against | Notes |
|---|---|---|---|
| Ablative | High-energy single hits | Sustained low-energy fire | Ceramic analog \u2014 shatters AP, erodes under SMG |
| Reactive | Sustained fire, pierce | AP rounds, blast | Mild steel analog \u2014 deforms and absorbs |
| Hardened | Low-energy small arms, pierce | High-energy single hits, beam | Brittle under concentrated energy |
| Layered | Balanced \u2014 no single exploit | Nothing specifically | Composite \u2014 no weakness, no immunity |

### Armor Recovery

- **Energy armor** \u2014 rating recharges passively when not under fire. Threshold does not recharge.
- **Physical armor** \u2014 no passive recovery. Repaired by Cleric systems or repair kits in or out of combat. Durability repair rate must satisfy Law 3.

---

## 7. SHIELD SYSTEM

*LOCKED \u2014 model and behavior*
*PROPOSED \u2014 specific values and overload threshold*

### Model

```
shield {
  type:              enum [energy, physical]
  capacity:          int
  current:           int
  overload_threshold: int   // single-hit damage that breaks shield instantly
  state:             enum [active, broken, offline]
}
```

### Energy Shields

- Recharge passively when not taking damage
- Can be re-cast if depleted but not broken
- Subject to overload: if a single hit exceeds overload_threshold, shield breaks instantly regardless of remaining capacity and enters lockout before recharging
- Lockout duration: UNDEFINED \u2014 needs value

### Physical Shields

- Do not recharge passively
- Repaired by Cleric systems, repair kits, or ally intervention
- Can be raised and lowered as an active defensive action (Guardian buckler)
- Not subject to overload \u2014 absorb until capacity reaches zero

### Damage Flow Through Shield Layer

```
1. Check overload threshold (energy shields only):
   if hit > overload_threshold \u2192 shield breaks, remaining damage passes to armor

2. If not overloaded:
   shield.current -= hit
   if shield.current \u2264 0 \u2192 shield depleted, excess damage passes to armor
```

---

## 8. DAMAGE TYPE TAXONOMY

*LOCKED*

### Kinetic Damage

| Type | Description | Examples |
|---|---|---|
| Pierce | Penetrating projectiles and blades | Bullets, AP rounds, daggers, rifle shots |
| Impact | Blunt force, melee strikes, rams | Autocannon blunt, mech melee, shield bash |
| Blast | Explosive ordnance only | Grenades, rockets, mortar rounds |

Melee weapons deal Pierce or Impact depending on weapon type \u2014 not a separate damage category. A blade deals Pierce. A hammer deals Impact.

### Energy Damage

| Type | Description | Examples |
|---|---|---|
| Beam | Sustained projection | Arclight Cannon, Prism Lance |
| Pulse | Burst discharge | Sparkstrike, ARC Conductor |
| Wave | Area propagation | Mana