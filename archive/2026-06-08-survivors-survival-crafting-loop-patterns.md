# Survivors-Like and Survival/Crafting Loop Patterns

Updated: 2026-06-08

## Source cluster

- Survivors-like GitHub search references:
  - https://github.com/migalvalm/vampire-survivors-clone
  - https://github.com/Gatreh/vampire-survivors-clone
  - https://github.com/murparreira/vampire-survivors-clone-godot-4
  - https://github.com/vickylance/FantasiaSurvivors
- Open-source 2D action references:
  - https://github.com/TeamHypersomnia/Hypersomnia
  - https://github.com/teeworlds/teeworlds
  - https://github.com/Gang-Garrison-2/Gang-Garrison-2
- Steam-style commercial reference targets for pattern analysis only:
  - Vampire Survivors-style auto-attack escalation
  - Core Keeper-style mining/crafting/exploration loop
  - Top-down survival-crafting with base/resource pressure

## Pattern 1 — Survivors-like session skeleton

Core loop:

```text
spawn enemies -> auto/trigger attacks -> collect XP/resources -> level-up choice -> build synergy -> survive timer/boss -> payout meta progress
```

Minimum implementation modules:

```text
RunDirector
- elapsed_time
- threat_tier
- spawn_budget
- elite_schedule
- boss_schedule

EnemySpawner
- spawn_groups
- spawn_radius
- density_cap
- despawn_distance

UpgradeDraft
- candidate_pool
- rarity_weight
- tag_synergy
- banlist_this_run

WeaponRuntime
- cooldown
- targeting_rule
- projectile_or_area_factory
- scaling_tags
```

Production note:

A survivors-like prototype becomes fun when enemy density, attack cadence, and upgrade choice cadence are all connected. Do not tune these independently.

---

## Pattern 2 — Spawn budget instead of fixed spawn spam

Use a budget system so enemy waves scale without instantly breaking performance.

```text
SpawnBudget
- budget_per_second
- max_alive
- enemy_cost_table
- elite_cost_multiplier
- spawn_interval
- pressure_curve_by_minute
```

Example:

```text
minute 0-2: low budget, weak swarm
minute 3-5: medium budget, first ranged enemy
minute 6-8: elite inject, faster swarm
minute 9-10: boss + support trickle
```

QA checks:

- Max alive cap is respected.
- Spawn points avoid unfair instant contact.
- Enemy mix changes the player's movement decisions.
- FPS remains stable under the worst expected density.

---

## Pattern 3 — Weapon tags create upgrade synergy cheaply

Instead of hand-coding every combo, add tags.

```text
Weapon tags:
- projectile
- area
- orbit
- summon
- burn
- slow
- chain
- knockback
- resource_gain
```

Upgrade examples:

```text
+20% projectile speed -> applies to projectile tag
+1 area pulse -> applies to area tag
Burn spreads on death -> applies to burn tag
Mining damage converts to combat damage -> applies to tool tag
```

Production note:

For a solo/AI-assisted workflow, tag-based upgrades are much faster to expand than one-off upgrade scripts.

---

## Pattern 4 — Survival/crafting loop layer

Survival/crafting should not be only a recipe screen. It needs field pressure.

Core loop:

```text
explore -> gather -> craft -> defend -> unlock route -> upgrade base/tool -> deeper biome
```

Minimum modules:

```text
ResourceNode
- resource_type
- tool_requirement
- yield_table
- respawn_rule

RecipeBook
- recipe_id
- ingredients
- output
- station_required
- unlock_flag

BaseCore
- hp
- upgrade_level
- radius
- defense_slots
- nightly_threat_scale

BiomeGate
- required_item_or_core_level
- unlock_reward
```

Design rule:

Gathering must create a future defensive/combat decision. If resources only increase numbers, the loop feels flat.

---

## Pattern 5 — Merge survivors pressure with crafting progression

Hybrid structure for a Steam-style top-down game:

```text
Day phase:
- gather, mine, repair, plant, craft

Night phase:
- survivors-like enemy pressure
- auto/semiauto defenses
- player movement + manual skill timing

Between nights:
- choose permanent base upgrade
- choose temporary run modifier
```

This is useful for projects like `Moon Garden Keeper`, `Tidefall`, or any top-down survival defense prototype.

Implementation seed:

```text
Create DayNightRunDirector:
- day_timer
- night_timer
- phase_changed signal
- spawn_budget_by_night
- resource_bonus_by_day
- base_damage_multiplier_by_threat
```

---

## Pattern 6 — Data files beat hard-coded content

Recommended data folder:

```text
data/
  enemies/
  weapons/
  upgrades/
  recipes/
  resources/
  biomes/
  waves/
```

Each content item should be readable by Codex or another coding agent and editable without digging into scene logic.

Example weapon data:

```text
id: shard_orbit
name: Shard Orbit
base_damage: 8
cooldown: 1.2
tags: [orbit, projectile, magic]
scales_with: [attack_speed, projectile_count, magic_power]
impact_profile: light_magic_hit
```

## Next implementation tasks

1. Build `RunDirector` with spawn budget and max-alive cap.
2. Build `UpgradeDraft` with tag filtering and rarity weights.
3. Build `WeaponRuntime` interface shared by melee, projectile, aura, orbit, and trap weapons.
4. Build `DayNightRunDirector` for hybrid survival/crafting defense games.
5. Add CSV/JSON data schema for enemies, upgrades, recipes, and waves.
