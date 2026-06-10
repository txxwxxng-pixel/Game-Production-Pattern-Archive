# Reference Refresh: Indie Production Ops Patterns

Date: 2026-06-10
Risk label: SAFE_PATTERN
Production labels: RUNTIME_ANIMATION, COMBAT_SYSTEM, HITBOX_HURTBOX, VFX_GAMEFEEL, AI_ASSET_PIPELINE, SURVIVAL_CRAFTING, SURVIVORS_LIKE, STEAM_RELEASE, QA_TELEMETRY

## Purpose

This entry converts a fresh practical-source scan into implementation-ready production notes for a Steam-style solo/small-team game. It does not preserve commercial source code, extracted assets, or copyrighted game content. It extracts durable workflow patterns only.

## Source cluster

- Godot AnimationPlayer / animation callback workflows
  - https://docs.godotengine.org/
  - Production read: animation timelines can be used as authoring surfaces for timed gameplay hooks, but combat authority should remain in data-driven runtime systems.
- Unity Animation Events
  - https://docs.unity3d.com/Manual/AnimationEvents.html
  - Production read: animation clips can emit timed callbacks with parameters; this is useful for hit windows, SFX, VFX, and footstep events, but needs validation so event names do not become invisible string debt.
- Steamworks build/depot/branch and release documentation
  - https://partner.steamgames.com/doc/sdk/uploading
  - https://partner.steamgames.com/doc/store/review_process
  - https://partner.steamgames.com/doc/store/application/branches
  - Production read: release is a promotion pipeline: local build -> internal branch -> beta/release-candidate branch -> default branch, with store review and build review treated as separate gates.
- Steam generative-AI disclosure and market trust discussion
  - https://partner.steamgames.com/doc/gettingstarted/contentsurvey
  - Production read: AI asset usage should be tracked as an internal compliance field even when assets are only used for drafts, placeholders, or ideation.
- Indie pre-release experimentation research
  - https://arxiv.org/abs/2411.17183
  - Production read: small teams usually lack large quantitative datasets before launch, so playtest notes, bug reports, qualitative friction logs, and demo conversion signals should be structured from the beginning.
- Godot relevance in indie development research
  - https://arxiv.org/abs/2401.01909
  - Production read: engine choice should optimize iteration speed, source control friendliness, export reliability, and authoring-tool simplicity rather than maximum AAA feature coverage.

## Extracted production patterns

### 1. Animation Event Contract Pattern

Problem:

Animation events are fast to author but can become hidden runtime dependencies.

Pattern:

```text
Animation clip/tag
-> emits named event key
-> runtime event adapter validates key
-> gameplay system resolves event into data asset action
-> debug overlay records event time, source clip, and target system
```

Rules:

- Animation decides `when`.
- Attack data decides `what`.
- Feedback profile decides `how it feels`.
- Event keys must be declared in a registry, not scattered as raw strings.
- Runtime should tolerate missing cosmetic events but fail loudly for missing gameplay-critical events.

Useful schema:

```yaml
animation_event:
  key: atk_light_start
  clip: keeper_slash_01
  frame: 7
  system: combat
  required: true
  payload:
    attack_id: keeper_light_01
    window: startup_to_active
```

Codex task seed:

```text
Create an AnimationEventRegistry that stores allowed animation event keys, required/optional status, owning system, and payload schema. Add a validator that scans imported animation metadata and reports missing, duplicate, unknown, or mistimed gameplay events.
```

### 2. Hitbox/Hurtbox Authoring Asset Pattern

Problem:

Hand-coded collision boxes slow combat iteration and make balance changes risky.

Pattern:

```text
AttackDefinition
-> frame windows
-> hitbox shapes
-> hurtbox override policy
-> reaction profile
-> impact profile
-> debug replay
```

Rules:

- Hitboxes are authored assets, not temporary runtime guesses.
- Hurtboxes describe vulnerability; hitboxes describe threat; sensors describe detection; interaction boxes describe non-damage triggers.
- Every active window needs a visual debug draw and a replayable log entry.
- Each attack instance needs a per-window hit cache to prevent multi-hit bugs.

Useful schema:

```yaml
attack_definition:
  id: keeper_light_01
  startup_frames: 6
  active_frames: [7, 8, 9]
  recovery_frames: 14
  hitboxes:
    - shape: rect
      bone: weapon_tip
      offset: [18, -6]
      size: [42, 18]
  reaction_profile: small_stagger
  impact_profile: blade_light
  can_hit_same_target_once_per_window: true
```

Codex task seed:

```text
Build a combat-box preview scene that loads AttackDefinition files and scrubs frame windows with hitbox, hurtbox, sensor, and origin overlays. Export validation errors as JSON for CI.
```

### 3. VFX/Game-Feel Priority Router Pattern

Problem:

Indie games often add shake, particles, flashes, and hitstop independently until combat becomes noisy.

Pattern:

```text
GameplayEvent
-> ImpactProfile
-> feedback priority router
-> hitstop / shake / flash / SFX / particles / camera impulse
-> readability budget check
```

Rules:

- Feedback is a stack, but readability is a budget.
- Major impact effects may suppress minor ambient effects for a short window.
- Hitstop and camera shake need tiers, not ad-hoc values.
- Every effect must declare whether it is gameplay-readable, cosmetic, UI, or ambient.

Useful schema:

```yaml
impact_profile:
  id: blade_light
  priority: 30
  hitstop_ms: 35
  camera_shake: tiny_horizontal
  flash: enemy_white_1f
  particle: slash_sparks_small
  sfx: blade_hit_light
  suppress_lower_priority_ms: 80
```

Codex task seed:

```text
Implement a FeedbackRouter that receives ImpactProfile events, applies priority suppression, pools VFX instances, and writes a combat feedback timeline to the debug HUD.
```

### 4. AI Asset Lineage and Steam Trust Pattern

Problem:

AI-assisted asset production can accelerate ideation but creates style drift, rights-tracking, and player-trust risk if not documented.

Pattern:

```text
prompt/source/reference
-> generation batch
-> curation pass
-> human edit / paintover / redraw / reject
-> lineage metadata
-> Steam disclosure/compliance review
-> final asset approval
```

Rules:

- AI output should never enter final build without lineage metadata.
- Store capsules and key art need stricter review than internal placeholders.
- Keep separate confidence labels: `placeholder`, `edited`, `paintover`, `human-final`, `rejected`.
- Track model/tool, prompt family, reference permission, edit author, and final approval status.
- Avoid visual inconsistency by maintaining a reference bible and per-character face/body locks.

Useful schema:

```yaml
asset_lineage:
  asset_id: enemy_leech_walk
  source_type: ai_assisted
  usage_stage: edited
  tool_family: image_generation
  prompt_id: drowned_coast_enemy_v3
  human_edit_required: true
  final_approval: pending
  steam_disclosure_review: required
```

Codex task seed:

```text
Add asset_lineage.yaml sidecars for generated images and make the asset import script fail when final-build assets lack source_type, approval_status, and steam_disclosure_review fields.
```

### 5. Survivors-Like Director + Synergy Ledger Pattern

Problem:

Survivors-like games scale poorly when every new weapon/enemy is bespoke.

Pattern:

```text
RunDirector
-> phase clock
-> threat budget
-> spawn cards
-> elite modifiers
-> player build tags
-> upgrade pool weighting
-> synergy ledger
-> post-run analysis
```

Rules:

- Difficulty should scale through budgets and modifiers before adding raw content volume.
- Upgrades should declare tags, prerequisites, exclusions, and evolution routes.
- Spawn cards should declare biome, phase, cost, max concurrency, and counterplay expectation.
- Failed-run analysis should report whether the cause was damage, mobility, sustain, unclear telegraph, or economy starvation.

Useful schema:

```yaml
upgrade:
  id: tide_nova_extra_ray
  tags: [nova, aoe, light]
  rarity: uncommon
  requires: [tide_nova]
  excludes: []
  synergy_tags: [salt, beacon]
  max_stacks: 3
```

Codex task seed:

```text
Create a SynergyLedger that records selected upgrade tags, rejected upgrade offers, evolved weapons, and death context. Export one JSON file per run for balance review.
```

### 6. Survival/Crafting Pressure Loop Pattern

Problem:

Survival and crafting loops become grindy when resources only serve as prices.

Pattern:

```text
world pressure
-> resource trip
-> inventory decision
-> station conversion
-> capability unlock
-> new zone risk
-> recovery/base improvement
```

Rules:

- Resources should create route, risk, timing, and opportunity-cost decisions.
- Each station should unlock a capability, not only a bigger number.
- A good resource has at least one sink, one route risk, one crafting use, and one scarcity phase.
- Pressure/recovery cycles should be visible in telemetry.

Useful schema:

```yaml
resource:
  id: driftwood
  tier: 1
  sources: [shoreline_debris, wreck_crates]
  sinks: [repair, torch, raft_part]
  risk_profile: low_exposure
  scarcity_phase: early_night
```

Codex task seed:

```text
Add a resource ledger that records sources, sinks, inventory overflow, craft failures, and station unlock timing. Generate a balance report after each test session.
```

### 7. Steam Branch Promotion and Demo Conversion Pattern

Problem:

Teams often treat Steam release as a final upload, causing last-minute review, depot, save, and demo problems.

Pattern:

```text
local/dev
-> internal branch
-> qa branch
-> demo branch
-> release-candidate branch
-> default branch
-> hotfix branch
```

Rules:

- Store review and build review are separate gates.
- Demo should be a stable product slice, not a random old build.
- Every branch promotion needs a build manifest, save migration status, known issues, and rollback plan.
- Release candidate freeze should allow only crash, blocker, legal/compliance, and platform-review fixes.
- Patch risk levels should be declared before upload.

Useful schema:

```yaml
steam_build_manifest:
  build_id: 2026-06-10-rc1
  branch: release_candidate
  depots: [windows_content, windows_exe]
  save_schema: 4
  migration_tested_from: [2, 3]
  known_issues: []
  rollback_branch: last_good
  patch_risk: medium
```

Codex task seed:

```text
Create a release_manifest.md generator that reads build metadata, save schema, depot list, branch target, known issues, and patch risk. Block promotion when required release fields are missing.
```

## Implementation order for a solo/AI-assisted project

```text
1. Add registries before adding more content.
2. Add validation before adding more animation events.
3. Add debug overlays before tuning combat feel.
4. Add lineage sidecars before scaling AI asset production.
5. Add run/resource ledgers before balancing survivors or survival loops.
6. Add Steam branch manifests before announcing a release date.
```

## Archive promotion candidates

Promote these to `docs/core-library/` if they survive one real vertical slice:

- AnimationEventRegistry
- CombatBoxPreviewScene
- FeedbackRouter
- AssetLineageSidecar
- SynergyLedger
- ResourceLedger
- SteamBuildManifest

## Practical checklist

```text
[ ] All gameplay animation events are registered.
[ ] All combat boxes can be previewed without entering a full level.
[ ] All hit impacts resolve through ImpactProfile.
[ ] All generated assets have lineage sidecars.
[ ] All upgrades and spawn cards have tags and costs.
[ ] All resources have sources, sinks, risk, and scarcity phase.
[ ] Every Steam branch promotion has a manifest and rollback target.
```
