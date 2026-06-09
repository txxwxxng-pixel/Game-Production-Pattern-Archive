# SteamPipe Release Packaging and Branch Patterns

Date: 2026-06-10

## Purpose

Convert Steamworks release documentation into a solo-friendly production checklist for Steam-style indie games.

This note focuses on practical build/release operations, not marketing copy.

## Core principle

```text
Steam release is not one upload.
It is a branch promotion pipeline.
```

## Store/build contract

Steam review treats the store page and submitted build as connected promises.

Production interpretation:

```text
If the store page says it exists, the build must prove it.
If a screenshot shows it, the build must contain it.
If a platform is listed, the executable must launch there.
```

## Recommended branch ladder

```text
local-dev
  -> internal-test
  -> private-beta
  -> demo-review
  -> release-candidate
  -> default-live
  -> hotfix
```

### local-dev

Purpose:

- Fast local iteration.
- No Steam upload required.
- Uses local save paths and debug flags.

Gate:

```text
Starts locally
No missing content errors
No critical validation blockers
```

### internal-test

Purpose:

- First Steam-launched build.
- Used for controller, overlay, platform paths, and install testing.

Gate:

```text
Starts through Steam client
Correct executable path
Save path works
Input works
Crash logs are writable
```

### private-beta

Purpose:

- External testers.
- Password-protected branch.
- Used for feedback before public demo or release.

Gate:

```text
No debug cheats exposed unless intended
Telemetry/logging active
Known issues documented
Rollback build exists
```

### demo-review

Purpose:

- Separate demo review and demo festival readiness.
- Demo must not accidentally expose full game content.

Gate:

```text
Demo AppID or demo package configured
Demo content boundary verified
Save compatibility policy declared
Wishlist CTA present if appropriate
```

### release-candidate

Purpose:

- Final release rehearsal.
- No new features.
- Only blocker fixes allowed.

Gate:

```text
Store promises match build
All supported OS launch
No placeholder screenshots or concept-only store images
Save migration tested
Patch size checked
```

### default-live

Purpose:

- Public live branch.
- Only promoted from release-candidate.

Gate:

```text
Release notes written
Rollback target known
Build manifest archived
Emergency hotfix path ready
```

### hotfix

Purpose:

- Critical post-release fix path.
- Minimal change surface.

Gate:

```text
Fix scope documented
Regression test focused
Patch size checked
Old save tested
```

## SteamPipe-aware packaging

SteamPipe builds updates from changed file chunks. Large monolithic archives and asset order shuffling can create oversized updates.

Practical packaging rules:

```text
Prefer smaller feature/biome packs.
Keep changed assets localized.
Avoid unnecessary asset reordering.
Avoid giant all-content pack files.
Avoid build timestamps inside packed assets when possible.
Prefer adding a new pack for a major content drop.
```

## Suggested content layout

```text
content/
  windows/
    game.exe
    steam_api64.dll
  linux/
    game.x86_64
  shared/
    config/
    localization/
  packs/
    core_ui.pak
    player_common.pak
    enemies_common.pak
    biome_shallows.pak
    biome_depths.pak
    biome_ruins.pak
    audio_music.pak
    audio_sfx.pak
  demo/
    demo_gate_config.json
```

## Build manifest template

```text
build_id:
branch:
commit_sha:
engine_version:
content_version:
save_schema_version:
platforms:
known_issues:
changed_packs:
expected_patch_size:
rollback_build:
review_status:
smoke_test_result:
```

## Release validation checklist

### Launch

```text
Windows launches through Steam
Linux launches through Steam if listed
Steam overlay works or known-disabled
Controller input works if listed
Game closes cleanly
```

### Store promise match

```text
Screenshots are gameplay
Trailer footage is representative
Feature list matches current build
Supported languages match current build
Supported OS list matches current build
```

### Save and config

```text
Fresh save works
Old save migration works
Corrupt save handling works
Settings persist
Cloud save policy tested if enabled
```

### Patch risk

```text
Changed packs are listed
Patch size is reasonable
No accidental full-content repack
Rollback build exists
```

### Demo boundary

```text
Demo cannot access full-game maps
Demo save cannot corrupt full-game save
Demo end screen works
Demo analytics/log markers separated
```

## Codex-ready task

```text
Create a docs/release/ directory with:

1. steampipe_content_manifest.md
2. branch_promotion_checklist.md
3. build_manifest_template.md
4. demo_boundary_checklist.md
5. hotfix_runbook.md

Add scripts or editor commands where possible to generate a build manifest containing commit SHA, engine version, content version, save schema version, changed asset packs, and smoke test status. Add validation warnings if a release candidate changes a large pack unexpectedly or if store-feature flags do not match enabled runtime features.
```

## Done criteria

```text
Every Steam upload has a manifest.
Every public build can be traced to a commit and content version.
Every release candidate has a rollback target.
Demo and full game content boundaries are explicit.
Patch size risk is reviewed before pushing live.
```
