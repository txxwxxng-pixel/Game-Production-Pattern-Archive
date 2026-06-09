# Director, Survivors-Like, and Progression Patterns

Updated: 2026-06-10

Risk label: SAFE_PATTERN
Production labels: SURVIVORS_LIKE, ENEMY_BOSS, DEBUG_QA

## Pattern 1 — Spawn director owns pacing

Avoid:

```text
enemies spawn themselves
level scripts spawn arbitrary waves
```

Prefer:

```text
SpawnDirector
├─ wave logic
├─ threat budget
├─ density budget
├─ elite rules
├─ biome rules
└─ boss rules
```

Rule:

```text
director is the only spawn authority
```

## Pattern 2 — Threat budget instead of enemy count

Enemy values:

```yaml
fodder: 1
ranged: 2
tank: 4
elite: 10
boss: 50
```

Wave example:

```text
budget = 20
10 fodder + 3 ranged + 1 elite = 26, rejected
6 fodder + 2 ranged + 1 elite = 20, accepted
```

Benefits:

```text
predictable difficulty
procedural encounter control
performance awareness
```

## Pattern 3 — Spawn cards

Enemies should be selectable data cards.

```yaml
id:
cost:
weight:
biomes:
tags:
elite_flags:
min_time:
max_alive:
```

Runtime:

```text
Director
↓
Eligible cards
↓
Weighted selection
↓
Spawn validation
↓
Spawn
```

## Pattern 4 — Threat is magnitude plus cadence

Track both:

```yaml
alive_threat:
spawn_rate:
pressure_curve:
```

Same enemy count can feel different depending on cadence.

```text
10 enemies every 30 seconds
2 enemies every second
```

## Pattern 5 — Upgrade pools need governance

Pools:

```text
offense
defense
mobility
economy
utility
special
```

Roll example:

```text
2 offense
1 defense
1 utility
```

Avoid fully random upgrade rolls that create dead choices.

## Pattern 6 — Build archetype catalog

Maintain explicit build categories.

```text
Projectile Swarm
Critical Burst
Summoner
Status Effect
Tank
Mobility
Resource Engine
```

New upgrades should either strengthen an existing archetype or intentionally create a new one.

## Pattern 7 — Synergy matrix beats raw item count

Weak:

```text
100 isolated upgrades
```

Strong:

```text
20 upgrades with 200 interactions
```

Track:

```text
weapon x passive
weapon x relic
weapon x character trait
weapon x evolution
```

## Pattern 8 — Weapon evolution network

Common structure:

```text
Weapon
+
Passive
=
Evolution
```

Advanced structure:

```text
Evolution
+
Secondary passive
=
Advanced evolution
```

Benefits:

```text
build planning
long-term goals
content multiplication
```

## Pattern 9 — Minimum survivors loop

Do not overbuild before the loop works.

```text
movement
↓
auto attack
↓
enemy waves
↓
XP drops
↓
level up
↓
upgrade choice
↓
stronger run
```

Do not build first:

```text
meta progression
crafting
story systems
large biome count
```

## Pattern 10 — Failed-run analysis

Track losses more carefully than wins.

```text
death time
death source
build composition
weapon usage
upgrade picks
location
active enemy density
```

Benefits:

```text
difficulty tuning
upgrade balancing
spawn pacing fixes
```

## Codex task seeds

```text
Create a SpawnDirector with threat budget, density budget, spawn card selection, max-alive caps, and off-screen spawn validation.
```

```text
Create an UpgradePool system with offense, defense, mobility, economy, utility, and special pools, plus tag-based synergy weighting.
```

```text
Create a run telemetry logger that records weapon usage, upgrade selections, death cause, time survived, enemy density, and boss attempts.
```
