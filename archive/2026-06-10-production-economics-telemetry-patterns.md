# Production Economics, Telemetry, and QA Patterns

Updated: 2026-06-10

Risk label: SAFE_PATTERN
Production labels: DEBUG_QA, ASSET_PIPELINE, STEAM_RELEASE

## Pattern 1 — Measure output, not activity

Track:

```text
implemented enemies per week
approved animations per week
integrated weapons per week
QA-passed builds per week
accepted AI assets per batch
```

Avoid using only:

```text
hours worked
tasks started
assets generated
```

## Pattern 2 — Content is not done until integrated

Definition of done:

```text
implemented
runtime tested
balanced
telemetry tracked
localized if needed
performance checked
QA verified
```

Not done:

```text
art complete only
```

## Pattern 3 — Production economics ledger

Track cost and maintenance.

```yaml
feature:
  creation_cost:
  maintenance_multiplier:
  reuse_score:
  runtime_risk:
```

Example:

```text
new weapon: low cost, high reuse
new boss: high cost, medium reuse
new save system: high cost, high risk
```

## Pattern 4 — Content cost classes

```text
Class A: bosses, major mechanics, new systems
Class B: enemies, weapons, biomes
Class C: variants, cosmetics, minor content
```

Use cost classes for roadmap decisions.

## Pattern 5 — Runtime cost budgets

Every major system needs runtime budgets.

```yaml
max_enemies:
max_projectiles:
max_vfx:
max_audio_emitters:
memory_budget:
frame_time_budget:
```

Rule:

```text
content must fit budget before approval
```

## Pattern 6 — Telemetry hooks

Every core loop emits telemetry.

Combat:

```text
damage dealt
damage taken
weapon used
ability used
enemy killed
player death
```

Economy:

```text
resource gain
resource spend
craft frequency
upgrade timing
```

Progression:

```text
unlock frequency
completion rate
quit points
```

## Pattern 7 — Balance from metrics first

Observe:

```text
pick rate
win rate
usage rate
completion rate
death source
resource bottleneck
```

Then interpret through playtesting.

Avoid balancing entirely by intuition.

## Pattern 8 — Regression tests protect production

Regression checks:

```text
animation frame count
pivot stability
event markers
hitbox timing
save migration
content references
build manifest
```

Preserve known-good test cases.

## Pattern 9 — Bottleneck analysis

Production flow:

```text
Design
↓
Art
↓
Animation
↓
Integration
↓
QA
↓
Release
```

Track:

```text
queue size
approval time
rework rate
blocked items
```

Improve the bottleneck before expanding capacity.

## Codex task seeds

```text
Create a production metrics dashboard markdown generator that counts approved animations, integrated enemies, validated weapons, accepted AI assets, and QA-passed builds.
```

```text
Create a telemetry event schema for combat, economy, progression, and release health events.
```

```text
Create regression tests for animation metadata, hitbox timing, save migration, and content registry references.
```
