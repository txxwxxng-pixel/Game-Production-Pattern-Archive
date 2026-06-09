# Steam Release and Live Operations Patterns

Updated: 2026-06-10

Risk label: SAFE_PATTERN
Production labels: STEAM_RELEASE, DEBUG_QA

## Pattern 1 — Store review and build review are separate gates

Store review concerns:

```text
capsules
screenshots
trailer
store copy
tags
language support
mature content disclosures
```

Build review concerns:

```text
launches from Steam client
correct executable
input support
save path
settings persistence
crash-free smoke test
no debug overlay by default
```

Rule:

```text
store page communicates value
build proves reliability
```

## Pattern 2 — Branch promotion ladder

```text
local
↓
internal
↓
beta
↓
release_candidate
↓
live
```

Do not validate directly on live.

Promotion requirements:

```text
critical bugs resolved
save compatibility verified
performance baseline met
content audit passed
build manifest verified
rollback path confirmed
```

## Pattern 3 — Release candidate freeze

Stages:

```text
feature complete
↓
content lock
↓
bug fix only
↓
release candidate
↓
launch build
```

After RC:

```text
critical fixes only
no new systems
no speculative content
```

## Pattern 4 — Patch risk classification

```yaml
low:
  localization
  text
  minor UI

medium:
  balance
  data content
  minor systems

high:
  combat code
  save systems
  progression systems

critical:
  save format
  economy data
  platform deployment
```

Higher risk requires broader testing.

## Pattern 5 — Demo conversion framework

Demo must include:

```text
core loop
first progression milestone
signature mechanic
future tease
clear ending / wishlist motivation
```

Avoid:

```text
late-game grind
unfinished systems
content filler
```

Recommended continuity:

```text
demo progress can transfer to full game where feasible
```

## Pattern 6 — Wishlist funnel observability

Track:

```text
impressions
store visits
wishlists
demo downloads
purchases
reviews
```

Metrics:

```text
store CTR
wishlist conversion
demo retention
purchase conversion
review rate
```

## Pattern 7 — Build manifest audit

Before release, verify:

```text
branch target
build id
manifest
depot mapping
version number
expected files
correct executable
patch size
rollback target
```

## Pattern 8 — Update as communication

Every update should explain:

```text
goal
player benefit
main changes
known issues
follow-up monitoring
```

Better:

```text
Combat Update
Crafting Expansion
Biome Update
```

Weaker:

```text
Patch 0.7.13
```

## Pattern 9 — Live build health dashboard

Monitor:

```text
crashes
fps trend
memory trend
save failures
load failures
session length
quit locations
```

Trends matter more than single snapshots.

## Codex task seeds

```text
Create a release checklist script that validates branch name, build version, manifest contents, depot mapping notes, save compatibility test result, and rollback target.
```

```text
Create a Steam screenshot plan with five shots: core fantasy, combat pressure, progression, world/biome, boss/event.
```

```text
Create a build health dashboard markdown report template for crashes, FPS, memory, save/load failures, session length, and quit points.
```
