# Reference Refresh 03: Steam Indie Workflow Patterns

Date: 2026-06-10
Risk label: SAFE_PATTERN
Production labels: GITHUB_TOPICS, RUNTIME_ANIMATION, COMBAT_SYSTEM, HITBOX_HURTBOX, VFX_GAMEFEEL, AI_ASSET_PIPELINE, SPRITE_FRAME_PRODUCTION, SURVIVAL_CRAFTING, SURVIVORS_LIKE, STEAM_RELEASE, SOURCE_QA

## Purpose

This entry refreshes practical production references for Steam-style indie game development and converts them into original production-pattern notes. It stores reusable workflow patterns only. It does not preserve commercial game source code, extracted assets, leaked code, or copyrighted asset dumps.

## Source cluster

- GitHub open-source game production tooling
  - https://github.blog/open-source/gaming/beyond-the-engine-10-open-source-projects-shaping-how-games-actually-get-made/
  - Production read: the useful production surface is bigger than the engine. Small teams need tooling for art, animation, level editing, audio, dialogue, debug UI, and engine-ready assets.
- Godot AnimatedSprite2D documentation
  - https://docs.godotengine.org/en/stable/classes/class_animatedsprite2d.html
  - Production read: SpriteFrames are good for simple frame playback, but gameplay-critical timing still needs explicit event contracts, metadata, or AnimationPlayer-style hooks.
- Godot community hitbox timing discussions
  - https://forum.godotengine.org/t/how-to-make-an-hitbox-spawn-according-to-an-attack-animation/92313
  - Production read: attack boxes should not be improvised in runtime code. Author startup/active/recovery windows, preview them, and validate them before integration.
- Steamworks graphical assets and store assets
  - https://partner.steamgames.com/doc/store/assets
  - https://partner.steamgames.com/doc/store/assets/standard
  - Production read: capsule, library, event, screenshot, and artwork override assets are release artifacts. They should be generated and checked like build outputs.
- OpenGame: Open Agentic Coding for Games
  - https://arxiv.org/abs/2604.18394
  - Production read: game agents need reusable Template Skill and Debug Skill memory because playable games fail through cross-file wiring and integration errors, not only isolated syntax bugs.
- Game engine architecture research
  - https://arxiv.org/abs/2004.05705
  - https://arxiv.org/abs/2406.05487
  - Production read: engine-like projects have subsystem coupling risks. Treat runtime animation, combat, VFX, asset import, save, telemetry, and release as explicit subsystems with dependency boundaries.
- AI-generated contribution quality risk
  - https://www.pcgamer.com/software/platforms/open-source-game-engine-godot-is-drowning-in-ai-slop-code-contributions-i-dont-know-how-long-we-can-keep-it-up/
  - Production read: AI output volume without review creates maintenance debt. Use provenance, test artifacts, and human verification gates.

## Extracted production patterns

### 1. Toolchain-First Indie Production Pattern

Problem:

Steam-style indie production bottlenecks usually appear outside the engine: sprite cleanup, frame naming, UI export, asset validation, screenshots, build packaging, and store asset variants.

Pattern:

```text
reference scan
-> source asset
-> export script
-> validation script
-> manifest
-> engine import
-> playable test scene
-> release package
```

Rules:

- Treat tools as production features, not optional helpers.
- Every recurring manual export needs a CLI or scripted equivalent.
- Store source files, generated outputs, validation logs, and manifests separately.
- A pattern is not production-ready until it has a repeatable handoff path to Codex or another coding agent.
- Build a small validation scene for every asset category before scaling content volume.

Codex task seed:

```text
Create an asset_pipeline_spine tool that scans source assets, runs category-specific exporters, validates naming/resolution/transparency/metadata, writes asset_manifest.yaml, and reports files not ready for engine import.
```

### 2. Runtime Animation Contract Pattern

Problem:

Sprite playback is visual, but combat, VFX, SFX, footsteps, invulnerability, and cancel windows require stable gameplay timing.

Pattern:

```text
AnimationClip or SpriteFrames
-> clip metadata sidecar
-> event keys
-> AnimationEventRegistry
-> runtime subscribers
-> debug event trace
```

Rules:

- Animation frames decide presentation timing; gameplay systems consume named events.
- Never let unrelated systems hard-code frame indices.
- Use normalized time, frame index, clip id, and actor id in every emitted event.
- Validate missing or duplicate gameplay-critical events in CI.
- Cosmetic events may fail soft; gameplay events must fail loudly before release builds.

Codex task seed:

```text
Implement AnimationEventRegistry and clip sidecar validation. Emit debug traces for every animation event and block final packaging if a gameplay-critical event is unknown, duplicated, or outside the clip frame range.
```

### 3. Hitbox/Hurtbox Authoring Asset Pattern

Problem:

Combat tuning becomes untestable when hitboxes are implicit node toggles or animation-timeline side effects.

Pattern:

```text
AttackDefinition
-> frame windows
-> hitbox/hurtbox/sensor shapes
-> scrub preview
-> overlap simulation
-> runtime import
-> debug overlay
```

Rules:

- Author combat boxes as data assets, not only scene-node arrangements.
- Separate hurtbox, hitbox, grabbox, sensor, shield, projectile, and environmental volumes.
- Required windows: startup, active, recovery, cancel, invulnerable, armor, cooldown.
- Export review frames showing sprite, origin, baseline, active boxes, and frame number.
- Every attack definition must declare hit rules: once-per-target, multi-hit interval, pierce, knockback, and friendly-fire policy.

Codex task seed:

```text
Build CombatBoxAuthoringData with schema validation and a scrub preview scene. Generate PNG overlay frames and JSON validation errors for missing active windows, invalid frame ranges, zero-area boxes, and mismatched sprite lengths.
```

### 4. Impact Profile Game-Feel Pattern

Problem:

Combat feels weak when damage, hitstop, camera shake, VFX, flash, and sound are tuned in separate files.

Pattern:

```text
AttackConnect
-> CombatResult
-> ImpactProfile
-> FeedbackPriorityRouter
-> hitstop / camera / VFX / SFX / flash / UI
-> suppression + telemetry
```

Rules:

- Define impact tiers: graze, light, medium, heavy, boss, fatal, environmental.
- Keep hitstop duration, camera impulse, VFX id, SFX id, flash settings, and UI response in one profile.
- Use suppression windows so major hits temporarily silence low-priority visual/audio clutter.
- Pool VFX and cap concurrent effects by readability and performance budget.
- Record telemetry for impact tier, feedback ids, suppression count, and player readability failures.

Codex task seed:

```text
Create ImpactProfile resources and a FeedbackPriorityRouter. Route combat results into feedback subscribers while enforcing per-tier VFX caps, hitstop limits, camera-shake priority, and telemetry export.
```

### 5. AI Asset Provenance + QC Gate Pattern

Problem:

AI image generation can produce volume quickly, but untracked outputs cause style drift, licensing confusion, engine-import failures, and repeated cleanup work.

Pattern:

```text
reference pack
-> prompt template
-> batch generation
-> selection
-> cleanup
-> QC manifest
-> engine-ready export
-> production usage record
```

Rules:

- Every generated asset needs a provenance sidecar before entering `assets/`.
- Required fields: asset id, prompt template, generator, source references, manual edits, reviewer, approval status, export paths, usage targets.
- Reject files with wrong canvas size, background leakage, inconsistent baseline, broken transparency, style drift, or unreadable silhouettes.
- Separate raw generations, selected candidates, cleaned assets, and engine-ready outputs.
- Add face/body/style drift scoring when the asset depends on identity consistency.

Codex task seed:

```text
Implement ai_asset_qc.py that checks image dimensions, transparency, naming, prompt sidecar presence, approved status, and target export paths. Produce a Markdown QC report and fail release packaging for unapproved AI assets.
```

### 6. Animation Frame Factory Pattern

Problem:

Frame production collapses when each idle, walk, hit, die, attack, and transition is manually sliced and inspected.

Pattern:

```text
source frames
-> normalization
-> baseline/pivot lock
-> strip/grid export
-> metadata sidecar
-> preview gif
-> import preset
-> validation scene
```

Rules:

- Use fixed cell sizes and consistent baseline for every frame in a clip.
- Store pivot, origin, pixels-per-unit, loop mode, FPS, and tags in metadata.
- Generate preview GIFs or MP4s automatically for review.
- Separate loop clips from bridge clips: start, stop, turn, land, recover, enter attack, exit attack.
- A character is production-ready only when the bridge matrix lists all required transitions.

Codex task seed:

```text
Create animation_frame_factory that normalizes frame strips/grids, verifies constant cell size and baseline markers, writes animation_metadata.json, generates preview GIFs, and flags missing bridge clips from animation_bridge_matrix.yaml.
```

### 7. Survivors-Like Interaction Graph Pattern

Problem:

Survivors-like games do not scale by adding isolated weapons. They scale through interactions among weapons, upgrades, enemy tags, stage pressure, and evolution rules.

Pattern:

```text
base weapon
-> tags
-> modifiers
-> enemy counters
-> evolution recipe
-> spawn pressure
-> post-run balance report
```

Rules:

- Use tags as the shared language between weapons, upgrades, enemies, and evolutions.
- Track interaction count, not just item count.
- Spawn director must budget threat by density, durability, speed, damage, crowd-control resistance, and counterplay.
- Upgrade cards need rarity, exclusions, prerequisites, max stacks, tags, and evolution recipe links.
- Post-run reports should include selected upgrades, skipped upgrades, DPS source, death cause, survival time, enemy budget over time, and object-pool pressure.

Codex task seed:

```text
Add WeaponCard, UpgradeCard, EnemyTag, EvolutionRecipe, SpawnCard, and RunBalanceReport schemas. Generate an interaction graph that counts weapon-upgrade-enemy links and flags isolated content with low systemic value.
```

### 8. Survival/Crafting Biome Contract Pattern

Problem:

Survival/crafting content becomes grind when biomes only provide new resources and higher recipe prices.

Pattern:

```text
biome identity
-> unique resource
-> hazard
-> traversal/combat mechanic
-> station recipe
-> capability unlock
-> next-risk gate
```

Rules:

- Every biome must contribute one exclusive resource, one hazard, one route decision, one crafting dependency, and one permanent capability unlock.
- Resources need source, sink, scarcity phase, route risk, and failure consequence.
- Stations should unlock new verbs, not only faster conversion numbers.
- Build resource ledgers before expanding the item list.
- Test overproduction, starvation, inventory overflow, station idle time, and unlock timing.

Codex task seed:

```text
Create biome_contract.yaml and resource_ledger telemetry. Validate that each biome has exclusive resource, hazard, route risk, station dependency, unlock, source/sink balance, and next-risk gate before it is marked content-complete.
```

### 9. Steam Release Artifact Pattern

Problem:

Steam release work fails late when store art, screenshots, demo build, review branch, depots, and live patching are treated as one final checklist.

Pattern:

```text
master key art
-> capsule/library/event variants
-> screenshot/trailer capture plan
-> store review package
-> build review package
-> demo branch
-> release candidate
-> default branch
-> hotfix lane
```

Rules:

- Treat Steam graphical assets as generated artifacts from master key art.
- Store review, build review, demo readiness, and live release are separate gates.
- Artwork Overrides are for time-limited updates/events; do not overwrite permanent capsule identity for temporary messaging.
- Every branch promotion needs depot id, changelog, save schema status, rollback target, review status, and known issues.
- Screenshots and trailer footage should be captured from release-candidate builds, not mockups.

Codex task seed:

```text
Create steam_release_artifacts.yaml and a validator that checks required capsule/library/event assets, screenshot capture build id, trailer build id, depot ids, branch target, demo status, save schema, review status, rollback branch, and patch risk.
```

### 10. Repository as Production Memory Pattern

Problem:

AI-assisted game production repeats the same mistakes unless fixes, templates, and validation rules are promoted into durable project memory.

Pattern:

```text
production issue
-> reference scan
-> pattern note
-> implementation card
-> validation proof
-> reusable template
-> core-library promotion
```

Rules:

- Archive entries should produce implementation cards, not just summaries.
- Split reusable knowledge into templates, workflows, debug patterns, and validation rules.
- Every fixed integration problem should become a Debug Skill-style entry with symptom, root cause, verified fix, and regression test.
- Every reusable scaffold should become a Template Skill-style entry with folder layout, data schema, and minimal runnable scene.
- Promote only patterns proven in a vertical slice into `docs/core-library/`.

Codex task seed:

```text
Add docs/core-library/template-skill-and-debug-skill-schema.md defining Template Skill and Debug Skill records for game production. Include fields for source pattern, implementation files, validation proof, regression test, and reuse status.
```

## Immediate implementation order

```text
1. asset_pipeline_spine
2. AnimationEventRegistry
3. CombatBoxAuthoringData + scrub preview
4. ImpactProfile + FeedbackPriorityRouter
5. ai_asset_qc.py + provenance sidecars
6. animation_frame_factory + bridge matrix
7. survivors interaction graph schemas
8. biome_contract.yaml + resource ledger
9. steam_release_artifacts.yaml
10. Template Skill / Debug Skill archive schema
```

## Practical checklist

```text
[ ] Every recurring asset export has a script or CLI path.
[ ] Runtime animation events are registry-backed and traceable.
[ ] Hitboxes/hurtboxes are authored as data and previewed by frame.
[ ] Combat feedback is routed through ImpactProfile tiers.
[ ] AI assets require provenance, reviewer, QC status, and export targets.
[ ] Animation clips have fixed cells, baseline, pivot, metadata, and preview output.
[ ] Survivors-like systems use tags, evolutions, spawn budgets, and balance reports.
[ ] Survival biomes have resource, hazard, route risk, station dependency, and unlock contract.
[ ] Steam assets are generated from master key art and validated with release manifests.
[ ] Solved workflow problems become reusable template/debug archive entries.
```
