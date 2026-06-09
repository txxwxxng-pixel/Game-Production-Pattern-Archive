# Production Pattern Core Index

Updated: 2026-06-10

Risk label: SAFE_PATTERN
Production labels: RUNTIME_ANIMATION, COMBAT_SYSTEM, ASSET_PIPELINE, VFX_GAMEFEEL, DEBUG_QA, SURVIVAL_CRAFTING, SURVIVORS_LIKE, STEAM_RELEASE

## Purpose

This index consolidates the full Batch 4-48 research stream into reusable production categories. The archive should avoid becoming a pile of isolated notes. Patterns are grouped by how they help a solo or small Steam-focused team ship faster.

## Core operating model

```text
Source / reference
↓
Pattern extraction
↓
Authoring standard
↓
Validation gate
↓
Runtime system
↓
Debug visibility
↓
Telemetry
↓
Release governance
↓
Archive entry
```

## Core categories

### 1. Runtime architecture

Patterns:

```text
state machines
command buffers
event buses
content registries
runtime authority boundaries
feature flags
object pools
runtime validators
```

Rule:

```text
centralize authority
broadcast events
keep presentation decoupled
```

### 2. Combat authoring

Patterns:

```text
attack definition assets
frame data authority
animation event contracts
hitbox / hurtbox separation
per-window hit caches
reaction profile libraries
impact profile catalogs
combat debug overlays
```

Rule:

```text
animation decides when
combat decides what
feedback decides how it feels
```

### 3. Animation and sprite production

Patterns:

```text
source asset authority
Aseprite tag-driven exports
metadata sidecars
baseline lock
pivot/origin governance
sprite normalization
atlas governance
animation approval gates
```

Rule:

```text
source files are primary
exports are build artifacts
```

### 4. AI asset factory

Patterns:

```text
reference bibles
batch generation
review lanes
quality ladders
asset lineage
style drift detection
regeneration tracking
confidence levels
```

Rule:

```text
AI output never bypasses review
```

### 5. VFX and game feel

Patterns:

```text
feedback stack
hitstop tiers
screen-shake language
VFX category budgets
readability budget
priority routing
pooled effects
```

Rule:

```text
clarity first
responsiveness second
spectacle third
```

### 6. Survivors-like systems

Patterns:

```text
run director
spawn budget
threat budget
spawn cards
upgrade pools
synergy matrices
weapon evolution networks
build archetype catalog
```

Rule:

```text
scale through modifiers and synergies
not raw item count
```

### 7. Survival and crafting loops

Patterns:

```text
resource tiers
resource sinks
station gating
unlock chains
capability gates
pressure / recovery cycles
economy simulation
crafting loop observability
```

Rule:

```text
resources must create decisions
```

### 8. Steam release operations

Patterns:

```text
store review / build review split
branch promotion ladder
release candidate freeze
patch risk classification
depot architecture
wishlist funnel tracking
build health dashboard
save migration governance
```

Rule:

```text
release is an audit process
not an upload step
```

## Promotion rule

Move an archive pattern into the core library when it satisfies at least two of these:

```text
used in 3 or more projects
engine-agnostic
survived runtime QA
reduces production time
supports automation
prevents recurring bugs
```

## High-level principle

```text
Tools before content
metadata before runtime
validation before integration
telemetry before balancing
branch promotion before release
```
