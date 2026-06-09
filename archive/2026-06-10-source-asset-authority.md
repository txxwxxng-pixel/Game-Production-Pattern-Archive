# Source Asset Authority Pattern

Updated: 2026-06-10

Risk label: SAFE_PATTERN
Production labels: ASSET_PIPELINE, RUNTIME_ANIMATION

## Pattern

Source files are the authority. Exported files are generated artifacts.

Authoring source:

```text
.aseprite
.psd
.blend
.fbx source scene
```

Generated output:

```text
.png
.atlas
.json
.spriteframes
.preview.gif
.engine-imported asset
```

## Rule

```text
edit source files
regenerate outputs
never manually edit exports
```

## Why it matters

Manual editing of exported assets creates hidden drift. Once exported sprites, atlases, metadata, and engine imports diverge from source, the pipeline becomes difficult to reproduce and hard to debug.

## Recommended folder structure

```text
assets_source/
  characters/
  enemies/
  vfx/

assets_generated/
  sprites/
  atlases/
  metadata/
  previews/
```

## Validation

Before committing generated output, verify:

```text
source file exists
export command recorded
metadata generated
runtime import path valid
preview generated
```

## Codex task seed

```text
Create a source-asset validation script that checks each generated PNG, atlas, and JSON metadata file has a matching source asset and records the export command used to produce it.
```
