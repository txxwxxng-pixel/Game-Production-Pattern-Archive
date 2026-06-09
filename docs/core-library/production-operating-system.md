# Steam Indie Production Operating System

Updated: 2026-06-10

Risk label: SAFE_PATTERN
Production labels: DEBUG_QA, ASSET_PIPELINE, COMBAT_SYSTEM, SURVIVORS_LIKE, SURVIVAL_CRAFTING, STEAM_RELEASE

## Purpose

This document summarizes the repository-wide operating model that emerged from the full research stream. It is designed for solo or AI-assisted Steam game production where the bottleneck is not ideas but repeatable execution.

## Production stack

```text
Reference Library
↓
Pattern Library
↓
Authoring Tools
↓
Content Factories
↓
Runtime Systems
↓
Feedback Systems
↓
Debug Systems
↓
Validation Systems
↓
Telemetry Systems
↓
Release Systems
↓
Archive Systems
```

## Principle 1 — Frameworks create leverage

```text
frameworks create leverage
libraries create speed
content creates scope
```

Use this priority order:

```text
1. combat framework
2. animation metadata pipeline
3. asset normalization pipeline
4. debug overlay
5. validators
6. content libraries
7. individual content
```

Do not produce large volumes of enemies, weapons, animations, or VFX before the pipelines that validate and integrate them exist.

## Principle 2 — Everything important must be visible

Every important runtime system should expose either a debug overlay, a validation report, or telemetry output.

Required visibility:

```text
combat: hitboxes, hurtboxes, attack phase, damage events
animation: state, frame, event markers, pivot/baseline
spawning: spawn budget, threat budget, active count
progression: unlock state, upgrade pool, resource flow
release: build id, branch, depot, save compatibility
```

## Principle 3 — Register everything that ships

If content ships, it needs an ID.

Registries:

```text
enemy registry
weapon registry
ability registry
animation registry
impact profile registry
reaction profile registry
loot registry
biome registry
build registry
```

Benefits:

```text
searchability
automation
validation
telemetry
mod-readiness
```

## Principle 4 — Separate authority from presentation

Authority belongs to runtime systems.

Presentation belongs to feedback systems.

```text
Combat system decides hit and damage.
Animation emits timing events.
VFX displays feedback.
Audio reinforces impact.
UI displays state.
Telemetry records outcome.
```

Avoid letting animation, VFX, UI, or audio decide gameplay outcomes directly.

## Principle 5 — Measure throughput before expanding scope

Track output, not activity.

```text
approved animations per week
integrated enemies per week
validated weapons per week
QA-passed builds per week
accepted AI assets per batch
```

Use these numbers to decide whether to add more content or improve tooling.

## Principle 6 — Release is production governance

Release is not a final export. It is a governed promotion path.

```text
local
↓
internal
↓
beta
↓
release candidate
↓
live
```

Each promotion requires:

```text
save validation
performance pass
content audit
crash review
branch/depot verification
rollback path
```

## Default solo-dev rule

```text
Build one system.
Create one hundred variations.
Measure usage.
Keep the winners.
Archive the pattern.
```
