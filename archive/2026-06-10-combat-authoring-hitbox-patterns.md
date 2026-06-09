# Combat Authoring and Hitbox Patterns

Updated: 2026-06-10

Risk label: SAFE_PATTERN
Production labels: COMBAT_SYSTEM, RUNTIME_ANIMATION, DEBUG_QA

## Pattern 1 — Attacks are data assets

Attack definitions should be editable data, not scattered code.

```yaml
attack_id:
damage:
stamina_cost:
startup:
active:
recovery:
cancel_window:
hitbox_profile:
impact_profile:
reaction_profile:
vfx_profile:
audio_profile:
```

Runtime flow:

```text
AttackData
↓
Animation Event
↓
Hitbox Runtime
↓
Combat Resolution
↓
Feedback Dispatch
```

## Pattern 2 — Frame data is source of truth

Attack timing should be authored explicitly.

```yaml
startup_frames:
active_frames:
recovery_frames:
cancel_window:
invulnerability_window:
```

Avoid:

```text
attack duration only
hardcoded timer hidden in combat code
```

Benefits:

```text
readable combat
balance iteration
reliable hitbox activation
```

## Pattern 3 — Animation emits events; combat executes logic

Animation owns timing.
Combat owns hit validation and damage.

```text
animation event:
  hit_window_open
  hit_window_close
  projectile_spawn
  combo_window_open
```

Combat system:

```text
validates target
checks duplicate hits
calculates damage
applies reaction
emits feedback event
```

Rule:

```text
animation signals
gameplay executes
```

## Pattern 4 — Four-box combat taxonomy

Use separate collision concepts.

```text
Pushbox: physical occupancy
Hurtbox: receives damage
Hitbox: applies damage
Blockbox: optional defensive validation
```

Rule:

```text
never combine pushbox and hurtbox
```

## Pattern 5 — Hitbox lifecycle

Hitboxes should exist only during active frames.

```text
Disabled
↓
Startup
↓
Active
↓
Cooldown
↓
Disabled
```

Debug overlay should show:

```text
owner
attack id
active frame
shape
damage profile
already-hit targets
```

## Pattern 6 — Per-window hit cache prevents duplicate hits

Problem:

```text
single swing hits same enemy multiple times
```

Solution:

```yaml
attack_instance:
  window_id:
  already_hit:
    - target_id
```

Reset:

```text
when active hit window closes
```

## Pattern 7 — Separate detection from resolution

Bad:

```text
hitbox directly edits HP
```

Good:

```text
Hitbox
↓
Detection
↓
HitConfirmEvent
↓
Damage Resolution
↓
Health System
↓
Feedback Event
```

Benefits:

```text
status effects
resistances
friendly-fire rules
telemetry
network readiness
```

## Pattern 8 — Visual combat debugging is mandatory

Minimum debug toggles:

```text
F1 collision
F2 hitbox
F3 hurtbox
F4 attack phase
F5 damage events
F6 target selection
```

Combat bugs become expensive when invisible. Make timing, collision, and resolution visible early.

## Codex task seeds

```text
Create AttackDefinition data assets with startup, active, recovery, cancel window, hitbox profile, impact profile, reaction profile, VFX profile, and SFX profile fields.
```

```text
Create a hitbox runtime that enables hitboxes only during animation event windows and stores a per-window already-hit target cache.
```

```text
Create a combat debug overlay that draws hitboxes, hurtboxes, attack ids, attack phase, and damage events.
```
