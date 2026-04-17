# PROJECT ORION — WORLD DESIGN & MAPPING
*Version 1.0 | Status: ACTIVE*

## 1. MAPPING PARADIGM: "ANCHORED PROCEDURAL WRAPAROUND" (APW)
To make small maps feel vast, disorienting, and expansive without the memory overhead of massive open worlds, Orion uses a hybrid static/procedural mapping system. 

### 1.1 The Visual Blueprint (Map Legend)
Based on the foundational design philosophy, map generation follows strict anchor rules:
*   **White (Spawn Anchor):** The fixed entry point where the player spawns into a zone.
*   **Blue (Transition Anchor):** The exact, fixed map exit or re-entry point leading to the next zone.
*   **Warm Tones [Red/Orange/Yellow] (Major Setpieces):** Static, hand-crafted map locations. These do not change and serve as vital navigational landmarks, arenas, or dense enemy camps. 
*   **Magenta (Micro-Setpieces):** Fixed coordinate points with a tiny footprint. Used for static item spawns (e.g., a dead body with a weapon, a rusted cache with a healing item) or singular lore elements.
*   **Black (The Procedural Sea):** The space between the anchors. Anytime this space is out of the player's view, it is procedurally generated (terrain scatter, enemy spawns, minor obstacles).

### 1.2 Edge Wrapping (The "Endless World" Mechanic)
If the player wanders off the edge of the map anywhere *other* than the Blue Transition Anchor, they do not hit an invisible wall. Instead, they seamlessly wrap to another point on a different edge (e.g. crossing east edge wraps to west edge).
*   **Effect:** The map feels infinite. A player who gets lost cannot simply hug a wall to find the exit. They must use the Major Setpieces (Red/Orange) as navigational beacons to find their way to the Blue Exit.

---

## 2. CAMPAIGN ARC: LEVELS 1–5 WORLD LAYOUT

Applying the APW mapping paradigm to the Border Town introductory sequence:

### ZONE 1: M00 — THE BOG (Levels 1 to 2)
*   **White (Spawn):** The Drop-Pod crater. Safe territory.
*   **Major Setpieces (Red/Orange):** Giant ruined pylons or sinkholes. These offer islands of solid ground where Bog Crawlers spawn in dense packs, heavily guiding the player's eye forward.
*   **Micro-Setpieces (Magenta):** The fallen scout. The exact fixed location where the player finds the **Khopesh** and the **Basic Booster Kit**. 
*   **The Procedural Sea:** Deep mud, shifting reeds, and randomly spawning Bog Crawlers. If the player runs blindly without tracking setpieces, they will wrap around indefinitely.
*   **Blue (Exit):** The rusted gate into the Slums.

### ZONE 2: BORDER TOWN SLUMS (Levels 3 to 4)
*   **White (Spawn):** The inside of the rusted gate (Zone 1 exit).
*   **Major Setpieces (Red/Orange):** Scavenger Barricades and Sniper Towers. Hand-crafted arenas that force the player to practice the Gladius/Khopesh weapon swap.
*   **Micro-Setpieces (Magenta):** Hidden caches containing Healing Consumables to prep the player for the boss.
*   **The Procedural Sea:** Shifting scrap piles, moving fencing, and roaming Brute patrols. 
*   **Blue (Exit):** The heavily fortified Camelot Gatehouse Courtyard.

### ZONE 3: THE CAMELOT GATEHOUSE (Level 5 Boss)
*   **White (Spawn):** Stepping into the massive courtyard.
*   **Major Setpiece:** The entire arena is a massive static element (Boss Arena for OD-BOSS-01). The "Endless Wrap" mechanic might be disabled here, locking the player in a hard-bordered cage match to contrast the endless feeling of the previous zones.
*   **Blue (Exit):** The massive Camelot Gates. Only unlocks when the boss dies, granting the first major gear drop (Buckler or off-hand Gladius).
