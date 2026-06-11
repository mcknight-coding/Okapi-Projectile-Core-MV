# Okapi Projectile Core - CHANGELOG

_Public-facing release history for RPG Maker MV plugin packaging_

**Source note:** This changelog was organized from the uploaded Okapi_ProjectileCore JavaScript plugin header, help text, and internal version comments. It is written for customers and release packaging, so raw developer comments have been converted into readable release notes.

## Release Summary

| Version | Release Theme | Main Category |
| --- | --- | --- |
| 2.20 | Current Public Header Release | Added, Fixed |
| 2.18 | Orbit and Override Priority Fixes | Fixed |
| 2.17 | Path + Behavior Cleanup | Added, Changed, Deprecated |
| 2.16 | Bounce Reclassification | Changed |
| 2.15 | Chain Naming Cleanup | Changed |
| 2.14 | YEP Sequential Timing Fix | Fixed |
| 2.13 | Sequential Multi-Target Fix | Fixed |
| 2.12 | Passive Missing Image Safety | Changed |
| 2.11 | Missing Projectile Image Safety | Added |
| 2.10 | Projectile Overrides | Added |
| 2.06 | Weapon-Element Skill Base Layer Rule | Changed |
| 2.05 | Projectile Stages | Added |
| 2.04 | Path-Facing and Back-Hit Support | Added |
| 2.03 | Default Launch Motion Improvements | Changed |
| 2.02 | Attack Pose Repair | Fixed |
| 2.01 | Projectile Image Alias | Added, Removed |
| 2.00 | YEP Perform Action Pose Restore | Fixed |
| 1.99 | YEP Target-Action Collapse Fix | Fixed |

## How to Use This Changelog

- Use this DOCX as a polished source document for the public CHANGELOG.md included with the plugin zip.
- Keep the newest release at the top.
- Use customer-facing wording and avoid internal-only debugging language unless it explains a user-visible fix.
- Before release, make the plugin header version, internal version string, zip name, README, and changelog agree.
## Detailed Release History

### [2.20] - Current Public Header Release

#### Added

- Added stronger demo safety behavior for named projectile skills.
- Non-weapon-element skills/items with their own projectile note boxes own their movement path, hit point, timing, stages, and behavior tags.
#### Fixed

- Final launch behavior now follows the same skill-owned behavior rule, so raw weapon/equipment projectile tags should not turn named skills into straight shots at the last second.
- `<Projectile Path: arc>` now visibly arcs in demo/front-view and side-view battles.
- Path-facing uses horizontal flip plus rotation, so left-flying projectiles do not appear upside down from a 180-degree rotation.
#### Compatibility

- Raw weapon projectile tags still work for Attack / weapon-element skills.
- For named skills, use Projectile Override blocks when equipment should conditionally change projectile behavior.
### [2.18] - Orbit and Override Priority Fixes

#### Fixed

- Fixed Behavior: orbit runtime pathing.
- Fixed path/behavior override priority issues.
- Higher-priority Projectile Mode tags should not erase explicitly selected skill-owned behavior for named skills.
### [2.17] - Path + Behavior Cleanup

#### Added

- Projectile Path: straight, arc, mortar, rolling, bounce, homing, sine, zigzag, helix, wild.
- Projectile Behavior: normal, boomerang, chain, orbit.
#### Changed

- Added the cleaner Path + Behavior notetag system.
- Path controls travel shape; Behavior controls lifecycle behavior.
- Old Projectile Mode tags still work for compatibility.
#### Deprecated

- figure8 is no longer part of the public path list and safely falls back to straight.
#### Relevant Notetag / Code Example

```text
<Projectile Path: arc>
<Projectile Behavior: normal>
<Projectile Path: straight>
<Projectile Behavior: boomerang>
```

### [2.16] - Bounce Reclassification

#### Changed

- bounce now means bouncing-ball projectile motion.
- Target-to-target jumping behavior should use chain instead.
### [2.15] - Chain Naming Cleanup

#### Changed

- Renamed target-to-target projectile behavior to chain.
- This separates chain behavior from bounce projectile motion.
### [2.14] - YEP Sequential Timing Fix

#### Fixed

- Fixed Yanfly Battle Engine Core sequential timing behavior.
- `<Projectile Timing: sequential>` now routes Yanfly ACTION EFFECT into the sequential target queue instead of the simultaneous volley path.
#### Relevant Notetag / Code Example

```text
<Projectile Timing: sequential>
```

### [2.13] - Sequential Multi-Target Fix

#### Fixed

- Sequential multi-target behavior now fires the full target list one projectile group at a time.
- Improves cinematic one-target-at-a-time attacks.
### [2.12] - Passive Missing Image Safety

#### Changed

- Missing projectile image safety is passive by default.
- Prevents false filesystem-check negatives from blocking valid projectile visuals in RPG Maker MV/NW.js deployments.
#### Recommended Setting

- Keep Strict Missing Projectile Image Check set to false for demos and most public releases.
### [2.11] - Missing Projectile Image Safety

#### Added

- Added safety behavior for missing projectile images.
- The plugin can warn, use a fallback image, skip the visual, and resolve the action safely instead of crashing battle.
#### Relevant Notetag / Code Example

```text
Plugin parameter:
Missing Projectile Fallback
Warn Missing Projectile Images
Strict Missing Projectile Image Check
```

### [2.10] - Projectile Overrides

#### Added

- Added Projectile Override blocks.
- Overrides are conditional projectile rules that can be placed on Actors, Classes, Enemies, Weapons, Armors, States, Skills, or Items.
- Multiple conditions use AND logic.
#### Supported Conditions

- Skill Type, Element, Element ID, Damage Type, Hit Type, Skill ID, Weapon Type, Actor ID, Class ID, Enemy ID, State.
#### Relevant Notetag / Code Example

```text
<Projectile Override>
Element: Cold
Projectile: ice_shard
Path: arc
Arc: 96
Trail: true
</Projectile Override>
```

### [2.06] - Weapon-Element Skill Base Layer Rule

#### Changed

- Normal Attack / weapon-element (-1) skills no longer discard the skill note box.
- The skill note box acts as the base behavior layer for trails, glow, stages, timing, sounds, and special movement.
- Higher-priority actor/state/weapon/equipment sources may still override the projectile image and explicitly tagged properties.
### [2.05] - Projectile Stages

#### Added

- Added Projectile Stages.
- Stage 2 can transform, split, or change behavior mid-flight before final impact.
- Stage 2 can trigger by progress, time, or distance.
#### Relevant Notetag / Code Example

```text
<Projectile Stage 2 Trigger: progress 60%>
<Projectile Stage 2 Trigger: time 30>
<Projectile Stage 2 Trigger: distance 160>
<Projectile Stage 2 Damage: each>
<Projectile Stage 2 Damage: once>
<Projectile Stage 2 Targeting: original>
```

### [2.04] - Path-Facing and Back-Hit Support

#### Added

- Added mortar path-facing support.
- Added back-hit loop path behavior.
- Added support for 180-degree turn behavior on back-hit projectile paths.
#### Relevant Notetag / Code Example

```text
<Projectile Face Path: true>
<Projectile Spin With Path: true>
<Projectile Hit: backMiddle>
<Projectile Back Attack: true>
<Projectile Back Arc Height: 120>
<Projectile Back Overshoot: 72>
```

### [2.03] - Default Launch Motion Improvements

#### Changed

- Physical projectile skills use the actor's equipped weapon motion by default.
- Sword/axe/flail style weapons use swing motion.
- Bow/crossbow/gun style weapons use missile motion.
- Magical projectile skills use spell motion.
- Item projectiles use item motion.
### [2.02] - Attack Pose Repair

#### Fixed

- Repaired attack command / weapon attack pose behavior.
### [2.01] - Projectile Image Alias

#### Added

- Added `<Projectile Image: ...>` alias.
- A single value works as a base projectile image if `<Projectile: ...>` is omitted.
- Multiple values work as an image sequence.
#### Removed

- Removed unused trickShot path code.
#### Relevant Notetag / Code Example

```text
<Projectile Image: Fireball>
<Projectile Image: Fire1;Fire2;Fire3;Fire4>
<Projectile Animation FPS: 12>
```

### [2.00] - YEP Perform Action Pose Restore

#### Fixed

- Restored one default Yanfly perform-action pose while continuing to suppress duplicate per-target poses.
### [1.99] - YEP Target-Action Collapse Fix

#### Fixed

- Yanfly Battle Engine Core default target actions now collapse into one Projectile Core volley for projectile skills.
- Prevents weapon strikes or damage from continuing on remaining targets after the first projectile.
- PERFORM ACTION is limited to one launch pose for projectile skills.
### [1.98] - YEP Sequencing Fix

#### Fixed

- Suppressed early ACTION ANIMATION.
- Preserved original Repeat count.
### [1.97] - Mortar Name Standardized

#### Changed

- mortar became the official name for true verticalLaunch behavior.
- verticalLaunch remains as a backward-compatible alias.
### [1.96] - TrickShot Deprecated

#### Deprecated

- Deprecated trickShot and randomPath as separate movement modes.
- Existing aliases now behave as zigzag.
### [1.84] - Mode Partial Override

#### Added

- Added partial movement mode overrides.
- A battler source can override movement mode without replacing the projectile image.
#### Relevant Notetag / Code Example

```text
Skill note:
<Projectile: Arrow>
Weapon/state/actor/class note:
<Projectile Mode: arc>
```

### [1.83] - Compatibility Second Pass

#### Fixed

- BattleManager.isBusy now waits while Projectile Core projectiles are active.
- Prevents Yanfly action sequences from continuing before projectile impact.
- Projectile impact now performs collapse checks after delayed damage.
- Added guards against duplicate projectile launches during action sequence ACTION EFFECT.
- If no projectile layer exists, impact resolves immediately instead of silently doing no damage.
### [1.80] - Advanced Visual Extras Completed

#### Added

- Completed orbit behavior, afterimages/trails, glow, and optional orbiting sub-projectiles.
### [1.51] - Advanced Movement Modes

#### Added

- Added advanced movement modes: sine, mortar, lob/vertical, homing, zigzag, spiral, helix, orbit, wild, and chain.
#### Changed

- figure8 safely falls back to straight.
- Older bounce/ricochet aliases normalize toward chain behavior in compatibility notes.
### [1.50] - Animated Projectiles and Visual Effects

#### Added

- Added animated projectile support using horizontal sprite sheets.
- Added image sequence animation.
- Added projectile pool modes: random and cycle.
- Added afterimage, trail, glow, and orbiters.
#### Relevant Notetag / Code Example

```text
<Projectile: FireballSheet>
<Projectile Frames: 8>
<Projectile Animation FPS: 12>
<Projectile Images: Fire1;Fire2;Fire3;Fire4>
<Projectile Animation FPS: 12>
<Projectile Pool: Fire1;Fire2;Fire3>
<Projectile Pool Mode: random>
```

### [1.39] - Scaling and Normal Attack Animation Fixes

#### Fixed

- Restored projectile scaling.
- Safely resolved Normal Attack (-1) animations so repeated hit animations do not crash RPG Maker MV.
### [1.37] - Replay Motion and Damage Animation Options

#### Added

- Added options to replay the user attack/throw motion on each projectile launch.
- Added options to replay damage animations for every projectile impact.
### [1.35] - Repeat Wave Target Recheck

#### Fixed

- User remains in battle/ready pose while projectile waves are pending.
- Later waves recheck living targets and reassign a projectile slot if an original target dies.
### [1.34] - Repeat Wave Duplicate Target Preservation

#### Fixed

- Preserved duplicate random target rolls while using repeated hits as delayed projectile waves.
## Packaging Checklist

- Confirm the plugin filename is Okapi_ProjectileCore.js.
- Confirm @plugindesc version matches the intended release version.
- Confirm Okapi.ProjectileCore.version matches the intended release version.
- Update README.md, QUICK_START.md, NOTETAG_GUIDE.md, and CHANGELOG.md together.
- Package sample projectile images inside img/projectiles/ if the demo expects them.
- Test the demo in both editor playtest and deployed build before upload.
```text
Recommended release package files:
Okapi_ProjectileCore.js
README.md
QUICK_START.md
NOTETAG_GUIDE.md
CHANGELOG.md
LICENSE.txt
Demo_Project/
Demo_Windows.zip
Demo_Browser/
img/projectiles sample images
```
