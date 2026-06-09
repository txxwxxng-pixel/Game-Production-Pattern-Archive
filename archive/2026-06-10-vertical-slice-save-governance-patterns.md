# Vertical Slice, Save Governance, and Release Reliability Patterns

Updated: 2026-06-10

Risk label: SAFE_PATTERN
Production labels: STEAM_RELEASE, DEBUG_QA

## Pattern 1 — Vertical slice certification

Prototype complete is not production ready.

Certification checklist:

```yaml
gameplay: pass
art: pass
audio: pass
save_load: pass
performance: pass
qa: pass
release_path: pass
```

A vertical slice should prove:

```text
final-quality gameplay
final-quality visuals
final-quality audio
final-quality UX
stable save/load
acceptable performance
```

It does not need all content.

## Pattern 2 — Every new system must survive a slice

Before large-scale production, prove the system inside a complete loop.

```text
1 playable loop
1 complete biome or arena
1 boss/event
1 progression path
1 save/load path
1 settings path
```

Avoid building 10 biomes or 50 weapons before this is validated.

## Pattern 3 — Content lock before polish

Stages:

```text
Prototype
↓
Vertical Slice
↓
Content Complete
↓
Content Lock
↓
Polish
↓
Release Candidate
```

After content lock:

```text
bug fixes
performance
QA
release assets
```

Avoid:

```text
new systems
major mechanics
late content expansion
```

## Pattern 4 — Save schema versioning

Every save has version metadata.

```yaml
save_version:
content_version:
migration_version:
created_build:
last_saved_build:
```

Runtime:

```text
load save
↓
check version
↓
run migration if needed
↓
validate result
↓
restore runtime
```

## Pattern 5 — Old saves are test assets

Keep example saves from:

```text
alpha
beta
demo
release candidate
live release
```

Automated tests:

```text
load validation
migration validation
missing field fallback
inventory restoration
progression restoration
settings restoration
```

## Pattern 6 — Save boundaries

Persist:

```text
progression
inventory
unlocks
settings
world state needed for continuation
```

Do not persist:

```text
temporary VFX
runtime object references
active hitboxes
transient animation state
pooled object handles
```

## Pattern 7 — Release freeze windows

Before release:

```text
feature freeze
content freeze
localization freeze
marketing asset freeze
```

Allowed after freeze:

```text
critical fixes
crash fixes
save corruption fixes
release-blocking platform fixes
```

## Pattern 8 — Build health score

Monitor:

```text
crashes
FPS
memory
save failures
load failures
progression blockers
content validation errors
```

Use trend tracking, not only single build snapshots.

## Codex task seeds

```text
Create a save schema with save_version, content_version, migration_version, and migration functions from older versions to current version.
```

```text
Create a vertical-slice certification checklist that checks gameplay loop, art/audio readiness, save/load, settings, performance, and QA status.
```

```text
Create automated old-save migration tests using archived alpha, beta, demo, and release-candidate save files.
```
