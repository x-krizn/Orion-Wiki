# Marauder Class — Mechanics & Design
*Version 1.0 | Status: ACTIVE*

The **Marauder** is arguably the most aggressive branching path from the base Warrior class. In *Project Orion*, the Marauder emphasizes relentless, unyielding assault, prioritizing extreme staggering power, crowd control via brutal cleaves, and trading defensive safeguards for pure offensive momentum. The primary gameplay loop hinges on maintaining the "Frenzy" state to out-heal sustained damage through continuous attacking.

## 1. CORE MECHANICS & LORE
**Lore:** The Marauders are individuals who have embraced the savage chaos of constant war. Unlike Guardians, who defend with shields and heavy plating, or standard Warriors who balance offense and defense, Marauders fight as if every battle is their last. They are found among warring tribes, desert scavengers, and the most hardened mercenaries.
**Theme:** "Offense is the only defense."
**Primary Resource:** Stamina (rapid depletion/rapid regen via specific combos).
**Secondary Resource:** Frenzy (accumulates by dealing *and* taking damage, decays rapidly outside combat).

## 2. STAT MODIFIERS & BASE LOADOUT
*   **Base HP:** 100
*   **Base Stamina:** 120
*   **Encumbrance Limit:** Medium (Heavy armor limits frenzy buildup severely).

### 2.1 Starting Weapons (Marauder Path Requirement)
To unlock the Marauder class, the base Warrior must prioritize upgrading and mastering Two-Handed or Dual-Wield weapons.
*   **Primary:** *Rusty Greatsword* or *Twin Barbed Axes*
*   **Secondary:** *Throwing Hatchets* (Replaces the standard parry-shield/dagger with a short-range, quick-interrupt ranged option).

### 2.2 Armor Paradigm
Marauders rely on **Medium Armor**.
*   Requires enough protection to survive trading blows.
*   Cannot use Heavy Armor effectively, as it drastically increases the stamina cost of their high-momentum attacks and slows Frenzy generation.
*   **Key Stat:** *Poise.* Marauders need to swing through incoming light attacks without being staggered.

## 3. PASSIVE ABILITIES
### 3.1 Bloodlust (Class Defining Passive)
When a Marauder executes an enemy or breaks a boss's stagger bar, they regenerate 15% of their maximum HP over 3 seconds and gain a temporary movement speed buff.

### 3.2 The Frenzy Gauge
Attacking enemies, getting hit, or perfectly dodging builds **Frenzy**.
*   **Tier 1 (33%):** Attack speed +10%.
*   **Tier 2 (66%):** Attacks deal minor AoE chip damage to surrounding enemies.
*   **Tier 3 (100%):** Attacks become *Uninterruptible* (infinite Poise for the duration of the swing) and Stamina regeneration is significantly boosted.

## 4. FEAT TREE: THE RAVAGER
The Ravager tree focuses on two-handed weapons and sustained area clearing.

### 4.1 TIER 1 (Novice)
*   **[Active] Whirlwind:** A spinning attack that damages all enemies in a 360-degree radius. Drains stamina continuously until canceled or stamina depletes. High stagger damage.
*   **[Passive] Heavy Steps:** Increases base Poise while wielding a Two-Handed weapon.

### 4.2 TIER 2 (Adept)
*   **[Active] Earthshatter:** (Requires *Impact* Weapon). The Marauder brings their weapon down, creating a directional shockwave. Breaks shields instantly.
*   **[Passive] Relentless:** Hitting at least 3 enemies with *Whirlwind* grants an immediate burst of Frenzy.

### 4.3 TIER 3 (Master)
*   **[Active] Executioner's Leap:** A high-arching jump attack that tracks the target. Deals massive damage based on the target's missing HP. If it kills the target, the cooldown is reset.

## 5. FEAT TREE: THE BERSERKER
The Berserker tree focuses on dual-wielding, single-target lock-down, and taking damage to deal damage.

### 5.1 TIER 1 (Novice)
*   **[Active] Flurry:** A rapid sequence of 5 strikes. The final strike inflicts *Bleed*.
*   **[Passive] Masochist:** Taking damage directly to HP (past shields/armor mitigation) instantly grants a huge spike of Frenzy.

### 5.2 TIER 2 (Adept)
*   **[Active] Blood Price:** Consume 15% of current HP to instantly maximize the Frenzy Gauge.
*   **[Passive] Crimson Blade:** *Bleed* effects inflicted by the Marauder tick 20% faster and heal the Marauder for a microscopic amount per tick.

### 5.3 TIER 3 (Master)
*   **[Active] Death Defiance:** (Passive Cooldown: 5 mins). Upon taking fatal damage, the Marauder drops to 1 HP instead, gaining maximum Frenzy and 50% Lifesteal for 5 seconds. If they fail to kill an enemy in this window, they die.

## 6. DEFENSIVE SYSTEMS
Marauders **cannot** block effectively. If they attempt to block with a weapon, they take massive stamina damage and significant chip damage.
*   **Primary Defense:** Dodging (i-frames).
*   **Secondary Defense:** "Trading." Using high-poise attacks during Tier 3 Frenzy to hit the enemy harder than the enemy hits them.
*   **Parry Substitute (Deflect):** When dual-wielding, perfectly timing a block acts as a *Deflect*, nullifying the damage and instantly triggering the final hit of a basic combo, but it does *not* stagger the enemy like a true shield parry.

## 7. SYNERGIZING WITH THE GAME WORLD (Design Law Alignment)
The Marauder is highly effective against Swarms (Rotting Footmen) due to *Whirlwind* but struggles deeply against shielded, high-burst enemies (Siege Golems) unless they spec into *Earthshatter*. 

To progress horizontally, a Marauder player will heavily seek out mechanics that upgrade *Poise* or add *Bleed* modifiers to their weapons, making exploration for specific upgrade materials crucial over simply leveling up.