# Reference Card: Hollow Knight / Silksong-style Production Patterns

Status date: 2026-06-08

## Risk label

REFERENCE_ONLY / SAFE_PATTERN

Silksong official source code is not public. Do not copy reverse-engineered code, extracted assets, or commercial resources. Use only production patterns and rewrite original implementations.

## Main production lesson

The production bottleneck is not solved by generating more sprite frames. A high-quality 2D action game relies on:

- runtime player controller
- state machine
- hitbox/hurtbox timing
- animation events
- attack objects
- particles/trails/shaders
- camera feedback
- hitstop
- debug visualization

## Pattern 1: Runtime animation over raw frame volume

Do not produce 144 images per second for 144Hz screens.

Instead:

```text
Few key animation frames or parts
+ engine interpolation
+ movement controller
+ particles
+ shader flashes
+ camera shake
+ hitstop
+ trails
```

Recommended frame counts:

```text
idle: 4-8 frames or skeletal/tween motion
walk/run: 6-10 frames or runtime bone/part motion
normal attack: 5-12 frames + attack object + VFX
hit: 2-4 frames + flash + knockback
special attack: 12-24 frames when necessary
death: 8-16 frames, boss death can be longer
```

## Pattern 2: Attack object lifecycle

```text
Input
↓
Change player state to ATTACK_START
↓
Play attack animation
↓
Animation event or timed marker spawns AttackBox/SlashObject
↓
Attack object enables collider for short window
↓
On hit: damage, knockback, hitstop, particles, sound
↓
Attack object fades/destroys
↓
Player enters recovery or cancel window
```

## Pattern 3: Split visual and gameplay layers

```text
Visual layer:
- sprite
- animation
- particle
- shader
- trail
- camera effect

Gameplay layer:
- health
- damage
- invincibility
- knockback
- collision
- state
- input buffer
```

This reduces the need for many unique animation frames.

## Pattern 4: Debug-first combat tuning

A production-quality 2D action game needs debug visibility:

```text
F1: show hitboxes
F2: show hurtboxes
F3: show attack active windows
F4: show projectile paths
F5: show current animation/state
F6: slow motion
F7: pause
F8: frame step
F9: screenshot QA capture
```

This turns subjective animation problems into specific tuning problems.

## Pattern 5: Useful open-source references

### lamnguyenkhoa/SilkMelody

URL: https://github.com/lamnguyenkhoa/SilkMelody

Use for:

- Unity 2D fan-game structure
- player controller decomposition
- animation event delegation
- slash object / particle / camera feedback pattern

Risk:

- Check license and asset provenance before direct use.
- Prefer SAFE_PATTERN extraction.

### Priler/HKSS.ShowHitbox

URL: https://github.com/Priler/HKSS.ShowHitbox

Use for:

- hitbox visualization concept
- slow-motion debug concept
- collider filtering by layer/type
- runtime debug overlay concept

Risk:

- Mod/reverse-engineering context. Pattern reference only.

### BitingStorm/SilkSong

URL: https://github.com/BitingStorm/SilkSong

Use for:

- engine/project separation observation
- attack box and damage system idea
- C++ custom engine layout reference

Risk:

- Fan-game / asset provenance warning. Do not copy assets. Structure reference only.

## Apply to TIDEFALL

Priority implementation:

1. Player runtime controller
2. AttackBox / SlashHitbox scene
3. Hitbox/Hurtbox debug viewer
4. Animation event or timed attack marker
5. Hitstop + camera shake + particle feedback
6. Reduced frame-count animation policy

## Codex task seed

Create a Godot prototype with:

- Player.tscn
- PlayerMovement.gd
- PlayerCombat.gd
- SlashHitbox.tscn
- SlashHitbox.gd
- DebugCombatOverlay.gd
- slow motion / pause / frame step controls
- attack active window visualization
