# Content Factory and Registry Patterns

Updated: 2026-06-10

Risk label: SAFE_PATTERN
Production labels: ASSET_PIPELINE, COMBAT_SYSTEM, SURVIVORS_LIKE, DEBUG_QA

## Pattern 1 — Build factories, not isolated assets

Weak production:

```text
create enemy A
create enemy B
create enemy C
```

Strong production:

```text
Enemy Factory
↓
Templates
↓
Variants
↓
Modifiers
↓
Runtime validation
```

Rule:

```text
every repeated workflow becomes a factory
```

## Pattern 2 — Template-driven enemies

Template:

```yaml
movement:
attack:
health:
loot:
vfx:
audio:
spawn_cost:
```

Enemy instance:

```yaml
template:
modifiers:
biome:
reward:
```

Benefits:

```text
faster content expansion
less duplicated code
predictable balancing
```

## Pattern 3 — Enemy families share behaviors

```text
Slime Family
├─ Small Slime
├─ Poison Slime
├─ Armored Slime
├─ Elite Slime
└─ Boss Slime
```

Shared:

```text
movement behavior
base attack language
loot category
reaction set
```

Varied:

```text
HP
damage
speed
VFX
modifiers
```

## Pattern 4 — Elite = modifier stack

Base enemy:

```text
Skeleton
```

Modifiers:

```text
Burning
Armored
Swift
Explosive
Regenerating
Shielded
```

Runtime:

```text
Base Archetype + Modifier Set
```

This multiplies encounter variety without requiring unique code per enemy.

## Pattern 5 — Content bundles ship as units

Biome bundle:

```text
enemy set
prop set
music
loot pool
encounter pool
ambient VFX
achievements/events
```

Validation:

```text
bundle complete
bundle playable
bundle testable
```

## Pattern 6 — Dependency graph prevents hidden blockers

Example:

```text
Iron Pickaxe
├─ Iron Ore
├─ Furnace
└─ Workbench II
```

Use dependency graphs for:

```text
features
biomes
enemies
crafting chains
release builds
```

## Pattern 7 — Reuse scoring

Formula:

```text
Reuse Value = Usage Count x Lifetime
```

High score:

```text
combat framework
animation library
status effect system
```

Low score:

```text
one-off cutscene asset
single-use boss gimmick
```

## Pattern 8 — Content multiplier assets

Prefer assets that multiply other assets.

```text
enemy modifier system
status effect system
upgrade synergy rules
biome palette variants
reaction profile library
```

These have higher ROI than isolated content pieces.

## Codex task seeds

```text
Create an EnemyFactory that builds runtime enemies from family templates, behavior profiles, modifier stacks, loot profiles, and spawn-cost data.
```

```text
Create a content dependency graph validator that reports missing references, circular dependencies, and blocked bundles.
```

```text
Create a content reuse score report that estimates usage count, lifetime, maintenance cost, and production value for each system or asset.
```
