# Runtime Motion Notes for 144Hz-era 2D Games

Status date: 2026-06-08

## Problem

Modern monitors can display 144Hz or higher, but game assets should not be produced as 144 unique images per second.

The production bottleneck must be solved by runtime animation design, not brute-force sprite-frame generation.

## Principle

Separate:

```text
animation source frame rate
from
runtime render/update rate
```

A character animation can use 4-12 source frames while the engine updates movement, interpolation, particles, shaders, and camera every frame.

## Use source frames for

- silhouette-changing poses
- hand-drawn death animations
- special attacks
- transformation
- explosions
- fluid/organic motion that cannot be faked cheaply

## Use runtime motion for

- idle breathing
- walking bob
- weapon follow-through
- dash stretch
- landing squash
- knockback
- attack trail
- UI pop
- projectile motion
- camera shake
- light pulse

## Recommended production split

```text
Character base pose/sprite frames: 30%
Runtime controller and interpolation: 30%
Particles/trails/shaders: 25%
Camera/sound/hitstop feedback: 15%
```

## Minimal general-enemy frame policy

```text
idle: 4-6 frames or tweened breathing
move: 6-8 frames
attack: 5-8 frames
hit: 2-3 frames
die: 6-10 frames
```

## Minimal boss frame policy

```text
idle: 8-12 frames
move: 8-12 frames
normal attack: 12-16 frames
special attack: 16-24 frames
phase change: 20-30 frames
death: 24-30 frames when important
```

## Runtime compensation checklist

- [ ] movement interpolation is smooth
- [ ] attack active window is clear
- [ ] hitstop exists
- [ ] camera feedback exists
- [ ] impact particles exist
- [ ] slash/trail effect exists
- [ ] sprite flash exists
- [ ] sound sync exists
- [ ] debug slow motion exists

## Tool direction without Spine

### Godot

- AnimationPlayer
- Tween
- Skeleton2D / Bone2D for simple 2D rigging
- GPUParticles2D / CPUParticles2D
- ShaderMaterial
- custom debug overlay

### Unity

- Animator
- Animation Events
- 2D Animation / Sprite Skin
- Particle System
- Cinemachine impulse
- Shader Graph or Sprite material flash

### Blender

Use for complex bosses or 3D-to-2D sprite generation.

```text
concept
↓
3D block/model
↓
rig
↓
animate
↓
orthographic camera render
↓
PNG sequence
↓
sprite sheet/QC
```

## Production takeaway

Do not solve fluidity with frame count. Solve it with runtime motion layers.
