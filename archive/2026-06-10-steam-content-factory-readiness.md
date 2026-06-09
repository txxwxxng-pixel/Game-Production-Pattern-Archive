# Steam Content Factory Readiness Pattern

Updated: 2026-06-10

Risk label: SAFE_PATTERN
Production labels: STEAM_RELEASE, ASSET_PIPELINE, DEBUG_QA

## Pattern

Steam launch requires content-factory readiness, not only a finished build.

Before launch, verify these factories exist:

```text
content creation pipeline
asset export pipeline
QA pipeline
patch pipeline
release pipeline
telemetry review pipeline
```

## Why it matters

A Steam game must survive updates, patches, bug reports, content additions, and player feedback. A one-time build is not enough.

## Readiness checklist

```text
new enemy can be added without new framework code
new animation can be exported and validated automatically
new VFX can be budgeted and pooled
new patch can be classified by risk
new build can be promoted through branches
old saves can be migrated and tested
```

## Minimum release operation path

```text
development build
↓
internal validation
↓
beta branch
↓
release candidate
↓
live branch
↓
post-launch monitoring
```

## High-value factory investments

```text
combat framework
animation factory
asset normalization
content registry
save migration tests
release checklist automation
build health dashboard
```

## Codex task seed

```text
Create a Steam content-factory readiness checklist that verifies content creation, asset export, QA, patch, release, save migration, and telemetry pipelines before launch.
```
