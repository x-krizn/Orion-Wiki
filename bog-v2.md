# PROJECT ORION — M00: THE BOG
### Zone Documentation — Belldor (Camelot)
*Version: 2.4 | Updated: 2026-04-11 | Status: Active*

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

1. [Zone Overview](#1-zone-overview)
2. [Environmental Structure](#2-environmental-structure)
3. [Hazard System](#3-hazard-system)
4. [Enemy Roster](#4-enemy-roster)
5. [NPC Encounter — The Orb](#5-npc-encounter--the-orb)
6. [Discoverable Content](#6-discoverable-content)
7. [Secret Boss — Lady of the Lake](#7-secret-boss--lady-of-the-lake)
8. [Player Experience Flow](#8-player-experience-flow)
9. [Combat Encounter Design](#9-combat-encounter-design)
10. [Environmental Storytelling](#10-environmental-storytelling)
11. [Zone Connections](#11-zone-connections)
12. [Open Decisions](#12-open-decisions)

---

## 1. ZONE OVERVIEW

*LOCKED*

| Field | Value |
|---|---|
| Designation | M00-THE-BOG |
| Type | Starting zone / Tutorial area |
| World | Belldor (Camelot) |
| Environmental Hazard | Corrosive bog water |
| Ætheric Status | High æther density |
| Exit | Camelot Border Town |
| Spawn Source | Player starting zone or waygate arrival |

High æther density enables Poe manifestation. This pattern is not made apparent to the player.

---

## 2. ENVIRONMENTAL STRUCTURE

*LOCKED*

**Terrain:**
- Dense maze layout of downed trees — natural obstacles, forced routing
- Ruins of old temple or monastery — architectural remnants, vertical elements, interior spaces
- Trap systems — type UNDEFINED (OD-BOG-01)
- Corrosive water hazard zones — forces navigation or damage mitigation

**Navigation:**
- Maze-like structure with multiple paths
- Environmental hazards create routing decisions
- Ruins suggest vertical elements and interior spaces for ambush and cover

---

## 3. HAZARD SYSTEM

*LOCKED*

### Corrosive Bog Water

Pilots are æther-immune (codex.md §V). Pilots are **not** immune to bog water corrosion.

Bog water is a separate hazard type from ætheric exposure. It represents a chemical/magical corrosive — not an ætheric effect.

**Mechanical effect:** Contact with bog water applies Corrosive spell effect to the armor layer only. Accelerated armor durability degradation for the duration of contact.

**Corrosive scope — LOCKED:**
```
Affects:      Armor layer only
              durability degradation multiplier applied to
              armor T/R/D calculations

Immune:       Energy shields — light does not corrode
              Physical shields — actor competence, kept clear of hazard
              Weapons — same
              Frame HP — directly unaffected
                         (indirect threat remains — armor degrades,
                          more damage reaches frame per hit)

Future:       Other hazard types may affect shields or weapons.
              Corrosive specifically does not.
```

**Corrosive multiplier model — LOCKED:**
```
Normal durability resolution:
  durability_damage = incoming × (1 - D%)

Corrosive durability resolution:
  durability_damage = incoming × (1 - D%) × corrosive_multiplier

Multipliers by source:
  Poison Arrow / Quill hit:   2× | duration: 4 sec per application
                                   stacking: duration extends, effect does not escalate
  Bog water contact:          3× | continuous while in contact

Example — reactive armor, 100 raw impact hit (D:6):
  Normal:               100 × (1-0.6) = 40 durability damage
  Under Poison/Quill:   100 × (1-0.6) × 2 = 80 durability damage
  In bog water:         100 × (1-0.6) × 3 = 120 durability damage
```

Bog water is stronger than combat corrosive because it is environmental, continuous, and avoidable through navigation. Armor does not shred instantly — it degrades faster, progressively weakening T and R over the fight.

**Tactical use:** The corrosive effect applies to any actor in contact with bog water — including enemies. Kiting the Orc Warrior through water zones accelerates its reactive armor degradation, progressively increasing effective damage per chain as the fight continues.

---

## 4. ENEMY ROSTER

*LOCKED — types, behaviors, stat blocks, and attack values.*

---

### ORC FACTION — GENERAL RULES

*LOCKED*

- Orcs are peer-scale combatants — same physical size as mechs
- Orcs always appear in pairs: one Warrior + one Scout
- Orc HP is equal across societal class — Warrior and Scout share identical frame HP
- Orc damage resolution follows the same sequential model as mechs: physical shield (if equipped) → armor → frame HP
- Organic actor resource model: UNDEFINED — pending OD-011

---

### Orc Warrior

*LOCKED — stat block, behavior, and attack values*

**Classification:** Melee Frontline
**Faction:** Orc
**Appears In:** M00-THE-BOG, M01-ORC-ENCAMPMENT, Border Town invasions
**Pair Rule:** Always accompanied by an Orc Scout. If Warrior is present, Scout is nearby.

| Stat | Value | Notes |
|---|---|---|
| Frame HP | 35 | LOCKED — BCU validated |
| Armor Type | Reactive | Deforms under sustained fire — vulnerable to AP and blast |
| Shield | Round shield (wood, mech-scale) | Active block — stamina system applies |
| Mobility | Medium | Standard movement, closes distance aggressively |
| Presence Generation | Medium | — |

**Damage Resolution:**
```
Shield raised:
  incoming → Round Shield (hardened coefficients) → Reactive Armor → Frame
Shield lowered:
  incoming → Reactive Armor → Frame

Impact vs Reactive (shield down):
  54 × (1 - 0.5) × (1 - 0.6) = 10.8 effective per chain

Impact vs Hardened → Reactive (shield up):
  54 × (1 - 0.6) × (1 - 0.4) × (1 - 0.5) × (1 - 0.6) = 3.89 effective per chain
```

**TTK (Gladius, impact, shield down):**
```
Chain 1: 35 - 10.8 = 24.2 HP
Chain 2: 24.2 - 10.8 = 13.4 HP
Chain 3: 13.4 - 10.8 = 2.6 HP
Chain 4: dead — ~18.6 sec at full reload cadence
```

**Shield Behavior:**
- Reactive block — raises shield in response to incoming fire
- Raising has a visible windup (Law 1 compliant)
- Lowering has a recovery window — counterplay exists
- Stamina pool drains passively while shield is raised
- Stamina drains per hit — drain rate varies by attack type and shield material
- Block broken if incoming drain exceeds remaining stamina — lockout until 50% stamina recovered
- Pool size and passive drain rate: UNDEFINED — pending OD-STAM-01, OD-STAM-03

**No heal mechanic.** Warrior fights to death.

**Environmental Interaction:**
Contact with corrosive bog water accelerates reactive armor durability degradation. Kiting the Warrior through water zones progressively increases effective damage per chain. Intentional — environmental awareness is a skill expression.

**Attacks — LOCKED:**

| Attack | Damage Type | Raw Damage | Effective vs Warrior L1 Reactive | Windup | Notes |
|---|---|---|---|---|---|
| Heavy Strike | Impact | 100 | 20 frame | 1.5 sec | Single heavy hit — telegraphed |
| Combo Slash (per hit) | Pierce | 30 | 10.5 frame | Sequential | 3-hit combo = 31.5 total frame damage |
| Charging Ram | Impact | 75 | 15 frame | 0.75 sec | Gap closer + knockback (0.75 sec recovery) |
| Shield Bash | Impact | 50 | 10 frame | 0.5 sec | Short range + stun 1.5 sec if stamina permits |

**Uncontested 10-second damage (player does nothing):**
```
Heavy Strike + Combo Slash + Shield Bash = 20 + 31.5 + 10 = 61.5 frame damage
Death estimate: ~17-18 sec if player does nothing
Smart play leaves player at 60-70%+ HP
```

**Stun amplifier:** Shield Bash stun (1.5 sec) = one full Gladius chain window. If Scout has Poison Arrow ready during stun, corrosive applies while player cannot respond. Punishes players who ignore Scout aggressively.

**Combat Behavior:**
Aggressive melee fighter. Closes distance. Raises shield reactively against incoming fire. Teaches player to bait shield raise and fire during recovery window. Vulnerable to environmental kiting through bog water.

**Loot Table:**

| Item | Rarity |
|---|---|
| Scrap Metal | Common |
| Orc War Paint | Uncommon |
| Crude Weapon Parts | Uncommon |

---

### Orc Scout

*LOCKED — stat block, behavior, and attack values*

**Classification:** Ranged Harassment
**Faction:** Orc
**Appears In:** M00-THE-BOG, M01-ORC-ENCAMPMENT, Border Town invasions
**Pair Rule:** Always accompanied by an Orc Warrior. Scout is the priority target.

| Stat | Value | Notes |
|---|---|---|
| Frame HP | 35 | LOCKED — BCU validated |
| Armor Type | Ablative | Sheds energy hits — low absorption once penetrated |
| Shield | None | — |
| Mobility | High | Evasive, flanking movement |
| Presence Generation | Low | — |

**Damage Resolution:**
```
incoming → Ablative Armor → Frame

Impact vs Ablative:
  54 × (1-0.4) × (1-0.3) = 22.68 effective per chain
```

**Kill Scenarios (Gladius, impact):**
```
Chain 1: 35 - 22.68 = 12.32 HP — below 50% (17.5), disengage + heal triggers
Reload:  3.15 sec → Scout heals 11.02 HP → Scout at 23.34 HP
Chain 2: 23.34 - 22.68 = 0.66 HP — barely survives on full reload cadence
Reload:  3.15 sec → Scout heals further
Chain 3: dead

Good reload management (partial reload between chains):
  Chain 2 lands before full heal — Scout dies in two chains
```

**Heal Mechanic:**
```
Trigger:    frame HP reaches 50% (17.5 HP)
Behavior:   Scout disengages, attempts to break line of sight
Heal rate:  3.5 HP/sec (10% of 35 per second)
Duration:   10 sec
Total heal: 35 HP — full reset if uninterrupted
EHP (full heal): 70
```

If the Scout fully resets, the encounter restarts from the beginning. The Warrior aggroes hard the moment the Scout's heal triggers.

**Skill Expression:**
```
Floor:    Three chains with full reloads — Scout nearly resets
Mid:      Two chains with managed reloads — Scout dies during disengage
Ceiling:  Anticipate disengage direction, maintain line of sight — clean two-chain kill
```

**Attacks — LOCKED:**

| Attack | Damage Type | Raw Damage | Effective vs Warrior L1 Reactive | Cadence | Notes |
|---|---|---|---|---|---|
| Bow Shot | Pierce | 30 | 10.5 frame | ~2 sec/shot | Standard ranged — consistent pressure |
| Poison Arrow | Pierce + Corrosive | 30 | 10.5 frame + DoT | ~2 sec/shot | Applies corrosive 2× multiplier, 4 sec |
| Smoke Bomb | — | — | — | Situational | Repositioning during disengage — obscurement |

**Uncontested 10-second damage (player ignores Scout):**
```
5 Bow Shots × 10.5 = 52.5 frame damage
Combined with Warrior: ~114 frame damage in 10 sec
```

**Combat Behavior:**
Maintains distance. Flanking positioning. Retreats when cornered. Triggers Warrior aggro at 50% HP and attempts to disengage. Teaches target prioritization — Scout is the priority kill, but the Warrior must be kited simultaneously. Fragile when caught in melee. Uses Smoke Bomb to break line of sight during disengage.

**Loot Table:**

| Item | Rarity |
|---|---|
| Primitive Arrows | Common |
| Leather Scraps | Common |
| Orc Bow | Rare |

---

### Bog Wurm

*LOCKED — stat block, behavior, and attack values*

**Classification:** Ambush Predator
**Faction:** Æther-Corrupted
**Appears In:** M00-THE-BOG

| Stat | Value | Notes |
|---|---|---|
| Frame HP | 45 | LOCKED — BCU validated |
| Armor Type | Hardened | Metallic scales — brittle under concentrated energy, strong vs pierce |
| Shield | None | — |
| Mobility | High in water, Medium on land | — |
| Presence Generation | High | Aggressive when engaged |

**Damage Resolution:**
```
incoming → Hardened Armor → Frame

Impact vs Hardened:
  54 × (1-0.6) × (1-0.4) = 12.96 effective per chain
```

**TTK (Gladius, impact):**
```
Chain 1: 45 - 12.96 = 32.04 HP (71% — chasing)
Chain 2: 32.04 - 12.96 = 19.08 HP (42% — chasing)
Chain 3: drops below 25% threshold (11.25 HP) — retreat triggers mid-chain
Chain 4: required to finish if player chases into bog water
```

**Invisibility:**
```
Initial state:    invisible — submerged in bog water
Detection:        none — underwater movement produces no acoustic signature
Reveal condition: first attack (tendril or quill)
Re-stealth:       player must fully disengage — break line of sight
                  and exit combat range
Re-stealth method: submerges back into bog water
AoE reveal:       applies at range — water does not block detection pulse
```

**Retreat and Heal Mechanic:**
```
Retreat trigger:  25% HP (11.25)
Behavior:         disengages, returns to bog water
Heal rate:        2.5% HP/sec = 1.125 HP/sec (bog water only)
Full reset time:  30 sec from 25% HP
Re-stealth:       restored on full disengage into water
```

The Wurm is a sustained attrition threat. A player who fails to push past the retreat threshold repeatedly faces a resetting, re-stealthed opponent.

**Attacks — LOCKED:**

| Attack | Damage Type | Raw Damage | Effective vs Warrior L1 Reactive | Range | Condition | Notes |
|---|---|---|---|---|---|---|
| Tendrils | Pierce | 45 | 15.75 frame | Melee — medium | Chase phase only | ~1.5 sec cadence |
| Bog Wurm Quill | Pierce + Corrosive | 30 | 10.5 frame + DoT | Projectile — medium | From stealth or while running only | Slow 2.5 sec per hit |

**Chase phase sustained damage:** 15.75 frame per hit at ~1.5 sec = ~10.5 frame/sec. Player caught in melee without response takes ~30 frame damage before a response window. Combined with corrosive degrading armor, the Wurm creates compounding pressure.

**Slow Effect — Bog Wurm Quill:**
```
Duration per hit:   2.5 sec (additive stacking)
Effect:             25% reduction to movement speed, attack speed, reload speed
Cooldowns:          unaffected — isolation rule stands (SR001 §6)
Stacking:           duration only — effect percentage does not increase
```

**Combat Behavior:**
The Wurm ambushes from stealth. Quill from stealth or while running. Switches to tendril strikes during chase phase. At 25% HP disengages back to bog water to heal and re-stealth. Player must chase into water — risking corrosive armor damage — or let it reset.

**Phase Summary:**
```
Phase 1 — Ambush:   invisible → quill from stealth (reveal) → tendril melee
Phase 2 — Chase:    tendrils sustained, quill if running
Phase 3 — Retreat:  at 25% HP, returns to water, heals, re-stealths
Phase 4 — Reset:    player must re-engage or disengage fully
```

**Grapple mechanic:** Tendrils have a grapple hit window of 10 frames (~167ms). Windup is 0.5 sec (30 frames). Player must commit to evade during windup, not during hit window. Basic dash provides no i-frames — grapple connects regardless. Booster kit evade provides i-frames by weight class sufficient to escape.

**Loot Table:**

| Item | Rarity |
|---|---|
| Corroded Metal Plating | Common |
| Wurm Scales | Uncommon |
| Ætheric Residue | Rare |
| Hydraulic Actuator | Rare |

**Design Notes:** Snake-like machine hybrid — thematic tie to Lady of the Lake. Ætheric corruption visible in design (glowing energy veins in mechanical parts). Teaches the player that some enemies weaponize the environment against them — the bog water protects the Wurm. Grapple counter mechanic must be available before this encounter is mandatory — see Belldor Encounter Design Law.

---

## 5. NPC ENCOUNTER — THE ORB

*LOCKED*

| Field | Value |
|---|---|
| Self-Designation | "Poe" |
| Classification | Poe entity |
| Condition for Encounter | Discoverable — can be "rescued" by player |
| Level Gate | Interaction with Poe is the fixed event that triggers Level 2 |
| Bog Exit | The bog exit cannot be found until the Poe interaction occurs |

**Poe Interaction Outcomes:**

| Player Choice | Outcome |
|---|---|
| Help Poe return to its lantern | Booster kit received (guaranteed) + evasion guidance (dialogue) |
| Ignore Poe | Level 2 triggered, no reward, booster kit findable in zone |
| Kill Poe | Level 2 triggered, no reward, booster kit findable in zone |

**Booster Kit (Poe reward):**
```
Type:           consumable — belt slot item
Charges:        3 uses
I-frames by weight:
  Superheavy:   11 frames (~183ms) — minimum viable, 1 frame over grapple window
  Heavy:        14 frames (~233ms)
  Medium:       18 frames (~300ms)
  Light:        22 frames (~367ms)
Energy cost per use by weight:
  Superheavy:   40
  Heavy:        30
  Medium:       20
  Light:        12
Heat generated: 10 (flat, all weights)

Note: consumable version is temporary.
      Mounted system (PNSY UF-9 Rough Rider Booster)
      replaces this when aux slots become configurable.
```

The Wurm encounter is reachable after Level 2. A player who explores aggressively before finding the Poe can encounter the Wurm without the kit. That is intentionally difficult — not a soft lock.

**Lore Context:** Poe entities manifest in high æther density zones. All Poe call themselves "Poe." The pattern of their appearance across zones is deliberate but not made apparent to the player.

---

## 6. DISCOVERABLE CONTENT

*LOCKED*

### Bedivere's Corpse

**Location:** Somewhere in the bog, en route to the border town exit.

**Items on Corpse:**

| Item | Type | Function |
|---|---|---|
| Bedivere's Signet Ring | Quest Item | Triggers loyalist hostility in Camelot Border Town. Required for dungeon subplot. |
| Numbered List | Quest Item | Cryptic item list. Collecting all items and returning to bog triggers Lady of the Lake encounter. |

**Lore Significance:** Bedivere was a knight traveling toward the border town. He did not make it. The ring proves the player looted his corpse — explains loyalist NPC hostility. The numbered list becomes meaningful only in retrospect.

**Player Decision:** Taking the ring increases border town difficulty. Leaving it means missing the dungeon subplot. The player should not understand the full weight of this decision when they make it.

---

## 7. SECRET BOSS — LADY OF THE LAKE

*LOCKED — existence and trigger. UNDEFINED — rewards (OD-005).*

**Trigger Conditions:**
1. Loot Bedivere's numbered list from his corpse
2. Collect all items referenced in the list (scattered across zones)
3. Return to M00-THE-BOG

**Boss Identity:** "Lady of the Lake" — Arthurian reference, corrupted form. Half machine, half snake. Thematically connected to Bog Wurms.

**Rewards:** UNDEFINED — pending OD-005.

**Design Intent:** Optional content gated behind thorough multi-zone item collection. Rewards players who engage with the lore and explore thoroughly. The numbered list reads as nonsense until all items are found and the pattern emerges.

---

## 8. PLAYER EXPERIENCE FLOW

*DERIVED*

### First Visit (Critical Path)

1. Spawn in bog or arrive via waygate — Level 1
2. Navigate maze of trees, ruins, traps — basic combat and movement
3. Combat encounters: Orc Warrior + Scout pairs, Bog Wurms (after Poe)
4. Find and interact with the Poe — triggers Level 2, unlocks bog exit
5. Poe interaction outcome determines booster kit acquisition
6. Discover Bedivere's corpse
7. Loot Signet Ring and numbered list
8. Exit to Camelot Border Town

### Strategic Considerations

| Choice | Consequence |
|---|---|
| Help the Poe | Booster kit guaranteed, evasion guidance, easier Wurm encounter |
| Ignore/kill the Poe | Booster kit findable but not guaranteed, no guidance |
| Take Bedivere's ring | Harder border town (loyalist hostility), enables dungeon subplot |
| Leave the ring | Easier border town, miss dungeon subplot |
| Rescue the Orb | Dialogue and lore reward in border town |
| Collect list items | Long-term quest hook, Lady of the Lake encounter |
| Kite Warrior through bog water | Accelerates armor degradation, reduces effective TTK |
| Kill Scout before 50% trigger | Prevents Warrior aggro spike and Scout reset |

### Optional Return (Post-List Completion)

1. Collect all items from Bedivere's numbered list
2. Return to M00-THE-BOG via waygate or backtracking
3. Trigger Lady of the Lake encounter
4. Defeat secret boss
5. Receive rewards (UNDEFINED — OD-005)

---

## 9. COMBAT ENCOUNTER DESIGN

*DERIVED*

**Tutorial Encounter Design:** Each enemy type teaches a distinct core mechanic.

| Enemy | Mechanic Taught |
|---|---|
| Orc Warrior | Shield baiting, melee spacing, environmental weapon use |
| Orc Scout | Target prioritization, kiting under dual threat, reload management |
| Bog Wurm | Evade timing, environmental hazard awareness, untargetable enemy behavior |

**Orc Pair Dynamic:**
The Warrior and Scout are designed as a unit, not two independent encounters. The Scout creates ranged pressure while the Warrior closes distance. The player must kite the Warrior while eliminating the Scout before its 50% heal trigger. Failure to do so results in an aggroed Warrior and a resetting Scout — a significantly harder fight that typically forces disengage.

**Encounter Compositions:**

| Type | Composition | Design Intent |
|---|---|---|
| Standard | 1 Orc Warrior + 1 Orc Scout | Core pair mechanic — kite and prioritize |
| Pressure | 1 Orc Warrior + 1 Orc Scout near water | Environmental hazard adds routing constraint |
| Ambush | 1 Bog Wurm + 1 Orc pair | Split attention, evade under dual threat |

**Difficulty Curve:**
- Early encounters: single orc pair, open terrain
- Mid-zone: orc pairs near water — environmental routing required
- Pre-exit: Bog Wurm + orc pair near water — full mechanic stack

**Belldor Encounter Design Law:** All encounters must be clearable by any valid build. No encounter requires a specific tool, mechanic, or equipment choice. The Bog Wurm encounter is available before the Poe interaction but the grapple can be avoided — once the Poe is found and Level 2 is triggered, the booster kit provides the counter. See SR001 Encounter Design Law.

---

## 10. ENVIRONMENTAL STORYTELLING

*LOCKED*

**Temple/Monastery Ruins:**
Suggests pre-catastrophe religious or scholarly presence. Ruined state implies desynchronization damage or abandonment. May contain lore documents about the catastrophe and Bedivere's mission.

**Downed Trees:**
Natural decay or catastrophic event. Creates maze-like navigation. Implies overgrown, neglected environment — forced abandonment after the catastrophe.

**High Æther Density:**
Enables Poe manifestation. Explains Bog Wurm corruption (ætheric + mechanical fusion). Suggests zone is in severe desynchronization state.

**Machine-Organic Hybrids:**
Bog Wurms and the Lady of the Lake both represent æther + technology creating unstable fusions. This is a localized expression of the broader desynchronization theme.

---

## 11. ZONE CONNECTIONS

*LOCKED*

| Connection | Details |
|---|---|
| Camelot Border Town | Primary exit. Player spawns at border town outer perimeter on arrival. Exit locked until Poe interaction. |
| M01-ORC-ENCAMPMENT | Proximity implied by shared orc enemy faction. |
| Multizone (Bedivere's List) | List items scattered across zones — connects bog to the broader world. |

---

## 12. OPEN DECISIONS

| # | Decision | Blocks |
|---|---|---|
| OD-005 | Lady of the Lake reward items | Secret boss completion |
| OD-BOG-01 | Trap types — present but undefined | Trap implementation, zone map design |
| OD-011 | Organic actor resource model — orc and wurm resource profile | All actor stat blocks in this zone |
| OD-STAM-01 | Shield broken state recovery — reduced rate, delayed start, or both | Physical shield implementation |
| OD-STAM-02 | Stamina drain per attack type vs shield material matrix | Block drain calculation |
| OD-STAM-03 | Passive stamina drain rate per shield weight class | All physical shield implementation |

---

*bog-v2.md | Project Orion internal documentation*
*Version 2.4 — Enemy attack damage values locked and BCU validated. Corrosive scope locked (armor only). Corrosive multiplier model locked. Poe interaction established as L2 fixed event and bog exit gate. Booster kit consumable defined. Grapple hit window and i-frame values locked.*
