# Attack Box Patterns

Status date: 2026-06-08

## Problem

If every attack is produced as a long sprite animation, visual production cost explodes.

The practical pattern is to separate:

```text
attack animation pose
from
attack hitbox object
from
attack VFX feedback
```

## Core pattern

```text
Input
↓
Player state: ATTACK_START
↓
Animation / pose begins
↓
Timed event or animation marker spawns AttackBox
↓
AttackBox is active for a short window
↓
On overlap: damage + knockback + hitstop + particle + sound
↓
AttackBox disables collider
↓
Fade out / destroy
↓
Player state: RECOVERY or CANCEL_WINDOW
```

## Minimal AttackBox object

```text
SlashHitbox
├─ Area2D / Collider2D
├─ CollisionShape
├─ Sprite / slash visual
├─ Particle emitter
├─ optional light / shader flash
└─ Script
```

## Data fields

```json
{
  "id": "slash_basic_front",
  "damage": 10,
  "active_time": 0.08,
  "lifetime": 0.16,
  "knockback": 160,
  "hitstop": 0.045,
  "recoil": 60,
  "pierce": false,
  "spawn_offset": [32, 0],
  "shape": "capsule_or_rectangle"
}
```

## Animation event markers

Recommended markers:

```text
BeginAttack
SpawnAttackBox
EnableCancelWindow
EndAttack
```

## Debug requirements

- show active attack box
- show inactive attack box if debugging timing
- show current attack state
- show active window timer
- show hit targets
- show knockback vector
- frame step support

## Visual production impact

Instead of producing 30 slash frames:

```text
1-3 slash sprites
+ collider
+ particles
+ alpha fade
+ scale tween
+ rotation tween
+ camera impulse
+ sound
```

## Godot implementation notes

- Use Area2D for AttackBox.
- Use CollisionShape2D for active hit area.
- Use Timer or await get_tree().create_timer for lifetime.
- Use AnimationPlayer call_method_track or script-timed markers.
- Use DebugCombatOverlay to draw rectangles and labels.

## Unity implementation notes

- Use prefab with Collider2D set as trigger.
- Use Animation Event to call SpawnAttackBox.
- Use coroutine for active/lifetime windows.
- Use CinemachineImpulseSource for camera feedback.
- Use ParticleSystem for hit sparks.

## Reuse checklist

- [ ] attack data is externalized
- [ ] hitbox active time is visible in debug overlay
- [ ] attack object pools correctly
- [ ] VFX is not hardcoded inside player controller
- [ ] damage application is separated from visuals
