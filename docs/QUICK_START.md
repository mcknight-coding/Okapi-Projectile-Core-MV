# Okapi Projectile Core - Quick Start Guide

_For RPG Maker MV - Plugin Version v2.20_

**Created for Okapi Interactive / McKnight Coding**

**Purpose:** This guide helps a new user install the plugin, add projectile images, create a first projectile skill, test common projectile paths, and troubleshoot the most common setup issues.

## At a Glance

| Need | Where / Tag | Example |
| --- | --- | --- |
| Plugin file | js/plugins/ | Okapi_ProjectileCore.js |
| Projectile images | img/projectiles/ | Fireball.png, Arrow.png |
| Minimum skill tag | Skill note box | `<Projectile: Fireball>` |
| Recommended path tag | Skill note box | `<Projectile Path: arc>` |
| Safe image checking | Plugin parameter | Strict Missing Projectile Image Check: false |

## Table of Contents

- 1. What This Plugin Does
- 2. Install the Plugin
- 3. Add Projectile Images
- 4. Create Your First Projectile Skill
- 5. Basic Notetag Format
- 6. Path vs Behavior
- 7. Starter Projectile Examples
- 8. Start Points, Hit Points, and Offsets
- 9. Image Facing, Flipping, and Rotation
- 10. Sound Effects
- 11. Multi-target Timing and Repeat Waves
- 12. Stage 2 Projectiles and Split Shots
- 13. Random Pools, Animation, Trails, and Glow
- 14. Weapon Priority and Projectile Overrides
- 15. Demo-ready Skill Examples
- 16. Enemy, Weapon, and State Examples
- 17. Troubleshooting
- 18. Release Checklist
- Appendix A. Quick Notetag Reference

## 1. What This Plugin Does

Okapi Projectile Core lets RPG Maker MV actions launch visible projectiles in battle. Skills, items, weapons, actors, enemies, states, classes, and equipment can use notetags to control projectile image, path, speed, hit timing, sound, animation, and special behavior.

Instead of damage happening instantly, the plugin can delay the action result until the projectile reaches the target.

| Use Case | Projectile Example |
| --- | --- |
| Basic magic | Fireball, ice shard, lightning bolt |
| Ranged weapons | Arrow, bullet, thrown dagger |
| Heavy attacks | Boulder, barrel, rolling bomb |
| Advanced skills | Mortar shot, homing shot, split burst, boomerang |
| Visual polish | Afterimages, glow, orbiters, animated sheets |

## 2. Install the Plugin

Place the plugin file inside your RPG Maker MV project:

```text
YourProject/js/plugins/Okapi_ProjectileCore.js
```

**Important:** For public release, use the clean filename Okapi_ProjectileCore.js. Do not ship the file as Okapi_ProjectileCore(6).js or another numbered copy name.

Then enable it in RPG Maker MV:

```text
Tools -> Plugin Manager -> Add Plugin -> Okapi_ProjectileCore -> ON
```

### Recommended First Settings

```text
Projectile Folder: projectiles/
Missing Projectile Fallback: blank
Warn Missing Projectile Images: true
Strict Missing Projectile Image Check: false
Default FPS: 60
Default Speed: 14
Default Arc Height: 72
Default Rotation Speed: 0.20
Default Start Point: frontMiddle
Default Hit Point: frontMiddle
Default Image Facing: right
Auto Flip To Travel Direction: true
Projectile Layer: aboveBattlers
```

Safest demo setting: Keep Strict Missing Projectile Image Check set to false. This avoids false file-check failures in playtest, browser builds, or deployed builds.

## 3. Add Projectile Images

Create this folder inside your project:

```text
YourProject/img/projectiles/
```

Place projectile PNG images inside it:

```text
img/projectiles/Fireball.png
img/projectiles/Arrow.png
img/projectiles/Boomerang.png
img/projectiles/Boulder.png
img/projectiles/beam_yellow.png
img/projectiles/blast_yellow.png
```

In notetags, use the image name without .png:

```text
<Projectile: Fireball>
```

For clean documentation, teach users to leave off the file extension even though the plugin can clean .png from names.

## 4. Create Your First Projectile Skill

Open the RPG Maker MV database and create or edit a skill:

```text
Database -> Skills -> Pick a skill -> Notes box
```

Example skill setup:

| Field | Suggested Value |
| --- | --- |
| Name | Fireball |
| Scope | 1 Enemy |
| Hit Type | Magical Attack |
| Damage Type | HP Damage |
| Formula | a.mat * 2 - b.mdf |
| Animation | Fire One |

Add this to the skill note box:

```text
<Projectile: Fireball>
<Projectile Path: straight>
<Projectile Speed: 14>
```

**Expected result:** the user acts, the Fireball image launches, the projectile travels to the target, and damage resolves when it reaches the target.

## 5. Basic Notetag Format

Minimum required notetag:

```text
<Projectile: Fireball>
```

Common full setup:

```text
<Projectile: Fireball>
<Projectile Path: straight>
<Projectile Speed: 14>
<Projectile FPS: 60>
<Projectile Start: frontMiddle>
<Projectile Hit: frontMiddle>
<Projectile Scale: 1.0>
<Projectile Rotation: true>
<Projectile Rotation Speed: 0.20>
<Projectile Image Facing: right>
<Projectile Auto Flip: true>
```

## 6. Path vs Behavior

Use the newer clean tags for public documentation:

```text
<Projectile Path: straight>
<Projectile Behavior: normal>
```

The older tag still works for compatibility:

```text
<Projectile Mode: straight>
```

| Concept | Meaning | Examples |
| --- | --- | --- |
| Path | How the projectile travels | straight, arc, mortar, homing, sine, zigzag, rolling |
| Behavior | Special behavior added on top | normal, boomerang, chain, orbit |

## 7. Starter Projectile Examples

### Straight Shot

Good for beams, bullets, bolts, and basic fireballs.

```text
<Projectile: beam_yellow>
<Projectile Path: straight>
<Projectile Speed: 18>
```

### Arc Shot

Good for arrows, thrown rocks, thrown knives, and magic shots with a visible curve.

```text
<Projectile: Arrow>
<Projectile Path: arc>
<Projectile Arc: 72>
<Projectile Speed: 16>
<Projectile Start: frontMiddle>
<Projectile Hit: frontMiddle>
```

### Mortar Shot

Good for bombs, meteors, boulders, rising magic, or anything that launches upward first.

```text
<Projectile: Boulder>
<Projectile Path: mortar>
<Projectile Launch Height: 160>
<Projectile Mortar Phase: 0.35>
<Projectile Face Path: true>
<Projectile Speed: 14>
```

### Rolling Projectile

Good for boulders, barrels, rolling bombs, or ground attacks.

```text
<Projectile: Boulder>
<Projectile Path: rolling>
<Projectile Speed: 10>
<Projectile Rotation: true>
<Projectile Rotation Speed: 0.30>
```

### Boomerang Projectile

Good for returning weapons.

```text
<Projectile: Boomerang>
<Projectile Path: straight>
<Projectile Behavior: boomerang>
<Projectile Speed: 16>
<Projectile Rotation: true>
<Projectile Rotation Speed: 0.35>
```

### Homing Projectile

Good for magic missiles and seeker shots.

```text
<Projectile: magic_missile>
<Projectile Path: homing>
<Projectile Homing Strength: 0.35>
<Projectile Speed: 14>
```

### Zigzag Projectile

Good for lightning, unstable magic, chaos bolts, and erratic shots.

```text
<Projectile: lightning_bolt>
<Projectile Path: zigzag>
<Projectile Zigzag Width: 40>
<Projectile Zigzag Segments: 6>
<Projectile Speed: 16>
```

### Sine / Wave Projectile

Good for wave beams, sonic attacks, and floating magic.

```text
<Projectile: wave_blue>
<Projectile Path: sine>
<Projectile Sine Amplitude: 32>
<Projectile Sine Frequency: 3>
<Projectile Speed: 14>
```

## 8. Start Points, Hit Points, and Offsets

Use start and hit points to control where the projectile begins and where it lands.

| Vertical | Front | Center | Back |
| --- | --- | --- | --- |
| Top | frontTop | centerTop | backTop |
| Middle | frontMiddle | centerMiddle | backMiddle |
| Bottom | frontBottom | centerBottom | backBottom |

Common setup:

```text
<Projectile Start: frontMiddle>
<Projectile Hit: frontMiddle>
```

Low rolling projectile:

```text
<Projectile Start: frontBottom>
<Projectile Hit: frontBottom>
```

Back-hit setup:

```text
<Projectile Hit: backMiddle>
<Projectile Back Attack: true>
<Projectile Back Arc Height: 120>
<Projectile Back Overshoot: 72>
```

### Offsets

Offsets fine-tune the exact position.

```text
<Projectile Start Offset X: 0>
<Projectile Start Offset Y: 0>
<Projectile Hit Offset X: 0>
<Projectile Hit Offset Y: 0>
```

| Goal | Tag Example |
| --- | --- |
| Start slightly higher | `<Projectile Start Offset Y: -20>` |
| Hit slightly lower | `<Projectile Hit Offset Y: 20>` |
| Start slightly forward | `<Projectile Start Offset X: 20>` |

## 9. Image Facing, Flipping, and Rotation

Recommended art rule: draw projectile images facing right and keep Auto Flip on.

```text
<Projectile Image Facing: right>
<Projectile Auto Flip: true>
```

If your projectile art naturally faces left:

```text
<Projectile Image Facing: left>
<Projectile Auto Flip: true>
```

### Rotation

Use rotation for spinning projectiles such as boomerangs, shuriken, boulders, and energy discs.

```text
<Projectile Rotation: true>
<Projectile Rotation Speed: 0.20>
```

For arrows or beams, avoid normal spin and use path-facing instead:

```text
<Projectile Face Path: true>
<Projectile Rotation: false>
```

## 10. Sound Effects

### Launch Sound

Launch sound plays once when the projectile is created.

```text
<Projectile Launch SE: Bow1>
<Projectile Launch SE Volume: 90>
<Projectile Launch SE Pitch: 100>
<Projectile Launch SE Pan: 0>
```

### Airborne Sound

Airborne sound repeats while the projectile travels.

```text
<Projectile Airborne SE: Wind4>
<Projectile Airborne SE Volume: 70>
<Projectile Airborne SE Pitch: 100>
<Projectile Airborne SE Pan: 0>
<Projectile Airborne SE Interval: 12>
```

**Tip:** Use airborne sounds carefully. They can sound great on slow dramatic projectiles, but they may become noisy on fast multi-hit skills.

## 11. Multi-target Timing and Repeat Waves

### Projectile Timing

Default behavior launches all projectiles at the same time:

```text
<Projectile Timing: simultaneous>
```

Sequential behavior launches projectiles one after another:

```text
<Projectile Timing: sequential>
```

| Timing | Best For |
| --- | --- |
| simultaneous | AOE magic, multi-shot attacks, enemy group attacks |
| sequential | Dramatic chains, one-target-at-a-time skills, boss attacks |

### Repeat Waves

If a skill has RPG Maker MV Repeat greater than 1, each repeat can become a delayed projectile wave instead of stacking instantly.

```text
<Projectile Waves: 2>
<Projectile Wave Delay: 120>
```

Alternative seconds format:

```text
<Projectile Waves: 2>
<Projectile Wave Delay Seconds: 2>
```

Example volley:

```text
<Projectile: arrow_yellow>
<Projectile Path: arc>
<Projectile Arc: 72>
<Projectile Speed: 18>
<Projectile Waves: 3>
<Projectile Wave Delay: 45>
```

## 12. Stage 2 Projectiles and Split Shots

Projectile stages let one projectile transform, split, or change behavior mid-flight.

| Stage | What Happens |
| --- | --- |
| Stage 1 | Main projectile launches toward original target. |
| Trigger | Stage 2 activates by progress, time, or distance. |
| Stage 2 | Projectile changes image, path, speed, count, spread, or damage behavior. |
| Impact | Final projectile applies damage on impact. |

### Stage 2 Trigger Options

```text
<Projectile Stage 2 Trigger: progress 60%>
<Projectile Stage 2 Trigger: time 30>
<Projectile Stage 2 Trigger: distance 160>
```

### Split Burst Example

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

### Stage 2 Damage

Each split projectile can damage:

```text
<Projectile Stage 2 Damage: each>
```

Or the split can be mostly visual with one final damage result:

```text
<Projectile Stage 2 Damage: once>
```

## 13. Random Pools, Animation, Trails, and Glow

### Random Projectile Pool

```text
<Projectile Pool: Fire1;Fire2;Fire3>
<Projectile Pool Mode: random>
```

### Cycle Projectile Pool

```text
<Projectile Pool: WaterBall;FireBall;Spark>
<Projectile Pool Mode: cycle>
```

### Animated Projectile Sheet

```text
<Projectile: FireballSheet>
<Projectile Frames: 8>
<Projectile Animation FPS: 12>
```

### Image Sequence Animation

```text
<Projectile Images: Fire1;Fire2;Fire3;Fire4>
<Projectile Animation FPS: 12>
```

### Trail / Afterimage

```text
<Projectile Trail: true>
<Projectile Afterimage: true>
<Projectile Afterimage Interval: 4>
<Projectile Afterimage Duration: 18>
```

### Glow

```text
<Projectile Glow: true>
```

### Orbiters

```text
<Projectile Orbiters: 3>
<Projectile Orbiter: Spark>
<Projectile Orbiter Radius: 32>
<Projectile Orbiter Speed: 0.15>
```

## 14. Weapon Priority and Projectile Overrides

For weapon-element skills, also known as Normal Attack element or element -1, projectile settings can come from the subject and its related note boxes. This is useful for attacks where the equipped weapon should decide the projectile.

| Priority | Source |
| --- | --- |
| 1 | States on the subject |
| 2 | Actor primary weapon |
| 3 | Actor other weapons |
| 4 | Actor armors |
| 5 | Enemy database notes |
| 6 | Actor database notes |
| 7 | Actor class notes |
| 8 | Skill or item notes |

v2.20 safety rule: If a non-weapon-element skill or item has its own projectile note box, that skill owns its movement path, hit point, timing, stages, and behavior. Weapon raw tags should not turn a named skill into a straight shot.

### Projectile Override Format

```text
<Projectile Override>
Element: Cold
Projectile: ice_shard
Path: arc
Arc: 96
Trail: true
</Projectile Override>
```

Supported condition examples: Skill Type, Element, Element ID, Damage Type, Hit Type, Skill ID, Weapon Type, Actor ID, Class ID, Enemy ID, and State.

## 15. Demo-ready Skill Examples

### Straight Shot

```text
Name: Straight Shot
Description: Fires a direct projectile at one enemy.
<Projectile: beam_yellow>
<Projectile Path: straight>
<Projectile Speed: 18>
```

### Arc Shot

```text
Name: Arc Shot
Description: Fires a curved projectile at one enemy.
<Projectile: arrow_yellow>
<Projectile Path: arc>
<Projectile Arc: 90>
<Projectile Speed: 16>
<Projectile Start: frontMiddle>
<Projectile Hit: frontMiddle>
```

### Mortar Blast

```text
Name: Mortar Blast
Description: Launches a projectile upward before it drops into the target.
<Projectile: blast_red>
<Projectile Path: mortar>
<Projectile Launch Height: 160>
<Projectile Mortar Phase: 0.35>
<Projectile Face Path: true>
<Projectile Speed: 14>
```

### Split Burst

```text
Name: Split Burst
Description: Fires an arcing projectile that splits before impact.
<Projectile: beam_yellow>
<Projectile Path: arc>
<Projectile Arc: 100>
<Projectile Stage 2 Trigger: distance 160>
<Projectile Stage 2 Image: blast_yellow>
<Projectile Stage 2 Path: straight>
<Projectile Stage 2 Count: 3>
<Projectile Stage 2 Spread: 30>
<Projectile Stage 2 Speed: 22>
<Projectile Stage 2 Damage: each>
```

### Boomerang Toss

```text
Name: Boomerang Toss
Description: Throws a projectile that returns after hitting the target.
<Projectile: boomerang>
<Projectile Path: straight>
<Projectile Behavior: boomerang>
<Projectile Speed: 16>
<Projectile Rotation: true>
<Projectile Rotation Speed: 0.35>
```

## 16. Enemy, Weapon, and State Examples

### Enemy Notetag Example

```text
<Projectile: sludge_ball>
<Projectile Path: arc>
<Projectile Arc: 60>
<Projectile Speed: 12>
```

Good enemy examples include a slime throwing sludge, a goblin throwing a rock, a mage firing an orb, an archer firing an arrow, or a boss launching a mortar blast.

### Weapon Notetag Example

```text
<Projectile: arrow_yellow>
<Projectile Path: arc>
<Projectile Arc: 72>
<Projectile Speed: 18>
<Projectile Motion: missile>
```

Use weapon notetags mostly for Attack or weapon-element skills. For named skills, put the important projectile path directly on the skill.

### State Override Example

```text
<Projectile Override>
Element: Fire
Projectile: fireball
Glow: true
Trail: true
</Projectile Override>
```

## 17. Troubleshooting

| Problem | Likely Cause | Fix |
| --- | --- | --- |
| Projectile does not appear | Missing image, typo, wrong folder, plugin off | Check img/projectiles, filename spelling, and Plugin Manager. |
| Damage happens but no visual | Projectile visual skipped safely | Check console warnings and optional fallback image. |
| Arc or mortar acts straight | Wrong tag or weapon-element override | Use `<Projectile Path: arc>` or `<Projectile Path: mortar>` on the named skill. |
| Projectile upside down | Rotation used instead of flip/path-facing | Use Auto Flip true; for arrows use Face Path true and Rotation false. |
| Starts from wrong place | Anchor point mismatch | Use Start point or Start Offset X/Y. |
| Hits wrong body area | Hit anchor mismatch | Use Hit point or Hit Offset X/Y. |
| Repeat damage too fast | Repeats resolve too close together | Use Projectile Waves and Wave Delay. |

## 18. Release Checklist

Use this checklist before uploading the plugin to Itch.io or sharing it with testers.

- [ ] Plugin file is named Okapi_ProjectileCore.js.
- [ ] Plugin turns ON in a clean RPG Maker MV project.
- [ ] img/projectiles folder exists.
- [ ] Demo includes several sample projectile PNG files.
- [ ] Straight projectile works and damage waits until impact.
- [ ] Arc projectile visibly curves.
- [ ] Mortar projectile launches upward first.
- [ ] Boomerang returns after impact.
- [ ] Rolling projectile stays low.
- [ ] Split projectile triggers Stage 2.
- [ ] Multi-target skill launches correctly.
- [ ] Repeat skill creates delayed waves.
- [ ] Enemy projectile works.
- [ ] Weapon projectile works for Attack.
- [ ] Named skill keeps its own path.
- [ ] Missing image does not crash the game.
- [ ] Left-moving projectile is not upside down.
- [ ] Sound effects play correctly.
- [ ] Browser and Windows deployed builds have been tested.

## Recommended Files for the Product Zip

```text
Okapi_ProjectileCore.js
QUICK_START.docx
README.md
LICENSE.txt
CHANGELOG.md
Notetag_Examples.md
Demo_Project/
Demo_Windows.zip
Demo_Browser/
img/projectiles sample images
```

## Appendix A. Quick Notetag Reference

| Category | Notetags |
| --- | --- |
| Core | `<Projectile: Fireball>`<br>`<Projectile Speed: 14>`<br>`<Projectile Scale: 1.0>` |
| Path | `<Projectile Path: straight>`<br>`<Projectile Path: arc>`<br>`<Projectile Path: mortar>`<br>`<Projectile Path: homing>` |
| Behavior | `<Projectile Behavior: normal>`<br>`<Projectile Behavior: boomerang>`<br>`<Projectile Behavior: chain>` |
| Arc / Mortar | `<Projectile Arc: 90>`<br>`<Projectile Launch Height: 160>`<br>`<Projectile Mortar Phase: 0.35>` |
| Facing | `<Projectile Image Facing: right>`<br>`<Projectile Auto Flip: true>`<br>`<Projectile Face Path: true>` |
| Stage 2 | `<Projectile Stage 2 Trigger: progress 60%>`<br>`<Projectile Stage 2 Image: beam_green>`<br>`<Projectile Stage 2 Count: 3>` |
| Sounds | `<Projectile Launch SE: Bow1>`<br>`<Projectile Airborne SE: Wind4>` |
