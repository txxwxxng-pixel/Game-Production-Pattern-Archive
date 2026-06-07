# Debug Toolkit Initial Design

Status date: 2026-06-08

## Goal

Reduce animation/combat tuning time by making invisible runtime systems visible.

The toolkit should help answer:

- Which state is active?
- Which animation is playing?
- Which frame or event window is active?
- Where are hitboxes and hurtboxes?
- When does the attack become active?
- Where are projectiles spawned?
- Is camera feedback too strong?
- Is hitstop readable?

## Core controls

```text
F1: toggle hitbox/hurtbox overlay
F2: toggle attack active window overlay
F3: toggle projectile path/spawn overlay
F4: toggle animation/state label
F5: toggle AI state label
F6: slow motion 0.25x
F7: pause
F8: frame step
F9: capture QA screenshot
```

## Overlay colors

```text
red: enemy attack
blue: player attack
green: hurtbox
yellow: detection/sensor
purple: projectile spawn/path
white: terrain/collision
gray: inactive collider
orange: camera zone / transition zone
```

## Data to display

```text
current state
current animation
frame index or normalized animation time
velocity
is_grounded
attack_active
cancel_window
invincible_time
hitbox_count
projectile_count
particle_count
FPS / frame time
```

## Godot node proposal

```text
DebugToolkit
├─ DebugOverlayCanvas
├─ DebugDraw2D
├─ DebugStateLabels
├─ DebugTimeController
├─ DebugScreenshotCapture
└─ DebugRegistry
```

## Runtime registry idea

Gameplay objects register debug metadata:

```json
{
  "id": "player_slash_front_001",
  "type": "player_attack",
  "active": true,
  "rect": [10, 20, 64, 24],
  "state": "ATTACK_ACTIVE",
  "time_left": 0.04
}
```

## Why this matters

Animation bottlenecks are often tuning bottlenecks:

```text
attack looks wrong
```

becomes:

```text
attack active window starts 2 frames too early
hitbox is 20px too far forward
hitstop is too short
slash trail alpha fades too fast
camera shake starts after the hit instead of on hit
```

## First implementation target

Create a small Godot prototype with:

- debug overlay toggle
- player hurtbox rectangle
- slash hitbox rectangle
- slow motion and pause
- state label above player
- screenshot capture command
