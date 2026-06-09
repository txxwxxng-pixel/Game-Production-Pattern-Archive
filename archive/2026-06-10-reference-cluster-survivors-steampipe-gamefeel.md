# Reference Cluster — Survivors, SteamPipe, Game Feel

Date: 2026-06-10

Purpose: turn current public references into practical production notes for Steam-style solo/AI-assisted indie development.

## Sources reviewed

- Vampire Survivors genre/reference summary
  - https://en.wikipedia.org/wiki/Vampire_Survivors
- Vampire Survivors-like genre discussion and visibility/readability issues
  - https://en.wikipedia.org/wiki/Vampire_Survivors%E2%80%93like
- Deep Rock Galactic: Survivor reference summary
  - https://en.wikipedia.org/wiki/Deep_Rock_Galactic%3A_Survivor
- GamesRadar interview/report on survivors-like iteration
  - https://www.gamesradar.com/games/action/vampire-survivors-kicked-off-a-game-development-gold-rush-but-has-a-legitimately-new-genre-emerged-between-the-cash-ins/
- Steamworks Review Process
  - https://partner.steamgames.com/doc/store/review_process
- Steamworks Builds / Beta Branches
  - https://partner.steamgames.com/doc/store/application/builds
- Steamworks Uploading to Steam / SteamPipe
  - https://partner.steamgames.com/doc/sdk/uploading
- Impact feel research
  - https://arxiv.org/abs/2208.06155
- Pre-release indie experimentation research
  - https://arxiv.org/abs/2411.17183

## Extracted production patterns

### 1. Survivors-like differentiation pattern

Do not copy the surface loop only.

Base formula:

```text
simple movement
+ automatic attacks
+ dense enemy pressure
+ XP pickup greed
+ random upgrade draft
+ meta unlocks
```

Differentiation should be one strong systemic verb, not ten weak features.

Examples of useful differentiators:

```text
mining / digging
route carving
vertical movement
mission extraction
crafting between waves
base defense at night
biome hazard manipulation
```

Production rule:

```text
Keep the input simple.
Add depth through encounter geometry, upgrade synergies, extraction pressure, and resource temptation.
```

### 2. Readability-first horde pattern

Survivors-like games fail quickly when visual clutter hides the player, enemy intent, XP, projectiles, or pickup value.

Practical rule:

```text
Every VFX asset needs a readability budget.
Every enemy family needs silhouette separation.
Every pickup tier needs color/value hierarchy.
Every boss attack needs pre-impact warning language.
```

Implementation notes:

- Player silhouette must remain visible at max enemy count.
- Enemy hit flashes must not obscure enemy attack telegraphs.
- XP and resource pickups need priority rules when stacked with damage VFX.
- Projectile VFX should have smaller brightness priority than danger telegraphs.

### 3. Mission chunking pattern

Pure endless survival can become fatigue-heavy. A Steam-ready solo project can reduce fatigue by slicing a run into smaller objectives.

Pattern:

```text
Run
  Stage 1: collect / survive / mini objective
  Stage 2: pressure rises / elite appears
  Stage 3: resource temptation / route decision
  Stage 4: boss gate
  Stage 5: extraction / conversion / reward
```

Benefits:

- Easier content pacing.
- Easier trailer capture.
- Easier telemetry analysis.
- More natural demo boundaries.
- Better Steam page feature communication.

### 4. SteamPipe-aware packaging pattern

SteamPipe patching favors localized file changes. Large shuffled pack files can cause oversized updates.

Production rule:

```text
Package by feature, level, biome, or content family.
Avoid shuffling asset order inside large packs.
Avoid giant monolithic content archives.
Prefer adding new pack files for major updates.
```

Recommended depot/content split for a small Steam game:

```text
base_executable
base_shared_assets
biome_01_assets
biome_02_assets
music_audio
localization
optional_demo_content
```

### 5. Steam review readiness pattern

Steam review checks whether the store promises match the build. This creates a production rule:

```text
Store page is a contract.
Build must prove every visible promise.
```

Before submitting review:

- Supported OS list matches actual tested executables.
- Screenshots are real gameplay, not concept-only art.
- Listed features exist in the current build.
- Capsule/title assets are readable.
- Demo branch and full-game branch are separated.

### 6. Impact feel stack pattern

Impact feel should be authored as a stack, not hardcoded per attack.

```text
Attack connects
  -> hit confirmation event
  -> damage number / marker
  -> hitstop tier
  -> camera impulse
  -> target flash
  -> attacker recoil or pose hold
  -> sound layer
  -> particle burst
  -> controller rumble if supported
```

Key production note:

```text
Hitstop, sound coherence, and camera control are high-priority impact feel levers.
```

### 7. Pre-release experiment pattern

For small indie teams, early experimentation often depends on qualitative data more than large telemetry samples.

Practical pattern:

```text
Prototype one question at a time.
Record player confusion.
Log first-death cause.
Log first-upgrade choice.
Log time-to-fun.
Review playtest video before adding systems.
```

## Codex-ready task seeds

### Task A — Survivors readability stress scene

```text
Create a debug scene that spawns max-count enemies, XP gems, pickup items, player attacks, enemy attacks, and boss warning VFX at once. Add hotkeys to toggle each layer. Add a screenshot command and save readability notes to docs/qa/readability_stress.md.
```

### Task B — SteamPipe content manifest

```text
Create docs/release/steampipe_content_manifest.md. Group build outputs by executable, shared assets, biome packs, audio, localization, and demo-only content. Add rules preventing large monolithic pack files and asset-order shuffling.
```

### Task C — Impact profile asset

```text
Implement data-driven ImpactProfile assets with hitstop_frames, camera_shake_id, target_flash_id, sfx_id, vfx_id, priority, and cooldown fields. Wire combat hit events to request an ImpactProfile instead of directly playing effects.
```

## Archive decision

Promote these patterns into the core library if they recur in at least three implementation tasks:

- readability budget
- mission chunking
- SteamPipe-aware content packaging
- impact profile stack
- store page as contract
