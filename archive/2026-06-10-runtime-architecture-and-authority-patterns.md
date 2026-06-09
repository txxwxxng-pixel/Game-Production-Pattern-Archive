# Runtime Architecture and Authority Patterns

Updated: 2026-06-10

Risk label: SAFE_PATTERN
Production labels: RUNTIME_ANIMATION, COMBAT_SYSTEM, DEBUG_QA

## Pattern 1 — Authoring data vs runtime state

Never store runtime values inside authoring assets.

Bad:

```text
Sword.asset
  damage = 10
  current_cooldown = 2.1
```

Good:

```text
SwordData
  damage = 10

SwordRuntime
  current_cooldown = 2.1
```

Benefits:

```text
save stability
balance safety
multiplayer readiness
clean debugging
```

## Pattern 2 — One writer, many readers

Runtime authority should be explicit.

```text
HealthSystem writes HP
UI reads HP
VFX reads HP
Telemetry reads HP
```

Avoid:

```text
UI modifies HP
VFX modifies HP
Animation modifies HP
```

Rule:

```text
every gameplay value has one owner
```

## Pattern 3 — Command buffer before execution

Input and AI should submit intent, not directly mutate gameplay state.

```text
Input / AI
↓
Command Buffer
↓
Validation
↓
Execution
```

Benefits:

```text
input buffering
replay support
AI control
network readiness
debuggable action history
```

## Pattern 4 — Event bus decouples systems

Avoid direct dependencies:

```text
Combat directly calls UI
Combat directly calls VFX
Combat directly calls Audio
```

Use event contracts:

```yaml
DamageEvent:
  source:
  target:
  damage:
  impact_profile:
  position:
```

Subscribers:

```text
UI
VFX
Audio
Camera
Achievements
Telemetry
```

## Pattern 5 — Content registries scale production

Everything that ships should have an ID.

```text
EnemyRegistry
WeaponRegistry
AbilityRegistry
AnimationRegistry
ImpactProfileRegistry
LootRegistry
BiomeRegistry
```

Runtime lookup:

```text
content_id
↓
registry
↓
data
↓
runtime instance
```

Benefits:

```text
automation
validation
searchability
mod-readiness
telemetry correlation
```

## Pattern 6 — Runtime validators catch content errors early

Create validators for:

```text
animations
combat definitions
loot tables
spawn cards
save schemas
VFX profiles
upgrade pools
```

Output levels:

```text
ERROR: unsafe or broken
WARNING: usable but risky
INFO: advisory
```

Examples:

```text
missing animation event
duplicate content id
invalid upgrade tag
missing VFX reference
spawn cost exceeds budget
save field missing migration default
```

## Pattern 7 — Feature flags protect production stability

Experimental systems should be toggleable.

```yaml
new_combat: false
new_director: true
new_vfx_pipeline: false
experimental_ai_assets: false
```

Benefits:

```text
safe experimentation
fast rollback
A/B testing
branch stability
```

## Codex task seeds

```text
Create a runtime content registry system with JSON/YAML data loading, duplicate ID validation, and typed lookup helpers for enemies, weapons, abilities, animations, VFX, and loot.
```

```text
Create a gameplay event bus with stable event contracts for DamageEvent, EnemyKilledEvent, ItemPickedUpEvent, UpgradeSelectedEvent, and BuildHealthEvent.
```

```text
Create runtime validators that scan all content data before entering play mode and print ERROR/WARNING/INFO reports.
```
