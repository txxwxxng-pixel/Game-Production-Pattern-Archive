# Game Production Pattern Archive

Practical archive for game-production workflow research, pattern extraction, and implementation handoff.

This repository is not a clone archive and does not store leaked or extracted commercial code/assets. It stores:

- public-source links
- production analysis notes
- workflow patterns
- implementation cards
- Codex-ready task prompts
- reusable game-production templates

## Primary goal

Build a reusable production knowledge base for solo/AI-assisted Steam-style game development.

Core focus:

1. Reduce visual asset and animation production bottlenecks.
2. Extract reusable runtime animation, combat, VFX, debug, and asset pipeline patterns.
3. Convert research into Godot/Unity/Unreal implementation tasks.
4. Connect Steam reference games, GitHub topics, open repositories, devlogs, and production workflow analysis.
5. Preserve solved production problems as reusable patterns.

## Current priority tracks

1. Runtime architecture, authority boundaries, registries, and event buses
2. Runtime animation without overproducing sprite frames
3. Hitbox / hurtbox / attack definition / frame-data authoring
4. VFX and game-feel systems
5. Debug, validation, telemetry, and QA tooling
6. AI image asset factory and sprite QC
7. Survival / crafting / top-down production loops
8. Survivors-like wave, director, upgrade, and synergy systems
9. Steam release, branch promotion, demo, and live-ops workflow
10. Production economics, throughput tracking, and content factories

## Archive entries

### Initial archive entries

- `archive/2026-06-08-runtime-combat-hitbox-vfx-patterns.md`
  - Runtime animation state flow, attack-object combat, hitbox/hurtbox debug overlays, impact profiles, and AI sprite-sheet QC.
- `archive/2026-06-08-survivors-survival-crafting-loop-patterns.md`
  - Survivors-like run director, spawn budget, upgrade tag system, survival/crafting loop, and day/night hybrid defense structure.
- `archive/2026-06-08-steam-release-workflow-patterns.md`
  - Steam store/build review split, branch/channel model, screenshot planning, demo scope, launch readiness matrix, and Codex task seeds.

### Full Batch 4-48 consolidated update

- `archive/2026-06-10-runtime-architecture-and-authority-patterns.md`
  - Authoring data vs runtime state, one-writer authority, command buffers, event buses, content registries, validators, and feature flags.
- `archive/2026-06-10-combat-authoring-hitbox-patterns.md`
  - Attack definition assets, frame data, animation event contracts, combat box taxonomy, per-window hit cache, and debug overlays.
- `archive/2026-06-10-animation-sprite-pipeline-patterns.md`
  - Source asset authority, tag-driven animation export, metadata sidecars, baseline/pivot lock, normalization, atlas governance, and approval gates.
- `archive/2026-06-10-ai-asset-governance-patterns.md`
  - Reference bibles, AI asset confidence levels, batch curation, lineage tracking, style drift detection, review lanes, and gameplay QA.
- `archive/2026-06-10-feedback-vfx-gamefeel-patterns.md`
  - Feedback stack, impact profiles, hitstop tiers, screen-shake language, readability budget, VFX lifecycle, pooling, and priority routing.
- `archive/2026-06-10-director-survivors-progression-patterns.md`
  - Spawn director authority, threat budgets, spawn cards, upgrade pools, build archetypes, synergy matrix, weapon evolution, and failed-run analysis.
- `archive/2026-06-10-survival-crafting-economy-patterns.md`
  - Pressure/recovery loops, resource decisions, resource funnel, sinks, station progression, capability gates, dependency depth, and economy simulation.
- `archive/2026-06-10-steam-release-liveops-patterns.md`
  - Store/build review split, branch promotion ladder, release freezes, patch risk classification, demo conversion, wishlist funnel, build manifest, and live build health.
- `archive/2026-06-10-production-economics-telemetry-patterns.md`
  - Output metrics, definition of done, production ledger, content cost classes, runtime budgets, telemetry hooks, balancing, regression tests, and bottleneck analysis.
- `archive/2026-06-10-content-factory-registry-patterns.md`
  - Content factories, template enemies, enemy families, elite modifier stacks, bundles, dependency graphs, reuse scoring, and multiplier assets.
- `archive/2026-06-10-vertical-slice-save-governance-patterns.md`
  - Vertical slice certification, content lock, save schema versioning, old-save tests, save boundaries, release freeze windows, and build health scoring.

### Explicit Batch 48 missing-path entries

- `archive/2026-06-10-source-asset-authority.md`
  - Source files are primary; generated exports are build artifacts.
- `archive/2026-06-10-cli-first-asset-pipeline.md`
  - CLI export, atlas generation, metadata generation, validation, and engine import pipeline.
- `archive/2026-06-10-hitbox-authoring-assets.md`
  - Hitboxes as authored assets with visual editor, metadata, validation, and runtime debug overlay.
- `archive/2026-06-10-animation-factory-architecture.md`
  - One source file generates atlas, metadata, preview, documentation, and runtime import output.
- `archive/2026-06-10-steam-content-factory-readiness.md`
  - Steam launch readiness as sustainable content, QA, patch, save, and release factory readiness.

## Core library

- `docs/core-library/production-pattern-index.md`
  - Repository-wide core pattern index grouped by runtime architecture, combat, animation, AI assets, VFX, survivors systems, crafting, and Steam operations.
- `docs/core-library/production-operating-system.md`
  - Steam indie production operating model: reference library, pattern library, authoring tools, content factories, runtime systems, validation, telemetry, and release operations.
- `docs/core-library/batch-coverage-map.md`
  - Coverage map showing how Batch 4-48 research was consolidated into durable repository files.

## First reference cluster

- Silksong / Hollow Knight / Metroidvania-style runtime combat
- Vampire Survivors / Survivors-like wave and auto-attack systems
- Core Keeper-style survival, mining, crafting, top-down exploration loops
- Steamworks / store release pipeline
- Aseprite / sprite metadata / hitbox authoring pipeline
- Risk-of-Rain-style director and spawn budget architecture

## Important rule

Do not paste or preserve commercial game source code, extracted assets, leaked code, or copyrighted asset dumps. Analyze production patterns only, then rewrite into original project structures.

## How to use this archive

1. Add source/topic/repo analysis cards.
2. Extract practical production patterns.
3. Convert useful patterns into implementation task cards.
4. Hand implementation tasks to Codex or another coding agent.
5. Store the result as a reusable template.
6. Promote repeatedly useful patterns into `docs/core-library/`.

## Repository operating principle

```text
Tools before content
Metadata before runtime
Validation before integration
Telemetry before balancing
Branch promotion before release
```