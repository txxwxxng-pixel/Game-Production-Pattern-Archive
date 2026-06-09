# Feedback, VFX, and Game-Feel Patterns

Updated: 2026-06-10

Risk label: SAFE_PATTERN
Production labels: VFX_GAMEFEEL, COMBAT_SYSTEM, DEBUG_QA

## Pattern 1 — Feedback stack

Every significant hit should trigger multiple feedback channels.

```text
Damage Event
↓
Feedback Stack
├─ hit stop
├─ target reaction
├─ impact VFX
├─ impact SFX
├─ camera response
└─ UI feedback
```

Rule:

```text
small layered effects beat one giant effect
```

## Pattern 2 — Combat remains functional without feedback

Feedback is presentation. Combat is authority.

```text
combat must still work
if VFX, SFX, camera shake, and UI feedback are disabled
```

Benefits:

```text
clean architecture
performance fallback
accessibility options
safe debugging
```

## Pattern 3 — Impact profile catalog

Standardize impact behavior.

```yaml
impact_profile:
  vfx:
  sfx:
  hitstop:
  camera_shake:
  reaction:
```

Common profiles:

```text
light_slash
heavy_slash
pierce
blunt
fire
ice
poison
explosion
critical
boss
```

## Pattern 4 — Hit stop governance

Use tiers.

```yaml
light: 2 frames
medium: 4 frames
heavy: 6 frames
boss: 8-10 frames
```

Avoid arbitrary values per attack.

## Pattern 5 — Screen shake as vocabulary

Treat shake as a readable language.

```text
Tier 0: none
Tier 1: minor impact
Tier 2: medium hit
Tier 3: heavy / critical hit
Tier 4: boss event
```

Rule:

```text
shake intensity tracks gameplay importance
```

## Pattern 6 — VFX readability budget

Priority order:

```text
enemy telegraph
player attack
hazard visibility
hit feedback
environment FX
ambient FX
```

Never let particles hide:

```text
player position
boss attacks
projectiles
hazards
weak points
```

## Pattern 7 — VFX lifecycle governance

Every effect defines:

```yaml
spawn:
duration:
fade:
destroy_or_recycle:
owner:
priority:
```

Reject indefinite effects without ownership.

## Pattern 8 — Pooled effects

Runtime lifecycle:

```text
request effect
↓
pool registry
↓
activate
↓
play
↓
recycle
```

Benefits:

```text
lower allocations
stable performance
central monitoring
```

## Pattern 9 — Priority routing

Feedback categories:

```text
Critical: boss attack, lethal damage, major objective
Major: elite death, rare reward, critical hit
Minor: common hit, footstep, small pickup
Ambient: weather, background movement
```

Performance fallback removes lower priority first.

## Codex task seeds

```text
Create an ImpactProfile library with VFX, SFX, hitstop, camera shake, and reaction fields. Route DamageEvent objects through the profile before dispatching feedback.
```

```text
Create a feedback router that subscribes to combat events and dispatches audio, VFX, camera, UI, and telemetry without direct combat dependencies.
```

```text
Create a VFX budget monitor that tracks active effects by category and suppresses optional effects when limits are exceeded.
```
