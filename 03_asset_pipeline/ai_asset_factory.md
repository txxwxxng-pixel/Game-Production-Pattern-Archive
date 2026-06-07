# AI Asset Factory

Status date: 2026-06-08

## Goal

Shorten visual asset production by turning AI image generation into a controlled production pipeline.

Do not treat each image as a one-off prompt. Treat the process as:

```text
manifest
↓
prompt generation
↓
raw image generation
↓
automatic cleanup
↓
QC
↓
engine-ready export
```

## Folder flow

```text
asset_factory/
  00_manifest/
  01_prompts/
  02_raw_generated/
  03_processed/
  04_sheets/
  05_engine_ready/
  06_qc_reports/
```

## Asset manifest example

```json
{
  "id": "dredge_gunner",
  "type": "enemy",
  "view": "side_right",
  "style": "drowned-coast undersea",
  "animations": ["idle", "move", "attack", "hit", "die"],
  "production_method": "runtime_animation_plus_short_frame_sequences",
  "background": "transparent",
  "engine": "godot"
}
```

## What AI should produce

Good uses:

- concept art
- silhouette sheets
- key poses
- item icons
- prop ideas
- boss concept variants
- UI mood references
- part-separated character source images
- color/material variations

Avoid relying on AI for:

- perfectly consistent long frame animation
- exact baseline alignment across many generated frames
- direct engine-ready 30-frame sheets without QC
- copyrighted style/asset imitation

## Automatic processing tasks

- remove background
- crop transparent margins
- normalize canvas size
- center object
- align feet/baseline if applicable
- validate transparent background
- generate thumbnails/contact sheets
- rename files from manifest
- move passing files to engine-ready folder
- move failures to reject folder

## Sprite QC rules

For generated sprite sheets:

```text
- correct columns/rows
- equal cell dimensions
- transparent background
- no text/labels/borders/grid lines
- object not clipped
- consistent scale
- consistent baseline
- consistent facing direction
- stable silhouette
```

## Tooling candidates

- Python
- Pillow
- OpenCV
- rembg
- ImageMagick
- Aseprite CLI
- TexturePacker
- Godot import scripts
- Unity AssetPostprocessor

## Production policy

Use 30-frame sheets only when the animation truly needs unique frame-by-frame motion.

Prefer:

```text
few key frames
+ runtime animation
+ particles
+ shader/tween/trail
```

## First implementation target

Create a script that reads `asset_manifest.json` and produces:

- prompt pack markdown
- expected filename list
- post-processing task list
- QC checklist
- engine-ready folder structure
