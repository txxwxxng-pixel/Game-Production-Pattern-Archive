# Animation Factory Architecture Pattern

Updated: 2026-06-10

Risk label: SAFE_PATTERN
Production labels: RUNTIME_ANIMATION, ASSET_PIPELINE, DEBUG_QA

## Pattern

One source asset should generate multiple production outputs.

```text
character.aseprite
↓
atlas.png
animations.json
hitboxes.json
preview.gif
review_sheet.png
engine_import_report.md
```

## Factory pipeline

```text
source asset
↓
tag scan
↓
frame export
↓
metadata export
↓
normalization
↓
atlas build
↓
preview build
↓
validation
↓
runtime import
```

## Required metadata

```yaml
animation_name:
frame_count:
frame_rate:
pivot:
baseline:
events:
hitboxes:
hurtboxes:
source_hash:
export_version:
```

## Benefits

```text
single source of truth
batch production
consistent runtime imports
fast regeneration
QA-friendly previews
```

## Approval gate

An animation factory output is approved only when:

```text
source exists
metadata exists
preview exists
runtime import succeeds
validation passes
review status is approved
```

## Codex task seed

```text
Create an animation factory script that turns one tagged source file into an atlas, metadata JSON, preview GIF, hitbox sidecar, and validation report.
```
