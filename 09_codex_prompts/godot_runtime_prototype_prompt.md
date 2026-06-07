# Codex Prompt: Godot Runtime Combat Prototype

```md
# Goal

Create a minimal Godot 4 prototype that demonstrates runtime animation, attack hitbox timing, VFX feedback, and debug visualization for a 2D action game.

# Core design

The prototype should avoid overproducing sprite frames. It should use a small number of placeholder sprites and rely on runtime motion, hitboxes, particles, camera shake, and debug overlays.

# Required scenes

1. `Player.tscn`
2. `SlashHitbox.tscn`
3. `TrainingEnemy.tscn`
4. `DebugToolkit.tscn`
5. `PrototypeArena.tscn`

# Required scripts

1. `PlayerMovement.gd`
2. `PlayerCombat.gd`
3. `SlashHitbox.gd`
4. `TrainingEnemy.gd`
5. `DebugToolkit.gd`
6. `Hitstop.gd`
7. `CameraFeedback.gd`

# Player requirements

- left/right movement
- jump or dash depending on prototype type
- basic attack
- attack state split into anticipation / active / recovery
- slash hitbox spawned only during active window
- player hurtbox
- simple invincibility flag placeholder

# SlashHitbox requirements

- Area2D
- CollisionShape2D
- active lifetime
- damage value
- knockback vector
- hitstop time
- optional visual sprite fade
- destroys or returns to pool after lifetime

# Enemy requirements

- hurtbox
- health
- flash on hit
- knockback
- death event

# DebugToolkit requirements

Hotkeys:

- F1: hitbox/hurtbox overlay
- F2: state labels
- F3: slow motion 0.25x
- F4: pause
- F5: frame step
- F6: screenshot capture

Display:

- player state
- attack active window
- slash hitbox rectangle
- enemy hurtbox rectangle
- FPS

# Output

Implement the prototype with clear folder structure and a README explaining controls, systems, and next steps.
```
