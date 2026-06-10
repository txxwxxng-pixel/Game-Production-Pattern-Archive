# Reference Refresh 02: Steam Indie Workflow Patterns

Date: 2026-06-10
Risk label: SAFE_PATTERN
Production labels: RUNTIME_ANIMATION, COMBAT_SYSTEM, HITBOX_HURTBOX, VFX_GAMEFEEL, AI_ASSET_PIPELINE, SPRITE_FRAME_PRODUCTION, SURVIVAL_CRAFTING, SURVIVORS_LIKE, STEAM_RELEASE, SOURCE_QA

## Purpose

This entry converts a fresh practical-source scan into concise production-pattern notes for Steam-style indie games. It stores workflow patterns only. It does not preserve commercial game source code, extracted assets, leaked code, or copyrighted assets.

## Fresh source cluster

- Godot Animation track types
  - https://docs.godotengine.org/en/stable/tutorials/animation/animation_track_types.html
  - Production read: animation tracks can call methods and change properties over time, making the timeline useful for timing hooks. Treat the timeline as an authoring surface, then route gameplay effects through validated data assets.
- Unity Animation Events
  - https://docs.unity3d.com/Manual/script-AnimationWindowEvent.html
  - Production read: animation events support clip-timed callbacks. They are useful for SFX, VFX, footsteps, and attack windows, but event names and payloads need registry validation.
- Game feel and impact-feel research
  - https://arxiv.org/abs/2011.09201
  - https://arxiv.org/abs/2208.06155
  - Production read: game feel can be treated as tuning, juicing, and streamlining. Impact feel depends heavily on coherent hitstop, sound, and camera control.
- 2D/3D hybrid art-production reference
  - https://www.creativebloq.com/3d/video-game-design/how-at-fates-end-uses-3d-camera-tricks-to-make-its-2d-art-feel-alive
  - Production read: reproducible art style, modular reuse, short bridge animations, and 3D camera depth can reduce 2D production burden while keeping a hand-drawn feel.
- Godot indie relevance and platform expansion signals
  - https://arxiv.org/abs/2401.01909
  - https://www.pcgamer.com/hardware/godot-devs-are-getting-a-helping-hand-from-microsoft-to-bring-their-games-to-xbox-on-pc/
  - Production read: Godot remains relevant for small-team iteration speed, source-control-friendly workflows, and expanding platform workflows, but platform integration should be treated as a separate release-risk lane.
- Open-source AI-contribution quality risk
  - https://www.pcgamer.com/software/platforms/open-source-game-engine-godot-is-drowning-in-ai-slop-code-contributions-i-dont-know-how-long-we-can-keep-it-up/
  - Production read: AI-assisted code and asset generation needs review gates, test evidence, and provenance. Volume without verification becomes maintenance debt.
- Steamworks review and graphical asset documentation
  - https://partner.steamgames.com/doc/store/review_process
  - https://partner.steamgames.com/doc/store/assets
  - Production read: Steam release readiness includes store review, build review, capsule/screenshot asset readiness, depot/branch discipline, and disclosure/compliance checks.

## Extracted production patterns

### 1. Timeline Hook Adapter Pattern

Problem:

Animation timelines are the easiest place to author timing, but the worst place to hide gameplay authority.

Pattern:

```text
Animation clip timeline
-> method/event key
-> TimelineHookAdapter
-> registry validation
-> combat / audio / VFX / camera system
-> debug event trace
```

Rules:

- Animation decides `when`, not final gameplay meaning.
- AttackDefinition, ImpactProfile, and AudioCue data decide `what` happens.
- Every gameplay-critical hook must exist in a registry.
- Cosmetic hooks may fail soft; gameplay hooks must fail loudly in validation.
- Store frame index, normalized time, source clip, and resolved payload in debug logs.

Codex task seed:

```text
Create a TimelineHookAdapter for Godot/Unity-style animation callbacks. It should map event keys to registered runtime actions, reject unknown gameplay-critical keys, and export a per-clip event trace JSON for CI validation.
```

### 2. Combat Box Scrub Tool Pattern

Problem:

Hitbox and hurtbox tuning becomes guesswork when boxes are only visible during live combat.

Pattern:

```text
AttackDefinition
-> frame-window data
-> box shape data
-> preview scene scrubber
-> overlap simulator
-> debug screenshot/export
```

Rules:

- Author hitboxes, hurtboxes, sensors, and interaction volumes as separate taxonomies.
- Preview startup, active, recovery, invulnerability, and cancel windows on a frame scrubber.
- Each attack needs one-hit-per-target or multi-hit rules declared explicitly.
- Export box overlays as review images before combat integration.
- Add CI checks for missing boxes, zero-size boxes, invalid frame windows, and unreachable cancel windows.

Codex task seed:

```text
Build a CombatBoxScrubber that loads attack YAML/JSON, displays sprite frame + origin + hitbox/hurtbox overlays, simulates target overlap, and exports validation errors plus PNG review frames.
```

### 3. Impact Feel Stack Pattern

Problem:

VFX, hitstop, shake, flashes, and SFX often get tuned separately, causing noisy or weak combat.

Pattern:

```text
HitConfirmed
-> ImpactProfile
-> priority router
-> hitstop / camera / sound / flash / particles
-> suppression window
-> readability telemetry
```

Rules:

- Create tiers: tiny, light, medium, heavy, boss, fatal.
- Bind sound, hitstop, and camera movement in one profile so they remain coherent.
- Major impacts may suppress ambient/low-priority effects for a short window.
- VFX lifetime and pooling rules belong in the profile.
- Track player-facing readability failures: too much flash, invisible enemy, late sound, confusing shake.

Codex task seed:

```text
Implement ImpactProfile assets and a FeedbackPriorityRouter. Add runtime telemetry that records hit tier, hitstop ms, camera impulse, SFX id, VFX id, and whether lower-priority effects were suppressed.
```

### 4. Reproducible 2D Art + Bridge Animation Pattern

Problem:

High-quality 2D art collapses under production load when every frame and background is bespoke.

Pattern:

```text
style bible
-> modular reusable props/background planes
-> loop animations
-> bridge animations
-> engine placement pass
-> consistency review
```

Rules:

- The art style must be reproducible, not just impressive in one key image.
- Build movement from reusable loops plus short transition bridges: start, stop, turn, land, recover.
- Use modular background planes, parallax, and optional 3D camera depth to reduce flat staging.
- Teach artists or asset operators enough engine workflow to place and inspect assets without breaking scenes.
- Maintain pivot, baseline, scale, and lighting rules in the source asset bible.

Codex task seed:

```text
Create an AnimationBridgeMatrix document and validator: for each character state, list required loop clips and bridge clips, then flag missing transitions before a character is marked production-ready.
```

### 5. AI Asset / Code Provenance Gate Pattern

Problem:

AI-generated batches can increase output volume while lowering trust, consistency, and maintainability.

Pattern:

```text
AI draft or code suggestion
-> provenance sidecar
-> human review
-> test/proof artifact
-> integration gate
-> release disclosure/compliance review
```

Rules:

- Generated assets need lineage sidecars before entering the build.
- Generated code needs test evidence before merging.
- Separate confidence labels: draft, placeholder, edited, reviewed, final, rejected.
- Track tool family, prompt/source, human reviewer, test status, and release usage.
- Never let unreviewed AI output become invisible infrastructure.

Codex task seed:

```text
Add provenance_required checks for generated assets and generated code. Block final-build packaging when any file marked ai_assisted lacks reviewer, approval_status, source_usage, and test_or_review_artifact.
```

### 6. Survivors-Like Content Multiplication Pattern

Problem:

Survivors-like scope explodes if every weapon, enemy, upgrade, and wave is bespoke.

Pattern:

```text
base weapon/enemy templates
-> tags
-> modifiers
-> spawn budget
-> upgrade weighting
-> synergy ledger
-> post-run balance report
```

Rules:

- Add content by multiplying templates with tags and modifiers before adding entirely new systems.
- Spawn waves should be budgeted by threat cost, density, speed, durability, and counterplay.
- Upgrades need prerequisites, exclusions, tags, rarity, max stacks, and evolution rules.
- Balance review should record selected upgrades, rejected upgrades, death cause, DPS source, and survival time.
- Performance budgets must include object pooling, max concurrent enemies, and VFX suppression.

Codex task seed:

```text
Create SpawnCard, UpgradeCard, and SynergyLedger schemas. Add a run-end balance export summarizing upgrade path, enemy budget over time, object-pool pressure, and death context.
```

### 7. Survival/Crafting Resource Pressure Pattern

Problem:

Survival/crafting systems become grind when resources are only prices in recipes.

Pattern:

```text
world pressure
-> route choice
-> resource acquisition
-> inventory constraint
-> station conversion
-> capability unlock
-> new risk zone
```

Rules:

- Every resource needs a source, sink, route risk, scarcity phase, and unlock purpose.
- Stations should unlock capabilities, not only bigger numbers.
- Inventory pressure should create decisions, not constant annoyance.
- Track resource starvation, overproduction, craft failures, station idle time, and unlock timing.
- Build a small economy simulation before scaling the item list.

Codex task seed:

```text
Implement ResourceLedger telemetry that records sources, sinks, inventory overflow, craft failures, station idle time, and unlock timing. Generate a resource-balance report after each test run.
```

### 8. Steam Release Asset and Review Gate Pattern

Problem:

Steam launches fail late when store assets, build review, branch promotion, and demo readiness are treated as one final upload.

Pattern:

```text
store capsule plan
-> screenshot/trailer capture plan
-> depot/build upload
-> internal branch
-> review branch
-> demo branch
-> release candidate
-> default branch
-> hotfix branch
```

Rules:

- Treat store review and build review as separate gates.
- Keep capsule/key art/screenshot requirements in a checklist next to the build manifest.
- Demo is its own product slice with its own branch, save boundary, analytics, and known-issues list.
- Release candidate freezes should allow only blocker, crash, compliance, save, or platform-review fixes.
- Each branch promotion needs rollback target, save schema status, depots, changelog, and patch risk.

Codex task seed:

```text
Create steam_release_manifest.yaml with store_asset_status, depot_ids, branch_target, demo_status, save_schema, review_status, known_issues, rollback_branch, and patch_risk. Fail promotion if required fields are missing.
```

## Immediate implementation order

```text
1. TimelineHookAdapter
2. CombatBoxScrubber
3. ImpactProfile + FeedbackPriorityRouter
4. AnimationBridgeMatrix
5. AI/code provenance gate
6. SpawnCard + UpgradeCard + SynergyLedger
7. ResourceLedger
8. SteamReleaseManifest
```

## Promotion candidates

Promote these to `docs/core-library/` after they survive one vertical slice:

- TimelineHookAdapter
- CombatBoxScrubber
- ImpactProfile
- AnimationBridgeMatrix
- AssetAndCodeProvenanceGate
- SpawnCard
- UpgradeCard
- SynergyLedger
- ResourceLedger
- SteamReleaseManifest

## Practical checklist

```text
[ ] Animation events are registry-backed and traceable.
[ ] Combat boxes can be scrubbed without running a full level.
[ ] Hit feel is driven by ImpactProfile, not scattered constants.
[ ] Character animation has loop and bridge requirements listed.
[ ] AI-generated assets/code require provenance and review proof.
[ ] Survivors-like content uses templates, tags, budgets, and ledgers.
[ ] Survival resources have source, sink, risk, scarcity, and unlock purpose.
[ ] Steam store assets, build review, demo branch, and release candidate are separate gates.
```
