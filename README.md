# PROJECT ORION — INTERNAL DOCUMENTATION

### Design reference, combat systems, zone documentation, and developer tools

---

## QUICK REFERENCE

| Document | Scope | Version |
|---|---|---|
| [System Revision 001](SystemRevision001.md) | Combat laws, progression, all locked mechanics | 1.0 |
| [Combat Mechanics](mechanics-v1.md) | Weapons, action bar, skill system, BCU | 1.3 |
| [Interaction Matrix](interaction-matrix-v0.md) | Damage T/R/D coefficients, all material pairings | 1.0 |
| [Armory I](armory.md) | Warrior, Marauder, Guardian loadouts and feats | 1.1 |
| [Armory II](armory-ii.md) | Rogue, Wizard, Cleric loadouts and feats | 1.1 |
| [Codex](codex.md) | Lore, world history, systems reference | 1.1 |
| [M00 — The Bog](bog-v2.md) | Zone 0 documentation, enemy stat blocks | 2.4 |
| [Camelot Border Town](border-town-v1.md) | Zone 1 documentation, boss encounters | 1.1 |
| [UI & Game Structure](ui-v0.md) | Genre definition, menus, incursion system | 0.1 |
| [Developer Tools](tools.md) | PWA toolset — map, actor, data editors | 0.2 |

---

## STATUS KEY

| Label | Meaning |
|---|---|
| **LOCKED** | Finalized. Do not revise without explicit design session. |
| **PROPOSED** | Under discussion. Pending confirmation. |
| **DERIVED** | Inferred from other locked documentation. |
| **UNDEFINED** | Known gap. Blocks dependent systems. |

---

## BASE COMBAT UNIT

```
Reference weapon:      Gladius (Bolter)
Reference damage type: Impact (bolt — Bolter class default)
Shots per chain:       9 × 6 damage = 54 (BCU)
Chain duration:        1.5 sec
Reload (full chain):   3.15 sec
Full attack cycle:     4.65 sec
Player frame HP:       200 (static, never changes)
HP : chain ratio:      ~4:1
TTK at parity:         ~18.6 sec
```

---

## DESIGN LAWS

**Law 1** — Every ability has a tell.
**Law 2** — CC creates opportunity, not guaranteed damage.
**Law 3** — Sustained healing cannot negate sustained DPS at parity.
**Law 4** — High-damage effects require high commitment.
**Law 5** — Presence governs priority, not invulnerability.
**Law 6** — Stats are marginal. Action is primary.

**The Unification Law** — PvP and PvE share identical rules. No exceptions.

**The Belldor Encounter Design Law** — Every mandatory encounter clearable by any valid build. Fix the encounter, not the gate.

---

*README.md | Project Orion internal documentation*
