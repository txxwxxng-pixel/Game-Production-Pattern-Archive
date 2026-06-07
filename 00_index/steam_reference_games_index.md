# Steam Reference Games Index

Status date: 2026-06-08

This index connects Steam-style reference games to reusable production workflow research.

## Reference cluster A: Metroidvania / 2D action platformer

### Hollow Knight / Silksong-style references

Production patterns to extract:

- runtime player controller
- animation event timing
- hitbox/hurtbox and attack active windows
- boss phase design
- camera feedback and hitstop
- debug visualization
- room and transition systems

Archive sections:

- `01_runtime_animation/`
- `02_combat_system/`
- `04_vfx_gamefeel/`
- `05_debug_qa_tools/`
- `06_genre_patterns/metroidvania/`

---

## Reference cluster B: Survival / mining / crafting

### Core Keeper-style references

Production patterns to extract:

- top-down exploration loop
- mining and tile destruction
- resource drops
- inventory/crafting recipes
- base building
- enemy spawning
- world persistence

Archive sections:

- `03_asset_pipeline/`
- `06_genre_patterns/topdown_survival/`
- `06_genre_patterns/farming_restoration/`

---

## Reference cluster C: Builders / restoration / economy loops

### Romestead / Fortune Mill-style references

Production patterns to extract:

- settlement loop
- crafting/economy chains
- placement/building tools
- production station logic
- UI and resource readability

Archive sections:

- `06_genre_patterns/farming_restoration/`
- `06_genre_patterns/topdown_survival/`

---

## Reference cluster D: Survivors-like / auto-combat

### Tower of Babel: Survivors of Chaos / Vampire Survivors-style references

Production patterns to extract:

- auto-attack scheduler
- enemy wave manager
- upgrade selection
- weapon evolution
- object pooling
- high enemy-count performance

Archive sections:

- `06_genre_patterns/survivors_like/`
- `04_vfx_gamefeel/`
- `05_debug_qa_tools/`

---

## Reference cluster E: 2D action RPG / dark fantasy combat

### No Rest for the Wicked / Dead Cells / Hades-adjacent references

Production patterns to extract:

- input buffering
- dodge/dash windows
- attack commitment
- weapon class data
- enemy telegraph design
- impact feedback

Archive sections:

- `02_combat_system/`
- `04_vfx_gamefeel/`
- `06_genre_patterns/2d_action_rpg/`

---

## Reference cluster F: 3D/Unreal spaces and production visualization

### Cloudheim / Unreal escape room / concert hall / audio room references

Production patterns to extract:

- blockout workflow
- material libraries
- modular asset placement
- interaction triggers
- camera and presentation render workflow
- Unreal QA and packaging

Archive sections:

- `06_genre_patterns/3d_unreal_space/`
- `03_asset_pipeline/`

## Rule

Commercial reference games are used for production-pattern analysis only. Do not store extracted assets, source code, or proprietary data.
