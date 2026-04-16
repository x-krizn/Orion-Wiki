# PROJECT ORION — GLOSSARY
### Canonical Term Definitions — Combat, Equipment, and Taxonomy
*Version: 0.1 | Updated: 2026-04-14 | Status: Active*

---

## STATUS KEY

| Label | Meaning |
|---|---|
| LOCKED | Finalized. Do not revise without explicit design session. |
| PROPOSED | Under discussion. Pending confirmation. |
| DERIVED | Inferred from other locked documentation. |
| UNDEFINED | Known gap. Blocks dependent systems. |

---

## PURPOSE

This document defines canonical terms used across all Project Orion documentation.
When a term appears in any document, its meaning is the definition given here.
No document may redefine a term locally without first updating this glossary.

---

## TABLE OF CONTENTS

1. [Weapon Taxonomy Terms](#1-weapon-taxonomy-terms)
2. [Link Terms](#2-link-terms)
3. [Combat Terms](#3-combat-terms)
4. [Equipment Terms](#4-equipment-terms)
5. [Damage Terms](#5-damage-terms)
6. [Resource Terms](#6-resource-terms)
7. [Status Terms](#7-status-terms)

---

## 1. WEAPON TAXONOMY TERMS

*LOCKED*

### Family

The broadest grouping of weapon classes. Weapons in the same family share
engagement philosophy and physical projectile character.
Family does not imply a shared damage type.

```
Examples:
  Ranged Kinetic  — Bolter, Polybolos, Ballista, Catapult
  Repeater        — Gatling
  Launcher        — Rocket, Missile
```

Family membership determines eligibility for family-level links (Barbarian feat).
Family membership alone does not grant base links.

---

### Class

A weapon classification within a family. Defines the weapon's mechanical
character, skill count, and base link eligibility.

```
Examples:
  Bolter     (Ranged Kinetic family)
  Polybolos  (Ranged Kinetic family)
  Ballista   (Ranged Kinetic family)
  Catapult   (Ranged Kinetic family)
  Gatling    (Repeater family)
  Rocket     (Launcher family)
  Missile    (Launcher family)
```

Same class in both weapon set slots = class link eligible.
Same family, different class, same damage type = type link eligible.

---

### Identity

A specific named weapon of a given class.

```
Examples:
  N-55 Gladius    — Bolter identity
  M-300 Hasta     — Polybolos identity
  M-450 Pilum     — Ballista identity
  B-25 Brushcutter — Gatling identity
```

Two weapons of the same class but different identity produce a standard class link.
Two weapons of the same class and same identity produce a bolstered class link.

---

### Damage Type

The physical or energy character of a weapon's output. Determines armor and
shield interaction coefficients. Independent of weapon class and family.

```
Kinetic:    Pierce | Impact | Blast
Energy:     Beam | Pulse | Wave
```

Full interaction profiles: interaction-matrix-v0.md.

---

### Energy Subtype (Reactor Character)

A modifier applied by the reactor to energy weapon output. Affects world
interaction profiles and spell effect affinities. Does not change base damage type.

```
Mana   — precision and control
Ichor  — neutral to extreme, tunable intensity
Ether  — volatile and wild
```

Full principle: mechanics-v1.md §10.

---

## 2. LINK TERMS

*LOCKED*

### Class Link

A linked skill produced when both slots in a weapon set contain weapons
of the same class.

```
Requirement:    same class in mainhand and offhand
Output:         bolstered or standard (see below)
Unlock:         base — no feat required
```

---

### Bolstered Link

A class link produced when both slots contain the same class AND the same identity.

```
Requirement:    same class + same identity in both slots
Output:         named skill, elevated output (above 1× BCU)
Example:        Gladius + Gladius → Twin Salvo (2× BCU)
```

---

### Standard Link

A class link produced when both slots contain the same class but different identities.

```
Requirement:    same class, different identity in both slots
Output:         class-named skill, 1× BCU
Example:        Gladius + Spatha → Bolter Link (1× BCU)
                (Spatha is a hypothetical Bolter identity)
```

---

### Type Link

A linked skill produced when mainhand and offhand are different classes
within the same family, and share the same damage type.

```
Requirement:    same family + same damage type + different class
Output:         1× BCU
Unlock:         base — no feat required
Example:        Rocket (Launcher / Blast) + Missile (Launcher / Blast)
                → standard type link (1× BCU)

Not eligible:   same family, different damage type
                different family, same damage type
```

Type links are not bolstered. Bolstered applies to class links only.

---

### Family Link

A linked skill unlocked by the Barbarian feat line. Expands link eligibility
beyond same-family same-type to cross-family or cross-type combinations.

```
Requirement:    Barbarian feat (specific feat UNDEFINED)
Unlock:         post-L5, feat-gated
Scope:          UNDEFINED — pending feat design
```

---

### Link Slot

The action bar slot (actions 2–4) populated by a link skill. Position is
determined by the mainhand weapon's skill count.

```
Mainhand skill count 4  →  link at action 4
Mainhand skill count 3  →  link at action 3
Mainhand skill count 2  →  link at action 2
Mainhand skill count 1  →  no link slot
```

A link skill occupies the slot regardless of whether it comes from a class link
or a type link.

---

### No Link

When mainhand and offhand are different classes and do not meet type link
requirements, no link skill is produced. The offhand still contributes
its offhand skills to the action bar.

---

## 3. COMBAT TERMS

*LOCKED*

### Action

The atomic unit of a moveset. Has a base duration and a typed payload.
Never modified at the definition level — modifications apply at the skill
slot reference level only.

---

### Action Modifier

Applied to a skill slot's action reference. Controls duration override,
repeat count, and timing split. Leaves the source action unchanged.

---

### Skill

One of five slots per weapon set bound to actions 1–5 on the action bar.
Skill 0 / Action 1 is auto-attack. Skills 1–4 are weapon-specific.

---

### Moveset

The complete set of five skills for a weapon set. Defines the weapon set's
combat identity.

---

### Chain

A multi-hit auto-attack sequence completing one full firing cycle.
BCU is defined as the damage of one full chain from the Gladius.

```
Gladius chain: 9 shots × 6 damage = 54 (1× BCU)
Duration:      1.5 sec
Reload:        3.15 sec
Full cycle:    4.65 sec
```

---

### BCU (Base Combat Unit)

The damage dealt by one full Gladius chain against an unarmored,
unshielded target. All damage values, healing values, and feat multipliers
are expressed as multiples or fractions of BCU.

```
1× BCU = 54 damage
```

Full definition: mechanics-v1.md §9.

---

### TTK (Time to Kill)

The time required to defeat a target from full HP using only auto-attack
chains at full reload cadence. Assumes no armor, no shields unless specified.

```
Parity TTK (auto only, unarmored): ~18.6 sec (~4 chains)
```

---

### Tell

A visible or audible windup animation before a hit lands. The counterplay
window. Required by Combat Law 1 on all skills.

---

### CC (Crowd Control)

Any effect that restricts the target's actions. Does not deal damage.
All CC types and durations: mechanics-v1.md §8.

---

### EHP (Effective Hit Points)

Total damage a build can absorb before defeat, calculated across all three
layers: Shields + Armor + Frame.

Not displayed to the player. Full stack: SystemRevision001.md §5.

---

## 4. EQUIPMENT TERMS

*LOCKED*

### Frame

The mech's persistent identity. Establishes structural stats, load capacity,
and base behavioral stats. Never swapped — defines the build.

---

### Weapon Set

One of two swappable groups of mainhand and offhand weapons.
Swapping sets changes all five action bar skills simultaneously.
Swap cooldown: 5.0 sec global.

---

### Mainhand

The primary weapon in a weapon set. Always owns actions 1–2.
May also claim actions 3–4 depending on skill count.

---

### Offhand

The secondary weapon in a weapon set. Always owns action 5.
May also claim actions 3–4 depending on mainhand skill count.
Offhand skills are role-specific — not leftover mainhand skills.

---

### Aux Slot

Equipment slots 1–4 (formerly: Sidearm 1–2, Defensive 1–2).
Specialized equipment — offensive, defensive, or passive.
Populates actions 7–10 (shared pool with belt slots).

---

### Utility Slot

A single equipment slot for sustain items (heal/repair).
Populates action 6. Active from L6. Pre-L6: consumable from belt slot.

---

### Belt Slot

Consumable slots 1–4. Populates actions 7–10 (shared pool with aux slots).
Identical items stack — 1 action, N charges.

---

### Action Slot Consumption Rules

```
Stacking:   identical items stack — 1 action, N charges
Linking:    named linked pairs produce fewer actions than slots
            defined at item level, not player choice
Passive:    items producing no action consume 0 action slots
Maximum:    actions 7-10 regardless of aux + belt item count
```

---

### Load Capacity

Frame stat determining what equipment can be mounted.
Governs cross-class weapon penalties. See SystemRevision001.md §11.

---

## 5. DAMAGE TERMS

*LOCKED*

### Threshold (T)

Outer soft armor or shield layer. Percentage of incoming hit stopped
before passing to the next layer.

---

### Rating (R)

Inner hard armor or shield layer. Percentage of T's remainder absorbed
before passing to the next layer.

---

### Durability (D)

Resistance to durability damage per hit. Evaluated against the full
incoming hit — not the reduced remainder. As durability pool depletes,
T and R degrade proportionally.

---

### Penetration

When an incoming hit meets or exceeds the effective threshold, it passes
through to the rating layer. Stopped hits deal nothing.

---

### Corrosive

A spell effect that applies a multiplier to armor layer durability
degradation. Affects armor only — immune: energy shields, physical
shields, weapons, frame (directly).

```
Combat source (Poison Arrow / Quill):  2× for 4 sec
Environmental (bog water):             3× continuous
```

---

### Frame Integrity

The defeat layer. Static at 200 across all builds and all levels.
Defeat occurs at zero. Critical state triggers at ≤ 40 HP (≤ 20%).

---

### Frame Critical State

```
Trigger:     frame integrity ≤ 40 HP
Mobility:    -30% movement speed
Malfunction: 15% misfire chance per shot
Expected:    ~46 chain damage vs 54 base
```

---

## 6. RESOURCE TERMS

*LOCKED*

### Heat

Commit meter. Rises with aggressive play and weapon use. Enables overdrive
at high heat. Punishes overextension with forced recovery. Not a damage type.

---

### Energy

Sustain pool. Fuels shields, heals, stealth, and defensive systems.
Passive regeneration over time.

---

### Presence

Force-reaction field. Generated through specific actions. Decays when not
actively maintained. Does not reduce damage taken or increase damage dealt.

```
PvE:   AI prioritizes high-Presence targets
PvP:   forces opponents to commit defensive resources
```

Replaces Threat in all documentation.

---

### Stamina

Physical shield resource. Drains passively while shield is raised and
per-hit based on attack type and shield material. Shield breaks when
stamina reaches zero — lockout until 50% stamina recovered.
Underlying system mirrors player energy. Specific drain values:
UNDEFINED (OD-STAM-01, OD-STAM-02, OD-STAM-03).

---

## 7. STATUS TERMS

*LOCKED*

These terms appear in document section headers to indicate design state.

| Term | Meaning |
|---|---|
| LOCKED | Finalized. Do not revise without explicit design session. |
| PROPOSED | Under discussion. Pending confirmation. |
| DERIVED | Not explicitly decided — inferred from locked documentation. |
| UNDEFINED | Known gap. Not yet resolved. Blocks dependent systems. |

---

## OPEN DECISIONS

| # | Decision | Blocks |
|---|---|---|
| OD-GLOS-01 | Family link scope — which weapon pairs and output values | Barbarian feat design |

---

*glossary-v0.md | Project Orion internal documentation*
*Version 0.1 — Initial draft. Weapon taxonomy, link hierarchy, combat terms, equipment terms, damage terms, resource terms.*
