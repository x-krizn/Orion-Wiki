# PROJECT ORION — DAMAGE INTERACTION MATRIX
### Armor & Shield Material Coefficients — Threshold / Rating / Durability
*Version: 1.0 | Updated: 2026-03-29 | Status: Active*
*Depends on: SystemRevision001.md §6, §7, §8*

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

1. [Scoring System](#1-scoring-system)
2. [Damage Resolution Model](#2-damage-resolution-model)
3. [Material Definitions](#3-material-definitions)
4. [Matrix](#4-matrix)
5. [Natural Pairing Analysis](#5-natural-pairing-analysis)
6. [Comparative Summary](#6-comparative-summary)
7. [Cross-Pairings](#7-cross-pairings)
8. [Open Decisions](#8-open-decisions)

---

## 1. SCORING SYSTEM

*LOCKED*

Each matrix cell contains three scores: **T / R / D**

| Score | Meaning |
|---|---|
| T | Threshold — outer soft layer. Percentage of incoming hit stopped before passing to next layer. |
| R | Rating — inner hard layer. Percentage of T's remainder absorbed before passing to next layer. |
| D | Durability — percentage of durability damage this layer resists per hit of this type. Evaluated against the full incoming hit, not the reduced remainder. |

**Scale:**

| Score | Effectiveness | Status |
|---|---|---|
| 1 | 10% | Special — external modifier only |
| 2 | 20% | Base minimum |
| 3 | 30% | Base |
| 4 | 40% | Base |
| 5 | 50% | Base |
| 6 | 60% | Base |
| 7 | 70% | Base maximum |
| 8 | 78% | Special — external modifier only |
| 9 | 87.5% | Special — external modifier only |
| 10 | 95% | Special — external modifier only |

Base values are limited to **2–7**. Scores 1, 8, 9, and 10 are only reachable via feats, buffs, spells, or skills applied on top of base values. No base value in this matrix is outside the 2–7 range.

**Gains/losses constraint:** Every strength in one column is paid for by a weakness in another. No material may have uniformly high scores across all damage types.

---

## 2. DAMAGE RESOLUTION MODEL

*LOCKED*

### Construction Principle

Armor and shields are constructed soft outer / hard inner. The matrix reflects this. T is the outer soft layer. R is the inner hard layer. Damage passes through four layers in sequence before reaching the frame.

```
incoming hit
  → Shield T  (outer soft layer of shield)
  → Shield R  (inner hard layer of shield)
  → Armor T   (outer soft layer of armor)
  → Armor R   (inner hard layer of armor)
  → Frame
```

### Hit Resolution Formula

```
frame_damage = incoming × (1 - T_shield%) × (1 - R_shield%) × (1 - T_armor%) × (1 - R_armor%)
```

Each layer works on the remainder from the previous. Stacking two high-T layers (e.g. Hardened + Projected Light) produces diminishing returns — the second T layer only acts on what survived the first. The efficient pairing is a high-T shield into a high-R armor: the shield reduces the hit, the armor catches what gets through.

### Durability Resolution Formula

Each layer tracks its own durability pool independently. Pool size is an **item stat**, not a matrix value — a heavier plate has a larger pool, a lighter plate a smaller one. All plates of the same material type share identical matrix coefficients regardless of pool size or weight.

```
durability_damage_to_layer = incoming × (1 - D_layer%)
```

D is evaluated against the **full incoming hit** — not the reduced remainder. A layer takes durability damage based on what struck it, regardless of how much T and R deflected.

As a layer's durability pool depletes, T and R degrade proportionally:

```
T_current = base_T × (durability / max_durability)
R_current = base_R × (durability / max_durability)
```

### D Score Design Intent

A material made for a damage type stops hits well AND holds up doing it. High T and high D against a specialty type is correct — the material is designed for it. The gains/losses cost is paid in other columns and in low R (the material sheds or deflects rather than absorbs). A material not made for a damage type takes heavy durability damage even when it partially succeeds — it is burning out to hold a line it was not built for.

**Result:** A sustained attacker using a layer's weak type will eventually push T below the stop threshold, exposing R — which has almost nothing to catch what gets through. Layers fail progressively, not suddenly.

---

## 3. MATERIAL DEFINITIONS

*LOCKED*

### Armor Materials

**Hardened**
Dense rigid material — hardened plate, ceramic composite. Stops impacts by being harder than the incoming force. Brittle under energy attacks: thermal shock cracks it, plasma melts it. Once penetrated there is little to absorb — rigid materials do not deform around a penetrator. Concentrates budget into T. Low R and low D against non-specialty types.

**Reactive**
Deformable material — mild steel, backed composites. Absorbs energy through plastic deformation. Pierce penetrates more easily but the material grabs it on the way through. Impact and blast absorbed well as the material deforms to receive force. Energy attacks find it less effective — cannot deform against photons or plasma. Distributes budget across R and D.

**Ablative**
Sacrificial shedding layer — heat shield ceramic, ablative foam. Designed to vaporise and shed under energy attacks, carrying heat away from the frame. High T and high D against energy — the surface consumes and disperses the hit and holds up doing so. Low R across all types — it sheds, it does not absorb. Degrades rapidly under sustained physical attack which cuts through before shedding can occur.

### Shield Materials

**Hard Light**
Rigid crystalline energy barrier — a solid-state light construct. Reflects or stops coherent energy (beam, pulse) well and holds up doing it. Physical force overwhelms it — no give. Blast shatters it rapidly. Analogous to hardened armor in energy form.

**Projected Light**
Directional focused energy projection — a concentrated field projected in a specific direction. Deflects focused physical projectiles (pierce) well and holds up doing it. Handles wave propagation through counter-projection. Weak against blast (area force bypasses directional focus) and beam (sustained burn cuts through the projection). Analogous to ablative armor in energy form.

**Diffuse Light**
Scattered wide-coverage energy field — a distributed low-density field with broad area coverage. Disperses area effects (blast, wave) well and holds up doing it. Cannot stop focused attacks (pierce, beam) — energy density at any point is too low to concentrate resistance. Analogous to reactive armor in energy form.

---

## 4. MATRIX

*PROPOSED — values pending confirmation*

### Armor

| | Pierce | Impact | Blast | Beam | Pulse | Wave |
|---|---|---|---|---|---|---|
| **Hardened** | T:7 / R:3 / D:3 | T:6 / R:4 / D:4 | T:3 / R:2 / D:2 | T:2 / R:2 / D:2 | T:3 / R:3 / D:3 | T:4 / R:4 / D:5 |
| **Reactive** | T:3 / R:5 / D:4 | T:5 / R:6 / D:6 | T:4 / R:6 / D:5 | T:4 / R:3 / D:3 | T:4 / R:4 / D:5 | T:3 / R:4 / D:3 |
| **Ablative** | T:3 / R:2 / D:2 | T:4 / R:3 / D:4 | T:5 / R:4 / D:5 | T:6 / R:2 / D:6 | T:7 / R:2 / D:6 | T:5 / R:3 / D:5 |

### Shields

| | Pierce | Impact | Blast | Beam | Pulse | Wave |
|---|---|---|---|---|---|---|
| **Hard Light** | T:5 / R:5 / D:5 | T:3 / R:3 / D:2 | T:3 / R:3 / D:2 | T:6 / R:5 / D:5 | T:6 / R:5 / D:5 | T:4 / R:4 / D:4 |
| **Projected Light** | T:6 / R:6 / D:6 | T:5 / R:5 / D:5 | T:3 / R:3 / D:3 | T:2 / R:3 / D:3 | T:4 / R:3 / D:4 | T:5 / R:4 / D:4 |
| **Diffuse Light** | T:2 / R:2 / D:3 | T:4 / R:4 / D:4 | T:6 / R:5 / D:6 | T:4 / R:3 / D:4 | T:5 / R:4 / D:5 | T:6 / R:6 / D:6 |

All 108 values verified within 2–7 range.

---

## 5. NATURAL PAIRING ANALYSIS

*DERIVED — from matrix values above using sequential resolution formula*

Natural pairings match armor and shield by analogous design principle. The shield covers the armor's specialty gap; the armor catches what the shield lets through.

| Armor | Shield | Principle |
|---|---|---|
| Reactive | Hard Light | Both redistribute — energy-physical generalist |
| Ablative | Projected Light | Both deflect at surface — physical-first with energy coverage |
| Hardened | Diffuse Light | Both stop area/mass force — physical wall with clear energy weakness |

Frame damage per 100 incoming at full durability:

### Reactive + Hard Light

| Damage Type | Frame Takes |
|---|---|
| Pierce | 8.75 |
| Impact | 9.80 |
| Blast | 11.76 |
| Beam | 8.40 |
| Pulse | 7.20 |
| Wave | 15.12 |

**Average: 10.17 / 100 | Total: 61.03 | Weight: Heavy**
No catastrophic hole. Wave is the exploitable weakness. Buys the most consistent coverage at the highest weight cost.

---

### Ablative + Projected Light

| Damage Type | Frame Takes |
|---|---|
| Pierce | 8.96 |
| Impact | 10.50 |
| Blast | 14.70 |
| Beam | 17.92 |
| Pulse | 10.08 |
| Wave | 10.50 |

**Average: 12.11 / 100 | Total: 72.66 | Weight: Medium**
Beam is a hard weakness — projected light folds under sustained burn and ablative absorbs almost nothing once beam penetrates. All other types moderate to strong.

---

### Hardened + Diffuse Light

| Damage Type | Frame Takes |
|---|---|
| Pierce | 13.44 |
| Impact | 8.64 |
| Blast | 11.20 |
| Beam | 26.88 |
| Pulse | 14.70 |
| Wave | 5.76 |

**Average: 13.44 / 100 | Total: 80.62 | Weight: Light**
Exceptional vs Wave and Impact. Beam is catastrophic — diffuse light cannot concentrate against sustained burn, hardened armor has minimal R to catch what gets through. Lowest total budget, clearest trade.

---

## 6. COMPARATIVE SUMMARY

*DERIVED*

| Pairing | Pierce | Impact | Blast | Beam | Pulse | Wave | Total | Weight |
|---|---|---|---|---|---|---|---|---|
| Reactive + Hard Light | 8.75 | 9.80 | 11.76 | 8.40 | 7.20 | 15.12 | 61.03 | Heavy |
| Ablative + Projected | 8.96 | 10.50 | 14.70 | 17.92 | 10.08 | 10.50 | 72.66 | Medium |
| Hardened + Diffuse | 13.44 | 8.64 | 11.20 | 26.88 | 14.70 | 5.76 | 80.62 | Light |

Lower = less frame damage = better protection against that type.

The total protection gap (61.03 vs 80.62) represents the weight cost of coverage. A heavier loadout buys more consistent protection. A lighter loadout forces harder choices. The 32% difference in total frame damage exposure must be reflected in the load penalty system (SR001 §11).

---

## 7. CROSS-PAIRINGS

*DERIVED — expected profiles, not validated*

Non-natural pairings are valid. They produce more extreme profiles with harder counters. These are specialization tools, not general loadouts.

| Pairing | Expected Profile |
|---|---|
| Reactive + Projected Light | Pierce-dominant. Both layers concentrate on stopping physical projectiles. Energy coverage drops. |
| Reactive + Diffuse Light | Blast and Wave focused. Loses coherent energy coverage. |
| Ablative + Hard Light | Energy wall. Excellent Beam/Pulse resistance. Physical coverage collapses. |
| Ablative + Diffuse Light | Area effect specialist. Blast and Wave both resisted. Beam catastrophic. |
| Hardened + Hard Light | Coherent energy wall. Beam/Pulse both resisted. Blast and Wave hard counters. |
| Hardened + Projected Light | Pierce and physical focus. Two T-heavy layers — diminishing returns on stacked stops. |

---

## OPEN DECISIONS

| # | Decision | Blocks |
|---|---|---|
| OD-008 | Confirm or revise all T / R / D base values in §4 | Full armor and shield implementation |
| OD-MAT-04 | Cross-pairing policy — intentionally supported or discouraged at the design level | Loadout design, build variety |
| OD-MAT-06 | Weight cost model — how armor + shield type combinations affect frame load | Load penalty system, SR001 §11 |

---

*interaction-matrix-v0.md | Project Orion internal documentation*
*Version 1.0 — Scoring system locked. Sequential resolution model locked. Material definitions locked. Matrix values proposed. Pairing analysis derived.*
