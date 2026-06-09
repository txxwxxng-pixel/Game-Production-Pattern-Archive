# Survival, Crafting, and Economy Patterns

Updated: 2026-06-10

Risk label: SAFE_PATTERN
Production labels: SURVIVAL_CRAFTING, DEBUG_QA

## Pattern 1 — Survival loops alternate pressure and recovery

Healthy loop:

```text
Explore
↓
Gather
↓
Craft
↓
Prepare
↓
Survive / Defend
↓
Recover
↓
Expand
```

Avoid:

```text
constant pressure
constant farming
```

## Pattern 2 — Resources must create decisions

Good resource:

```text
wood
├─ building
├─ fuel
├─ repairs
└─ crafting components
```

Weak resource:

```text
single-use item with no future sink
```

Rule:

```text
every resource needs acquisition, usage, sink, and upgrade path
```

## Pattern 3 — Resource funnel

Structure:

```text
Raw Resource
↓
Processed Resource
↓
Advanced Component
↓
Major Upgrade
```

Example:

```text
Wood
↓
Plank
↓
Reinforced Frame
↓
Workshop Upgrade
```

## Pattern 4 — Resource sinks prevent inflation

Common sinks:

```text
repairs
crafting
station upgrades
consumables
rerolls
base maintenance
```

Danger signal:

```text
large unused inventory with no meaningful choice
```

## Pattern 5 — Station-centric progression

Progression:

```text
Campfire
↓
Workbench
↓
Forge
↓
Advanced Workshop
↓
Special Station
```

Stations unlock:

```text
recipes
resource tiers
automation
new capabilities
```

Avoid making all recipes available immediately.

## Pattern 6 — Capability gates beat stat inflation

Unlocks should provide:

```text
new traversal
new biome access
new crafting tier
new automation
new combat option
new building capability
```

Avoid:

```text
+5% numbers only
```

## Pattern 7 — Limit crafting dependency depth

Healthy:

```text
resource -> component -> item
```

Risky:

```text
resource -> component A -> component B -> component C -> component D -> item
```

Deep chains should be reserved for major upgrades, not basic progression.

## Pattern 8 — Economy simulation before playtesting

Simulate:

```text
resource gain
resource spend
craft costs
upgrade costs
drop rates
storage limits
```

Then playtest.

Benefits:

```text
faster balancing
earlier bottleneck detection
less grind tuning by guesswork
```

## Pattern 9 — Crafting loop observability

Track:

```text
gather rate
craft frequency
upgrade timing
resource stockpile
unused resources
failed craft attempts
```

Metrics:

```text
time to first tool
time to first station
time between major upgrades
resource bottlenecks
```

## Codex task seeds

```text
Create a resource economy audit that reports each resource's sources, sinks, recipes, upgrade path, and current usage count.
```

```text
Create a crafting station progression table where each station unlocks recipe tiers, resource tiers, and capability gates.
```

```text
Create an economy simulation script that estimates resource gain/spend over a 30-minute session and flags bottlenecks or surplus inflation.
```
