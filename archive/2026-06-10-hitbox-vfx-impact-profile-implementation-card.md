# Implementation Card — Hitbox, VFX, and Impact Profile Stack

Date: 2026-06-10

## Problem

Combat polish often becomes unmaintainable when hitboxes, animation events, sound effects, camera shake, hitstop, flashes, and particles are authored directly in scripts.

This creates five production failures:

1. Every attack becomes custom code.
2. Debugging combat timing requires reading animation and script logic together.
3. VFX priority becomes inconsistent during crowded scenes.
4. AI-generated sprite frames cannot be validated against attack timing.
5. Game-feel changes require risky runtime edits.

## Production pattern

Separate attack authorship from runtime response.

```text
AttackDefinition
  owns timing, damage, hitbox windows, tags

AnimationClip / SpriteFrames
  owns visual frames only

AnimationEventContract
  maps frame index or normalized time to named events

ImpactProfile
  owns feedback stack

CombatRuntime
  resolves collision, hit cache, target filtering, and event dispatch

FeedbackRuntime
  plays hitstop, shake, flash, sound, and VFX by priority
```

## Data model

### AttackDefinition

```text
id
owner_type
animation_id
startup_frames
active_frames
recovery_frames
damage
knockback
poise_damage
hitbox_windows[]
cancel_windows[]
impact_profile_id
tags[]
```

### HitboxWindow

```text
start_frame
end_frame
shape_type
local_offset
size
rotation
team_filter
target_filter
max_hits_per_target
multi_hit_interval
```

### HurtboxDefinition

```text
body_part
shape_type
local_offset
size
damage_multiplier
armor_group
invulnerable_tags[]
```

### ImpactProfile

```text
id
hitstop_frames
attacker_pause_frames
target_pause_frames
camera_shake_id
camera_impulse_strength
target_flash_id
vfx_id
sfx_id
priority
cooldown_ms
screen_clutter_budget
```

## Runtime flow

```text
1. Animation enters attack state.
2. CombatRuntime loads AttackDefinition.
3. Frame clock advances.
4. Active hitbox windows are spawned virtually or physically.
5. Runtime queries hurtboxes.
6. Per-window hit cache prevents accidental duplicate hits.
7. Damage event is emitted.
8. ImpactProfile request is sent to FeedbackRuntime.
9. FeedbackRuntime resolves priority and clutter budget.
10. Runtime writes debug trace for replay or QA.
```

## Debug overlays

Minimum debug modes:

```text
F1: show hurtboxes
F2: show active hitboxes
F3: show attack frame windows
F4: show hit cache and target IDs
F5: show impact profile labels
F6: slow motion 25 percent
F7: single-frame advance
F8: export last combat trace
```

Overlay color language:

```text
blue: hurtbox
red: active hitbox
yellow: startup
orange: recovery
purple: invulnerability
green: successful hit
white: blocked / ignored hit
```

## Validation rules

Hard blockers:

- AttackDefinition references missing animation_id.
- HitboxWindow starts before frame 0.
- HitboxWindow ends after animation length unless explicitly looped.
- Damage attack has no active hitbox window.
- impact_profile_id is missing for player-facing attack.
- Hurtbox has zero or negative size.

Warnings:

- Active frames overlap recovery frames.
- Hitbox is much larger than sprite silhouette.
- High-priority VFX used on common low-damage hit.
- Multi-hit interval is lower than intended hitstop duration.
- Camera shake strength exceeds readability budget.

## AI sprite pipeline connection

Every generated sprite strip or atlas should produce a metadata sidecar:

```text
animation_id
frame_count
fps
pivot
feet_baseline
silhouette_bounds_by_frame[]
recommended_contact_frames[]
qc_confidence
source_prompt_id
source_reference_id
```

Combat authoring uses the sidecar to reduce manual timing work but never trusts it blindly.

```text
AI metadata suggests.
Human or validation tool approves.
Runtime consumes approved data only.
```

## Practical implementation order

1. Create AttackDefinition schema.
2. Create HitboxWindow schema.
3. Add runtime frame clock independent from visual FPS.
4. Add debug overlay before adding more attacks.
5. Add ImpactProfile schema.
6. Route all damage feedback through ImpactProfile.
7. Add combat trace export.
8. Add validation command for all attacks.

## Codex prompt

```text
Implement a data-driven 2D combat authoring foundation.

Create AttackDefinition, HitboxWindow, HurtboxDefinition, and ImpactProfile data schemas. Add a CombatRuntime that evaluates active hitbox windows against hurtboxes using a deterministic frame clock. Add per-window hit cache, target filtering, and a DamageEvent output. Add a FeedbackRuntime that receives ImpactProfile requests and resolves hitstop, camera shake, target flash, VFX, and SFX by priority. Add debug overlays and a validation command that reports blockers and warnings for every attack asset.

Do not hardcode attacks in enemy/player scripts. Existing scripts should request attack IDs and receive events.
```

## Done criteria

```text
A new attack can be added without editing runtime combat code.
A hitbox timing error can be seen in overlay within 10 seconds.
A VFX/game-feel change can be made by editing ImpactProfile data only.
A generated sprite animation can be linked to attack timing through metadata.
A combat trace can reproduce the frame, target, hitbox, and profile used for a hit.
```
