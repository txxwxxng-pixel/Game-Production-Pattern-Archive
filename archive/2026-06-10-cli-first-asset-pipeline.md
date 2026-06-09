# CLI-First Asset Pipeline Pattern

Updated: 2026-06-10

Risk label: SAFE_PATTERN
Production labels: ASSET_PIPELINE, DEBUG_QA

## Pattern

Asset exports should be automated through command-line scripts.

```text
source changed
↓
CLI export
↓
atlas generation
↓
metadata generation
↓
validation
↓
engine import
```

## Avoid

```text
manual export
manual slicing
manual renaming
manual metadata edits
manual atlas packing
```

Manual steps scale poorly and create inconsistent builds.

## Minimum CLI pipeline

```text
export_sprites
build_atlas
generate_metadata
validate_assets
copy_to_runtime
write_report
```

## Report output

```yaml
source_files_checked:
exports_created:
metadata_created:
validation_errors:
validation_warnings:
runtime_paths:
```

## Benefits

```text
repeatable builds
lower human error
faster batch production
safe regeneration
clean CI integration
```

## Codex task seed

```text
Create a CLI asset pipeline that exports tagged sprite animations from source files, builds atlases, writes JSON metadata, validates pivots/baselines/frame counts, and produces an import report.
```
