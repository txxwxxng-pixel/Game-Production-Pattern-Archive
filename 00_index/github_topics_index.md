# GitHub Topics Index

Status date: 2026-06-08

This file tracks GitHub topics useful for practical game-production workflow research.

## High-priority topics

### metroidvania

URL: https://github.com/topics/metroidvania

Primary value:

- 2D action platformer architecture
- player controller patterns
- animation state machine patterns
- hitbox / hurtbox handling
- boss and enemy state patterns
- camera and game-feel feedback

What to extract:

- movement controller structure
- attack box lifecycle
- animation event usage
- debug overlay if present
- tilemap / room transition handling

Risk note:

- Fan-game repositories may include copyrighted assets. Structure reference only unless license and asset provenance are clear.

---

### hollow-knight

URL: https://github.com/topics/hollow-knight

Primary value:

- modding ecosystem
- debug tools
- fan implementations
- hitbox visualization
- metroidvania combat references

What to extract:

- runtime debug overlay ideas
- combat timing tools
- mod/plugin architecture only if useful for tooling
- animation event and attack-object patterns from safe fan implementations

Risk note:

- Many repositories may relate to mods, decompilation, fan assets, or reverse engineering. Do not copy commercial code/assets.

---

### silksong

URL: https://github.com/topics/silksong

Primary value:

- Silksong-adjacent mod tools, fan projects, hitbox visualization, save tools, and analysis utilities
- useful for observing production-adjacent systems, not for official source code

What to extract:

- hitbox debug design
- state visualization
- mod/plugin tool patterns
- safe fan-project architecture if license allows

Risk note:

- Silksong official source is not public. Treat all reverse-engineered material as reference-only. Rewrite all patterns into original architecture.

---

### vampire-survivors

URL: https://github.com/topics/vampire-survivors

Primary value:

- survivors-like wave spawning
- auto-attack systems
- upgrade/evolution loops
- high enemy-count optimization
- object pooling patterns

What to extract:

- enemy spawner structure
- weapon auto-fire scheduler
- upgrade selection data model
- pooling and performance patterns
- XP/level curve systems

---

### roguelite

URL: https://github.com/topics/roguelite

Primary value:

- procedural runs
- meta progression
- replayable combat loops
- upgrade systems
- random generation patterns

What to extract:

- run state manager
- item/upgrade data definitions
- weighted random selection
- room/wave generation
- save/meta progression split

---

### survival-game

URL: https://github.com/topics/survival-game

Primary value:

- survival loop architecture
- resource gathering
- hunger/health/status systems
- enemy waves and exploration
- save/load and world state

What to extract:

- world resource node structure
- item pickup / inventory / crafting flow
- enemy spawn manager
- survival stat manager
- persistence patterns

---

### crafting-game

URL: https://github.com/topics/crafting-game

Primary value:

- recipes
- resource conversion
- building and object placement
- production chains

What to extract:

- recipe JSON/YAML structures
- inventory compatibility
- crafting station behavior
- dependency graph / unlock tree

---

### top-down

URL: https://github.com/topics/top-down

Primary value:

- top-down movement
- 2D camera patterns
- pathfinding
- interaction systems
- twin-stick and action RPG structures

What to extract:

- player movement
- enemy steering/pathfinding
- projectile patterns
- interaction zones
- top-down animation constraints

---

### godot

URL: https://github.com/topics/godot

Primary value:

- Godot templates
- plugins
- runtime animation tools
- shader examples
- tilemap and import automation

What to extract:

- scene structure conventions
- reusable scripts
- editor plugins
- AnimationPlayer/Tween usage
- TileMap/TileSet workflows

---

### unity2d

URL: https://github.com/topics/unity2d

Primary value:

- Unity 2D controllers
- animation event patterns
- Sprite Skin / 2D animation
- Cinemachine / camera feedback
- 2D physics examples

What to extract:

- component separation
- Animator + event flow
- attack hitbox prefabs
- pooling
- AssetPostprocessor patterns

---

### steamworks

URL: https://github.com/topics/steamworks

Primary value:

- Steamworks SDK integration
- GodotSteam / Steamworks.NET / Facepunch.Steamworks
- achievements, cloud save, leaderboards

What to extract:

- minimal Steam init
- achievement wrapper
- cloud save integration
- build/release checklist

---

### steam-game

URL: https://github.com/topics/steam-game

Primary value:

- Steam store tools
- Steam banner/capsule generators
- Steam data analysis
- review/sales estimation tools

What to extract:

- capsule asset workflow
- store-page data collection ideas
- release marketing support tools

## Topic research rule

For each topic, archive only the repositories that provide production workflow value. Prioritize practical implementation patterns over popularity alone.
