# Runtime Combat, Hitbox/Hurtbox, and VFX Game-Feel Patterns

Updated: 2026-06-08

## Source cluster

- Godot 2D sprite animation documentation: https://docs.godotengine.org/en/stable/tutorials/2d/2d_sprite_animation.html
- Godot Area2D / collision-area workflow reference: https://docs.godotengine.org/en/stable/tutorials/2d/using_area_2d.html
- Open-source 2D shooter references searched: Hypersomnia, Teeworlds, Gang Garrison 2
- Open-source survivors-like references searched: `migalvalm/vampire-survivors-clone`, `murparreira/vampire-survivors-clone-godot-4`, `vickylance/FantasiaSurvivors`

## Practical extraction

### Pattern 1 — Runtime animation first, sprite count second

Do not start by producing huge frame sheets for every action. Build a runtime state layer first, then decide where frame art is actually needed.

Recommended order:

1. Define animation states: idle, move, attack_startup, attack_active, attack_recovery, hit, die.
2. Drive state transitions through gameplay events, not only animation names.
3. Let AnimationPlayer or equivalent trigger exact gameplay markers: enable_hitbox, spawn_attack_object, play_vfx, camera_shake, recovery_done.
4. Use small frame counts for prototype readability, then upgrade only the high-frequency actions.

Implementation card:

```text
CharacterAnimationController
- current_state
- facing_dir
- requested_action
- lock_until_recovery
- play_state(state_name)
- emit_marker(marker_name)
```

Production note:

For Steam-style 2D games, animation polish should be attached to state certainty. If the combat state machine is unstable, better sprite sheets only create more rework.

---

### Pattern 2 — Attack object beats direct damage calls

Avoid putting all hit logic directly inside the player script. Use temporary attack objects or activated hitbox nodes.

Attack object fields:

```text
AttackObject
- owner_id
- team
- damage
- knockback
- hitstop_ms
- lifetime
- hit_once_per_target
- collision_shape
- source_animation_marker
```

Runtime flow:

```text
attack_startup
-> spawn or enable AttackObject
-> detect Hurtbox
-> send HitEvent
-> apply hitstop / knockback / damage / VFX
-> disable AttackObject
-> recovery
```

Why this matters:

- Keeps combat readable.
- Makes debug overlays simple.
- Allows projectiles, melee arcs, traps, explosions, and aura damage to share one pipeline.
- Prevents animation code from becoming the damage system.

---

### Pattern 3 — Hitbox/Hurtbox debug layer is a production feature

Every action game or survivors-like prototype should ship internally with a debug overlay, even if it is hidden from players.

Minimum overlay:

```text
F1: show collision areas
F2: show active attack objects
F3: show hurtboxes
F4: show enemy target vectors
F5: show damage numbers + recent hit log
```

Debug log fields:

```text
time, attacker, target, attack_id, damage, crit, knockback, hitstop_ms, target_hp_after
```

QA checks:

- Active frames match visual swing/projectile frame.
- Hurtbox does not extend too far outside readable sprite silhouette.
- Multi-hit attacks do not double-hit unless intended.
- Enemy contact damage is separate from enemy hurtbox.

---

### Pattern 4 — Game-feel should be data-driven, not hand scattered

Bundle impact response into one data object per attack type.

```text
ImpactProfile
- hitstop_ms
- camera_shake_strength
- camera_shake_time
- flash_color
- sound_id
- vfx_id
- knockback_curve
- freeze_attacker
- freeze_target
```

Use cases:

- Light attack: 25-40 ms hitstop, tiny shake.
- Heavy attack: 60-90 ms hitstop, stronger shake, bigger hit spark.
- Boss hit: minimal attacker freeze, clear target flash.
- Player damage: controller rumble/mobile vibration slot if platform supports it.

Production note:

VFX/game-feel is easiest to tune when it is centralized. A solo developer should avoid hunting through 15 scripts to adjust shake, freeze, SFX, and flash.

---

### Pattern 5 — Frame-production QC for AI sprite assets

When generating sprite sheets with AI, use strict cell and baseline rules.

QC checklist:

```text
- transparent background
- exact grid size
- identical cell dimensions
- same feet baseline
- same character scale
- same facing direction
- no text/labels/grid lines
- no clipped weapon, hair, VFX, cape
- no identity drift across frames
- no brightness drift between rows
```

Recommended production loop:

```text
prompt -> generate sheet -> slice -> baseline check -> silhouette check -> import -> animation preview -> reject/patch bad frames
```

Codex task seed:

```text
Build a sprite_sheet_qc.py script that loads a PNG sheet, verifies expected columns/rows, slices cells, estimates non-transparent bounds per frame, reports baseline drift, scale drift, clipping risk, and exports a contact sheet with warnings.
```

---

## Reusable Godot scene structure

```text
Player.tscn
- CharacterBody2D
  - SpriteRoot
    - AnimatedSprite2D or Sprite2D
    - AnimationPlayer
  - HurtboxArea2D
    - CollisionShape2D
  - AttackSocket2D
  - StateMachine
  - DebugDraw

AttackObject.tscn
- Area2D
  - CollisionShape2D
  - LifetimeTimer
  - VFXSocket

Enemy.tscn
- CharacterBody2D
  - SpriteRoot
  - HurtboxArea2D
  - ContactDamageArea2D
  - NavigationAgent2D or steering script
  - EnemyBrain
```

## Next implementation tasks

1. Create a reusable `AttackObject` scene.
2. Create an `ImpactProfile` resource/data file.
3. Create a hit event bus or combat mediator.
4. Create debug overlay toggles.
5. Add sprite sheet QC script for AI-generated sheets.
