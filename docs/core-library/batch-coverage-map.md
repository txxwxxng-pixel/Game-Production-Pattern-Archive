# Batch Coverage Map

Updated: 2026-06-10

Purpose: confirm that the long Batch 4-48 research stream has been consolidated into repository files instead of remaining only in chat notes.

## Already existing before this full update

```text
Batch 1-3
- runtime combat / hitbox / VFX patterns
- survivors / survival / crafting loop patterns
- Steam release workflow patterns
```

Existing files:

```text
archive/2026-06-08-runtime-combat-hitbox-vfx-patterns.md
archive/2026-06-08-survivors-survival-crafting-loop-patterns.md
archive/2026-06-08-steam-release-workflow-patterns.md
```

## Consolidated in the 2026-06-10 full update

### Batch 4-10: runtime architecture, ability systems, state machines

Covered by:

```text
archive/2026-06-10-runtime-architecture-and-authority-patterns.md
docs/core-library/production-pattern-index.md
```

Key patterns:

```text
ability data vs runtime instances
tag-driven upgrades
debug overlay framework
event bus
runtime validators
single source of truth
state machine authoring
command buffers
runtime authority boundaries
```

### Batch 11-18: combat feedback, hitboxes, reactions, QA gates

Covered by:

```text
archive/2026-06-10-combat-authoring-hitbox-patterns.md
archive/2026-06-10-feedback-vfx-gamefeel-patterns.md
```

Key patterns:

```text
attack windows
frame data
hitbox lifecycle
reaction profiles
impact profile library
combat observers
hit confirm pipeline
telegraph systems
hurtbox layer taxonomy
combat debug overlay
```

### Batch 19-25: directors, survivors systems, Steam demo / playtest operations

Covered by:

```text
archive/2026-06-10-director-survivors-progression-patterns.md
archive/2026-06-10-steam-release-liveops-patterns.md
```

Key patterns:

```text
spawn directors
threat budgets
wave composition
upgrade pool management
build archetype catalog
Steam playtest funnel
branch discipline
release candidate validation
wishlist funnel tracking
```

### Batch 26-34: content registries, asset factories, animation tooling

Covered by:

```text
archive/2026-06-10-content-factory-registry-patterns.md
archive/2026-06-10-animation-sprite-pipeline-patterns.md
archive/2026-06-10-ai-asset-governance-patterns.md
```

Key patterns:

```text
content registries
combat event bus
elite modifier framework
source asset authority
Aseprite export governance
animation tag-driven production
AI sprite normalization
asset reference bibles
asset curation lanes
```

### Batch 35-43: production economics, telemetry, content lifecycle, factories

Covered by:

```text
archive/2026-06-10-production-economics-telemetry-patterns.md
archive/2026-06-10-content-factory-registry-patterns.md
docs/core-library/production-operating-system.md
```

Key patterns:

```text
runtime cost budgets
content cost scoring
production throughput metrics
content lifecycle stages
balance data separation
combat telemetry events
failed run analysis
content factories
template-driven enemies
balancing with real data
```

### Batch 44-48: vertical slice, save governance, CLI asset pipelines, Steam factory readiness

Covered by:

```text
archive/2026-06-10-vertical-slice-save-governance-patterns.md
archive/2026-06-10-source-asset-authority.md
archive/2026-06-10-cli-first-asset-pipeline.md
archive/2026-06-10-hitbox-authoring-assets.md
archive/2026-06-10-animation-factory-architecture.md
archive/2026-06-10-steam-content-factory-readiness.md
```

Key patterns:

```text
vertical slice certification
feature flags
release freeze windows
boss production layering
save schema versioning
save migration tests
CLI-first exports
source asset authority
hitbox authoring assets
animation factory architecture
Steam content factory readiness
```

## Consolidation rule

Many batch notes repeated the same pattern under slightly different names. This update intentionally consolidates duplicates into durable archive files instead of creating hundreds of near-identical Markdown files.

## Verification checklist

After this update, the repository should contain:

```text
README index updated
core-library docs added
master archive files added
explicit Batch 48 missing-path files added
coverage map added
```
