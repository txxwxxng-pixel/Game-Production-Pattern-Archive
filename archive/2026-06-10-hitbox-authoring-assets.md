# Hitbox Authoring Assets Pattern

Updated: 2026-06-10

Risk label: SAFE_PATTERN
Production labels: COMBAT_SYSTEM, RUNTIME_ANIMATION, DEBUG_QA

## Pattern

Hitboxes are authored content assets, not hardcoded collision logic.

```yaml
frame:
hitbox:
hurtbox:
damage_profile:
reaction_profile:
impact_profile:
```

## Recommended ownership

```text
Animation: timing and event markers
Hitbox data: shape, offset, duration
Combat system: validation and damage resolution
Feedback system: VFX, SFX, hitstop, shake
```

## Workflow

```text
animation frame
↓
visual hitbox editor
↓
save metadata
↓
validate frame data
↓
runtime load
↓
debug overlay review
```

## Avoid

```text
manual coordinate editing
hitbox directly editing HP
always-active weapon collision
animation directly applying damage
```

## Validation checks

```text
hitbox exists only during active frames
hurtbox layers are valid
no duplicate target hits unless intended
attack id exists
impact profile exists
reaction profile exists
```

## Codex task seed

```text
Create a frame-based hitbox authoring format with visual debug loading, per-frame hitbox/hurtbox data, impact profile references, reaction profile references, and duplicate-hit protection.
```
