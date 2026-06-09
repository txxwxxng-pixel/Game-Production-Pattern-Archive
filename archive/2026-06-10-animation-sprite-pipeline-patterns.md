# Animation and Sprite Pipeline Patterns

Updated: 2026-06-10

Risk label: SAFE_PATTERN
Production labels: RUNTIME_ANIMATION, ASSET_PIPELINE, DEBUG_QA

## Pattern 1 — Source assets are primary

Authoring files are the product.

```text
.aseprite
.psd
.blend
```

Generated files are build artifacts.

```text
.png
.atlas
.json
.spriteframes
.preview.gif
```

Rule:

```text
edit source
never edit exports
```

Benefits:

```text
reproducibility
automation
clean version control
safe regeneration
```

## Pattern 2 — Tags define runtime animations

Inside the source file, use tags as production units.

```text
idle
walk
run
attack_light
attack_heavy
hit
death
```

Export rule:

```text
one tag -> one runtime animation clip
```

Benefits:

```text
consistent naming
simple imports
less manual slicing
```

## Pattern 3 — Metadata sidecars travel with animation

Recommended structure:

```text
player_attack.png
player_attack.json
```

Sidecar metadata:

```yaml
frame_count:
pivot:
baseline:
events:
hitboxes:
hurtboxes:
cancel_windows:
```

This keeps animation timing, events, and hitbox authoring portable across Godot, Unity, Unreal, and custom tools.

## Pattern 4 — Baseline and pivot are locked

Every frame must preserve:

```text
same ground contact line
same pivot origin
same scale
same facing direction
```

Reject frames with:

```text
floating feet
shrinking body
pivot drift
cropped weapon
camera angle drift
```

## Pattern 5 — Normalize before atlas

Pipeline:

```text
Generated frames
↓
Canvas match
↓
Pivot match
↓
Baseline match
↓
Frame order validation
↓
Atlas build
↓
Runtime import
```

Never import unnormalized AI frames directly into a production branch.

## Pattern 6 — Atlases are production units

Suggested ownership:

```text
player_movement
player_combat
player_reactions
enemy_crab
enemy_eel
boss_phase_1
```

Avoid:

```text
mixed enemies
mixed categories
random export folders
```

## Pattern 7 — Animation approval gate

Checklist:

```text
frame count correct
pivot stable
baseline stable
silhouette readable
anticipation visible
impact frame clear
loop clean
event markers valid
runtime preview passed
```

Approval states:

```text
generated
review
needs fixes
approved
integrated
```

## Pattern 8 — Frame economy

Suggested frame allocation:

```text
idle: 4-8
walk: 6-12
basic attack: 8-16
boss attack: 12-24
death: 10-30
```

Rule:

```text
spend frames where players notice
```

## Codex task seeds

```text
Create an animation import validator that checks frame count, pivot stability, baseline consistency, alpha integrity, tag names, and missing metadata sidecars.
```

```text
Create a CLI script that exports Aseprite tags into separate PNG sheets and JSON metadata files using a stable naming convention.
```

```text
Create a runtime preview scene that loads an animation sheet and sidecar metadata, plays all clips, and overlays hitbox/hurtbox/event markers.
```
