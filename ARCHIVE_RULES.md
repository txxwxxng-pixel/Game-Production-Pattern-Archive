# Archive Rules

## Purpose

This archive exists to shorten real game-production time. Every note should eventually help with implementation, QA, or reuse.

## What to store

- Links to public GitHub topics and repositories
- Short source summaries
- Production workflow observations
- Original diagrams and pattern descriptions
- Implementation cards for Godot, Unity, Unreal, Blender, Python, or asset tooling
- Codex-ready prompts
- QA checklists
- License and usage-risk notes

## What not to store

- Leaked source code
- Decompiled commercial source code copied verbatim
- Extracted commercial game assets
- Copyrighted sprite sheets, sound effects, textures, or music dumps
- License-unclear code pasted into this repo
- Private paid course content or paywalled text copied wholesale

## Analysis style

For every source, extract:

1. What production problem it solves
2. How the system is structured
3. What can be generalized
4. What should not be copied
5. How it maps to our projects
6. What Codex can implement from it

## Risk labels

Use these labels in analysis cards:

- SAFE_DIRECT: permissive license and reusable code/template
- SAFE_PATTERN: structure can be learned, but rewrite original code
- CAUTION_LICENSE: license or asset provenance requires review
- REFERENCE_ONLY: useful conceptually, do not copy code/assets
- DO_NOT_USE: leaked, extracted, commercial, or unsafe material

## Production value labels

- RUNTIME_ANIMATION
- COMBAT_SYSTEM
- ASSET_PIPELINE
- VFX_GAMEFEEL
- DEBUG_QA
- LEVEL_TILEMAP
- ENEMY_BOSS
- SURVIVAL_CRAFTING
- SURVIVORS_LIKE
- STEAM_RELEASE
- UNREAL_SPACE

## Update cadence

The scheduled research task should add or update archive entries regularly, with source links and practical extraction notes.
