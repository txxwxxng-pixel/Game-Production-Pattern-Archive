# Steam Release Workflow Patterns for Solo Indie Production

Updated: 2026-06-08

## Source cluster

- Steamworks release process: https://partner.steamgames.com/doc/store/releasing
- Steamworks application / store setup: https://partner.steamgames.com/doc/store/application
- GitHub Actions / release artifact workflow reference: https://docs.github.com/en/actions
- GitHub Releases reference: https://docs.github.com/en/repositories/releasing-projects-on-github/about-releases

## Pattern 1 — Treat Steam release as a production pipeline, not a final upload

Steam release work should begin before the game is finished.

Pipeline:

```text
capsule/key art -> store copy -> screenshots -> trailer -> demo/build branch -> playtest branch -> store review -> build review -> release checklist -> launch visibility beats
```

Minimum folders:

```text
release/
  steam/
    store_copy.md
    capsule_requirements.md
    screenshot_plan.md
    trailer_script.md
    build_checklist.md
    release_checklist.md
    post_launch_patch_notes.md
```

Production note:

For a solo developer, store assets are not marketing decoration. They are part of production validation. If the game cannot generate five clear screenshots, the core fantasy is probably not readable yet.

---

## Pattern 2 — Build review and store review are separate gates

Use two QA tracks.

Store review checklist:

```text
- title, short description, long description
- tags/genre positioning
- capsule art and key art
- screenshots match actual gameplay
- trailer represents real gameplay
- mature content / disclosures if relevant
- supported languages/platforms
```

Build review checklist:

```text
- launches from Steam client
- no missing DLL/assets
- correct executable path
- controller/mouse/keyboard support as advertised
- save path works
- resolution/fullscreen works
- crash-free first 10 minutes
- no debug overlays by default
```

Pattern:

```text
Store page can be emotionally strong.
Build must be boringly reliable.
```

---

## Pattern 3 — Use branches/channels for release confidence

Suggested branch/channel model:

```text
main: current production state
release-candidate: frozen build candidate
steam-demo: public demo branch
playtest: limited external test branch
hotfix: launch-day patch branch
```

GitHub/Godot/Unity build artifact structure:

```text
builds/
  windows/
  linux/
  mac/
  checksums/
  changelog/
```

Build naming:

```text
GameName_win64_0.1.0-rc1_2026-06-08.zip
GameName_win64_demo_0.1.0_2026-06-08.zip
```

---

## Pattern 4 — Screenshot production is a design diagnostic

Steam screenshots should each communicate a different promise.

Recommended five-shot set:

```text
1. Core fantasy shot: what the player does most
2. Combat/pressure shot: danger and response
3. Progression shot: upgrades/crafting/unlocks
4. World/biome shot: atmosphere and exploration
5. Boss/event shot: escalation and spectacle
```

For small 2D games, avoid screenshots where:

```text
- player character is too small to read
- UI hides the important action
- all shots have the same color/mood
- VFX overwhelms enemy/player silhouette
- no shot explains the unique hook
```

---

## Pattern 5 — Demo scope should be smaller than vertical slice

A demo should not expose unfinished systems. It should prove one loop.

Demo scope example:

```text
10-20 minutes
1 biome
3 enemy families
1 boss/event
6-10 upgrades
1 progression unlock
1 clear ending screen
```

Do not include:

```text
- half-working crafting trees
- empty settings menus
- placeholder key art inside the build
- debug commands visible to player
- content that teaches mechanics not yet supported later
```

---

## Pattern 6 — Launch readiness matrix

Use a simple matrix before release.

```text
A. Technical
- launch pass
- save/load pass
- crash smoke test
- framerate pass
- input pass

B. Content
- first 15 minutes polished
- tutorial readable
- fail/retry loop smooth
- minimum content promise met

C. Store
- screenshots final
- trailer final
- capsules final
- tags checked
- short description tested

D. Ops
- patch notes template ready
- known issues file ready
- feedback channel ready
- hotfix branch ready
```

---

## Codex task seeds

```text
Create release/steam/build_checklist.md with Windows build smoke-test steps for a Godot/Unity 2D indie game.
```

```text
Create tools/build_manifest.py that scans an export folder, lists executables, file sizes, missing common runtime files, checksum hashes, and writes release/build_manifest.json.
```

```text
Create release/steam/screenshot_plan.md with five screenshot slots: core loop, combat pressure, progression, biome/world, boss/event. Include composition notes and filename conventions.
```
