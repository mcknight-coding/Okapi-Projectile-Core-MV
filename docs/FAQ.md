# FAQ

**Plugin:** Okapi Projectile Core  
**Current documented plugin version:** v2.20  
**Engine target:** RPG Maker MV  
**Author/Brand:** McKnight-Coding / Okapi Interactive

---

## General Questions

### What does Okapi Projectile Core do?

Okapi Projectile Core adds visual projectile travel to RPG Maker MV battles. Instead of damage happening immediately, the projectile launches, travels to the target, and then applies damage, animation, popups, and battle log results on impact.

### Does this require Yanfly?

No. Projectile Core is self-contained and does not require Yanfly, Action Sequences, SV motion plugins, or any other Okapi plugin.

### Does it work with Yanfly Battle Engine Core?

Yes, it includes compatibility handling for YEP_BattleEngineCore-style battle flow. Yanfly is optional, not required.

Recommended order when using Yanfly battle plugins:

```text
YEP_CoreEngine
YEP_BattleEngineCore
YEP_X_ActionSequencePack1 / Pack2 / Pack3, if used
Okapi_ProjectileCore
```

### Does it work in front-view battles?

Yes. Side-view throw motion is optional. If the project is front-view or no SV battler/motion is available, the plugin safely continues without the throw pose.

### Does it work in side-view battles?

Yes. Side-view is supported, and the plugin can try to play a launch/throw motion before firing the projectile.

### Does it work in RPG Maker MZ?

Not officially. This plugin is written for RPG Maker MV. MZ may require a port and separate testing.

---

## Installation Questions

### Where do I put the plugin file?

Put the plugin here:

```text
js/plugins/Okapi_ProjectileCore.js
```

Then enable it in RPG Maker MV's Plugin Manager.

### Where do projectile images go?

By default, put projectile images here:

```text
img/projectiles/
```

Examples:

```text
img/projectiles/Fireball.png
img/projectiles/Arrow.png
img/projectiles/Boomerang.png
img/projectiles/Boulder.png
```

### Do notetags include `.png`?

Use the image name without `.png` for cleanest results:

```text
<Projectile: Fireball>
```

The plugin can clean `.png` if included, but extensionless names are easier for users to read and maintain.

### Is an `assets/` folder required?

No. RPG Maker MV does not need a repository `assets/` folder for this plugin to run. The required in-game image folder is `img/projectiles/` by default.

A GitHub `assets/` folder is still useful for optional screenshots, GIFs, thumbnails, and marketing images.

---

## Basic Notetag Questions

### What is the simplest projectile notetag?

Add this to a Skill or Item note box:

```text
<Projectile: Fireball>
```

The plugin will use `img/projectiles/Fireball.png` by default.

### How do I make a straight projectile?

```text
<Projectile: Fireball>
<Projectile Path: straight>
<Projectile Speed: 14>
```

Legacy mode syntax also works:

```text
<Projectile Mode: straight>
```

### How do I make an arc projectile?

```text
<Projectile: Arrow>
<Projectile Path: arc>
<Projectile Arc: 72>
<Projectile Speed: 18>
```

### How do I make a mortar projectile?

```text
<Projectile: Boulder>
<Projectile Path: mortar>
<Projectile Launch Height: 160>
<Projectile Mortar Phase: 0.35>
<Projectile Face Path: true>
```

### How do I make a boomerang?

```text
<Projectile: Boomerang>
<Projectile Path: straight>
<Projectile Behavior: boomerang>
<Projectile Rotation: true>
<Projectile Rotation Speed: 0.35>
```

Old syntax also works:

```text
<Projectile Mode: boomerang>
```

### How do I make a chain projectile?

```text
<Projectile: Lightning>
<Projectile Path: straight>
<Projectile Behavior: chain>
<Projectile Chains: 3>
<Projectile Chain Range: 9999>
<Projectile Chain Target: nearest>
```

### How do I make a bouncing-ball projectile?

```text
<Projectile: Boulder>
<Projectile Path: bounce>
<Projectile Bounces: 3>
<Projectile Bounce Height: 56>
```

Important: in newer docs, `bounce` is a projectile path for bouncing movement. For target-to-target jumps, use `chain` behavior.

---

## Path and Behavior Questions

### What is the difference between Path and Behavior?

`Projectile Path` controls how the projectile travels through space.

Examples:

```text
straight
arc
mortar
homing
sine
zigzag
helix
wild
```

`Projectile Behavior` controls special action behavior.

Examples:

```text
normal
boomerang
chain
orbit
```

Recommended modern format:

```text
<Projectile Path: arc>
<Projectile Behavior: normal>
```

### Do old `<Projectile Mode: ...>` tags still work?

Yes. Old mode tags still work for compatibility. New docs should prefer `Path` and `Behavior` because they are clearer.

### Is `figure8` supported?

No. Existing `figure8` notes safely fall back to `straight`.

### What happened to `trickShot` and `randomPath`?

They now behave as `zigzag` for backward compatibility.

---

## Weapon and Skill Priority Questions

### Why is my weapon projectile overriding my skill projectile?

This usually happens when the skill uses RPG Maker MV damage element `-1`, also called Normal Attack / Weapon element.

For weapon-element skills, Projectile Core allows projectile data to come from the battler's states, weapon, armor, enemy, actor, class, and skill/item notes.

### What is the priority for weapon-element projectile notes?

Highest to lowest:

1. States on the subject
2. Actor primary weapon
3. Actor other weapons
4. Actor armors
5. Enemy database notes
6. Actor database notes
7. Actor class notes
8. Skill/item notes

### How do I stop a weapon from turning a named skill into a straight shot?

Use v2.20 or later and make sure the named skill is **not** using weapon element `-1` unless you intentionally want weapon-style behavior.

For non-weapon-element named skills, the skill owns its path, hit point, timing, stages, and behavior tags.

If equipment should change a named skill, use a conditional override block instead of raw weapon tags:

```text
<Projectile Override>
Skill ID: 91
Projectile: ice_shard
Path: arc
Arc: 96
</Projectile Override>
```

### Can weapons change only the movement mode but keep the skill image?

Yes. Projectile Core supports partial movement overrides from battler sources, but v2.20 protects non-weapon-element named skills from accidental raw equipment overrides. For deliberate named-skill changes, use `<Projectile Override>`.

---

## Image and Direction Questions

### My projectile image is facing the wrong way. What do I do?

If your art naturally faces right, use:

```text
<Projectile Image Facing: right>
<Projectile Auto Flip: true>
```

If your art naturally faces left, use:

```text
<Projectile Image Facing: left>
<Projectile Auto Flip: true>
```

### My projectile flies left and appears upside down. How do I fix it?

Use v2.20 or later. v2.20 path-facing uses horizontal flip plus rotation so left-flying projectiles should not need a 180-degree upside-down rotation workaround.

Recommended tags:

```text
<Projectile Face Path: true>
<Projectile Auto Flip: true>
```

### My projectile is too big or too small. How do I scale it?

Use:

```text
<Projectile Scale: 1.5>
```

Percent format works too:

```text
<Projectile Scale: 150%>
```

Battler sources can also multiply scale:

```text
<Projectile Scale Multiplier: 1.25>
<Projectile Scale Bonus: 25%>
```

---

## Missing Image Questions

### What happens if a projectile image is missing?

The plugin can warn in the console, use a fallback image if configured, or skip the projectile visual and safely resolve the action.

### What should `Strict Missing Projectile Image Check` be set to?

Recommended:

```text
Strict Missing Projectile Image Check: false
```

This is safest because RPG Maker MV/NW.js paths can differ between editor, playtest, and deployment.

### Should I set a Missing Projectile Fallback?

Optional, but recommended for commercial releases. A simple fallback like `DefaultProjectile.png` can prevent a missing file from looking broken.

Example Plugin Manager setting:

```text
Missing Projectile Fallback: DefaultProjectile
```

Then include:

```text
img/projectiles/DefaultProjectile.png
```

---

## Timing and Multi-hit Questions

### Can multi-target projectiles launch together?

Yes. The default timing is simultaneous.

```text
<Projectile Timing: simultaneous>
```

### Can targets be hit one after another?

Yes.

```text
<Projectile Timing: sequential>
```

### How do repeats work?

If the skill/item has Invocation Repeat greater than 1, each repeat becomes a delayed projectile wave instead of all hits landing instantly.

Optional tags:

```text
<Projectile Waves: 2>
<Projectile Wave Delay: 120>
<Projectile Wave Delay Seconds: 2>
```

### Are duplicate random target rolls preserved?

Yes. If a repeated random-target skill rolls the same enemy multiple times, that enemy can receive the correct number of strikes.

---

## Stage and Advanced Projectile Questions

### Can a projectile transform or split mid-flight?

Yes. Use Projectile Stage 2 tags.

Example:

```text
<Projectile: blast_green>
<Projectile Path: mortar>
<Projectile Stage 2 Trigger: progress 60%>
<Projectile Stage 2 Image: beam_green>
<Projectile Stage 2 Path: homing>
<Projectile Stage 2 Count: 3>
<Projectile Stage 2 Spread: 35>
<Projectile Stage 2 Speed: 18>
```

### What can trigger Stage 2?

```text
<Projectile Stage 2 Trigger: progress 60%>
<Projectile Stage 2 Trigger: time 30>
<Projectile Stage 2 Trigger: distance 160>
```

### Does Stage 2 have advanced targeting?

Current documented targeting is:

```text
<Projectile Stage 2 Targeting: original>
```

Future versions may expand this.

### Can projectiles have trails or glow?

Yes.

```text
<Projectile Trail: true>
<Projectile Afterimage Interval: 4>
<Projectile Afterimage Duration: 18>
<Projectile Glow: true>
```

### Can projectiles be animated?

Yes. Horizontal sprite sheet frames:

```text
<Projectile: FireballSheet>
<Projectile Frames: 8>
<Projectile Animation FPS: 12>
```

Image sequence:

```text
<Projectile Images: Fire1;Fire2;Fire3;Fire4>
<Projectile Animation FPS: 12>
```

### Can a skill pick from random projectile images?

Yes.

```text
<Projectile Pool: Fire1;Fire2;Fire3>
<Projectile Pool Mode: random>
```

Cycle mode is also supported:

```text
<Projectile Pool: WaterBall;FireBall;Spark>
<Projectile Pool Mode: cycle>
```

---

## Sound Questions

### Can a projectile play a sound when launched?

Yes.

```text
<Projectile Launch SE: Bow1>
<Projectile Launch SE Volume: 90>
<Projectile Launch SE Pitch: 100>
<Projectile Launch SE Pan: 0>
```

### Can a projectile play a sound while flying?

Yes.

```text
<Projectile Airborne SE: Wind4>
<Projectile Airborne SE Volume: 70>
<Projectile Airborne SE Pitch: 100>
<Projectile Airborne SE Pan: 0>
<Projectile Airborne SE Interval: 12>
```

Leave sound names blank or omit the tags for no sound.

---

## Troubleshooting Questions

### Nothing happens when I use the skill.

Check:

1. The skill has a projectile notetag.
2. The projectile image exists in `img/projectiles/`.
3. The plugin is enabled in MV Plugin Manager.
4. Another battle plugin is not loaded after Projectile Core and overriding battle flow.
5. The skill actually targets an enemy or ally that can receive the action.

### Damage happens before the projectile hits.

Move Okapi Projectile Core below battle flow plugins such as YEP_BattleEngineCore and Action Sequence packs. Projectile Core needs to control the delayed impact flow.

### My skill animation plays at the wrong time.

Projectile Core suppresses the early skill/item animation and plays impact animation when the projectile hits. If another plugin forces early animation, check plugin order.

### My projectile works in playtest but not deployed build.

Check capitalization and deployment packaging. Web and Windows deployments can be stricter about exact filenames than local playtest.

### Should projectile art face right or left?

Right-facing art is recommended by default. Use `Projectile Image Facing` if your art naturally faces left.

### What is the safest first test skill?

Use a single-target fireball:

```text
<Projectile: Fireball>
<Projectile Path: straight>
<Projectile Speed: 14>
<Projectile Image Facing: right>
<Projectile Auto Flip: true>
```

Make sure `img/projectiles/Fireball.png` exists.

---

## Commercial Use Questions

### Can I use this in a commercial RPG Maker MV project?

The plugin header states that it is free to use and modify for personal or commercial RPG Maker MV projects.

### Can I edit the plugin?

Yes, according to the plugin terms in the header. Keep a backup before editing the plugin code.

### Should I include documentation with my release?

Yes. For an end-user release, include at least:

```text
README.md
QUICK_START.md
NOTETAG_GUIDE.md
COMPATIBILITY.md
FAQ.md
CHANGELOG.md
LICENSE.md
TERMS_OF_USE.md
TROUBLESHOOTING.md
```
