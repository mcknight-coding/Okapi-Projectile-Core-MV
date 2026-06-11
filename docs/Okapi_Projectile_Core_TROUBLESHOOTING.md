<p align="center"><strong>Okapi Interactive / McKnight Coding</strong><br />Okapi Projectile Core Documentation</p>

---

# TROUBLESHOOTING.md — Okapi Projectile Core

**Brand:** Okapi Interactive / McKnight Coding

This troubleshooting guide is for **Okapi Projectile Core v2.20** for RPG Maker MV.

Use this file when projectiles do not appear, damage happens at the wrong time, arc/mortar paths act wrong, weapon tags override skills, Stage 2 projectiles do not split, or deployed builds behave differently from playtest.

---

## 1. Start Here: Fast Diagnosis Checklist

Before changing many settings, check these first:

```text
[ ] The plugin file is in js/plugins/.
[ ] The plugin is turned ON in Plugin Manager.
[ ] The projectile image is in img/projectiles/.
[ ] The notetag image name matches the PNG filename.
[ ] The notetag does not have a spelling mistake.
[ ] The skill/item has <Projectile: ImageName>.
[ ] Strict Missing Projectile Image Check is OFF.
[ ] The skill has a valid target scope.
[ ] The skill has a valid damage formula or effect.
[ ] The battle is being tested in a clean project if possible.
```

Recommended safe plugin setting:

```text
Strict Missing Projectile Image Check: false
Warn Missing Projectile Images: true
Projectile Layer: aboveBattlers
Auto Flip To Travel Direction: true
Default Image Facing: right
```

---

## 2. Correct Folder Setup

Projectile images should go here:

```text
YourProject/img/projectiles/
```

Example:

```text
YourProject/img/projectiles/Fireball.png
YourProject/img/projectiles/Arrow.png
YourProject/img/projectiles/Boulder.png
```

Use the file name in the notetag:

```text
<Projectile: Fireball>
```

The plugin looks in the configured projectile folder under `img/`.

Default folder:

```text
img/projectiles/
```

---

## 3. Projectile Does Not Appear

### Likely Causes

```text
The image is missing.
The image is in the wrong folder.
The filename is misspelled.
The plugin is OFF.
The skill does not have a projectile notetag.
Strict image checking blocked the visual.
The projectile layer was not created.
```

### Fix

Use a very simple test first:

```text
<Projectile: Fireball>
<Projectile Path: straight>
<Projectile Speed: 14>
```

Then confirm:

```text
img/projectiles/Fireball.png
```

Recommended plugin setting:

```text
Strict Missing Projectile Image Check: false
```

If you want a fallback image, set:

```text
Missing Projectile Fallback: Fireball
```

Then make sure this exists:

```text
img/projectiles/Fireball.png
```

---

## 4. Damage Happens, But No Projectile Appears

### What This Usually Means

The plugin is probably resolving the action safely because the projectile visual could not be displayed.

### Check

```text
[ ] Is the image filename correct?
[ ] Is the image in img/projectiles/?
[ ] Is the image a PNG?
[ ] Is the plugin parameter Projectile Folder set to projectiles/?
[ ] Is Strict Missing Projectile Image Check set to false?
[ ] Is the battle scene using a normal RPG Maker MV battle spriteset?
```

### Fix

Use this exact test:

```text
<Projectile: Fireball>
<Projectile Path: straight>
<Projectile Speed: 14>
```

Create or copy:

```text
img/projectiles/Fireball.png
```

If this works, the plugin is installed correctly and the issue is likely the original notetag or image name.

---

## 5. Missing Image Crash or Missing Image Warning

### Recommended Setting

Keep this OFF:

```text
Strict Missing Projectile Image Check: false
```

Strict checking can be useful during development, but passive checking is safer for playtest and deployment.

### Safer Release Setup

```text
Warn Missing Projectile Images: true
Strict Missing Projectile Image Check: false
Missing Projectile Fallback: 
```

Or, if you want a fallback:

```text
Missing Projectile Fallback: fallback_projectile
```

Then include:

```text
img/projectiles/fallback_projectile.png
```

---

## 6. Arc Projectile Acts Like a Straight Shot

### Common Causes

```text
The skill uses the wrong tag.
The skill uses weapon element -1 and a weapon tag is overriding it.
The skill has no projectile path tag.
The path was written as Behavior instead of Path.
The old Mode tag is being overridden by another source.
```

### Correct Arc Setup

```text
<Projectile: Arrow>
<Projectile Path: arc>
<Projectile Arc: 90>
<Projectile Speed: 16>
```

### Optional Force Arc

If needed, try:

```text
<Projectile: Arrow>
<Projectile Path: arc>
<Projectile Arc: 90>
<Projectile Force Arc: true>
<Projectile Speed: 16>
```

### Important

For named skills, avoid relying on weapon tags for the movement path. Put the path directly on the skill.

---

## 7. Mortar Projectile Acts Like a Straight Shot

### Correct Mortar Setup

```text
<Projectile: Boulder>
<Projectile Path: mortar>
<Projectile Launch Height: 160>
<Projectile Mortar Phase: 0.35>
<Projectile Face Path: true>
<Projectile Speed: 14>
```

### Common Mistakes

Wrong:

```text
<Projectile Behavior: mortar>
```

Correct:

```text
<Projectile Path: mortar>
```

Path controls movement shape. Behavior controls special behavior like boomerang or chain.

---

## 8. Weapon Tags Override My Named Skill

### Why This Happens

RPG Maker MV skills with damage element `-1` use the weapon element. For those skills, projectile settings may come from the actor’s states, weapons, armor, enemy notes, actor notes, class notes, or skill notes.

### Fix for Named Skills

For a named skill, avoid using damage element `-1` unless you want weapon-element behavior.

Use a specific element instead, then put projectile settings directly on the skill:

```text
<Projectile: arrow_yellow>
<Projectile Path: arc>
<Projectile Arc: 90>
<Projectile Speed: 16>
```

### Correct Use of Weapon Tags

Use weapon projectile tags mostly for:

```text
Normal Attack
weapon-element skills
basic attack replacement skills
```

### Use Projectile Overrides for Conditional Equipment Behavior

Instead of raw weapon tags forcing every skill, use:

```text
<Projectile Override>
Weapon Type: Bow
Projectile: arrow_yellow
Path: arc
Arc: 90
Speed: 18
</Projectile Override>
```

---

## 9. Projectile Is Upside Down

### Likely Cause

The projectile is rotating to face its path, but the image is also being flipped or rotated in a way that makes it appear inverted.

### Recommended Setup for Arrows, Spears, and Beams

```text
<Projectile Image Facing: right>
<Projectile Auto Flip: true>
<Projectile Face Path: true>
<Projectile Rotation: false>
```

### Recommended Setup for Spinning Objects

```text
<Projectile Image Facing: right>
<Projectile Auto Flip: true>
<Projectile Rotation: true>
<Projectile Rotation Speed: 0.20>
```

Use rotation for:

```text
boomerangs
boulders
shuriken
energy discs
spinning blades
```

Avoid normal spin rotation for:

```text
arrows
spears
beams
bullets
lasers
```

---

## 10. Projectile Faces the Wrong Direction

### Fix

If your projectile art faces right:

```text
<Projectile Image Facing: right>
<Projectile Auto Flip: true>
```

If your projectile art faces left:

```text
<Projectile Image Facing: left>
<Projectile Auto Flip: true>
```

Recommended art rule:

```text
Draw projectile art facing right.
Keep Auto Flip ON.
```

---

## 11. Projectile Starts From the Wrong Spot

### Fix with Start Point

```text
<Projectile Start: frontMiddle>
```

Other useful start points:

```text
<Projectile Start: frontTop>
<Projectile Start: centerMiddle>
<Projectile Start: frontBottom>
```

### Fix with Start Offset

Move start point forward:

```text
<Projectile Start Offset X: 20>
```

Move start point higher:

```text
<Projectile Start Offset Y: -20>
```

Move start point lower:

```text
<Projectile Start Offset Y: 20>
```

---

## 12. Projectile Hits the Wrong Spot

### Fix with Hit Point

```text
<Projectile Hit: frontMiddle>
```

Low hit for ground projectiles:

```text
<Projectile Hit: frontBottom>
```

Center hit:

```text
<Projectile Hit: centerMiddle>
```

### Fix with Hit Offset

Hit slightly higher:

```text
<Projectile Hit Offset Y: -20>
```

Hit slightly lower:

```text
<Projectile Hit Offset Y: 20>
```

---

## 13. Rolling Projectile Floats Too High

Use frontBottom start and hit points:

```text
<Projectile: Boulder>
<Projectile Path: rolling>
<Projectile Start: frontBottom>
<Projectile Hit: frontBottom>
<Projectile Speed: 10>
<Projectile Rotation: true>
<Projectile Rotation Speed: 0.30>
```

If it still floats, lower the hit point:

```text
<Projectile Hit Offset Y: 20>
```

---

## 14. Boomerang Does Not Return

### Correct Boomerang Setup

```text
<Projectile: Boomerang>
<Projectile Path: straight>
<Projectile Behavior: boomerang>
<Projectile Speed: 16>
<Projectile Rotation: true>
<Projectile Rotation Speed: 0.35>
```

### Common Mistake

Wrong:

```text
<Projectile Path: boomerang>
```

Better:

```text
<Projectile Path: straight>
<Projectile Behavior: boomerang>
```

Old `<Projectile Mode: boomerang>` may still work, but new docs should use Path + Behavior.

---

## 15. Chain Projectile Does Not Jump Targets

### Correct Chain Setup

```text
<Projectile: lightning_bolt>
<Projectile Path: straight>
<Projectile Behavior: chain>
<Projectile Chains: 3>
<Projectile Chain Range: 9999>
<Projectile Chain Repeat: false>
<Projectile Chain Target: nearest>
```

### Chain Target Options

```text
nearest
farthest
random
lowestHp
highestHp
```

### Notes

Chain requires valid living targets. If there are no other valid targets, it may stop after the first target.

---

## 16. Bounce Is Not Chain

In newer documentation, use:

```text
<Projectile Behavior: chain>
```

for target-to-target jumping.

Use:

```text
<Projectile Path: bounce>
```

for bouncing-ball movement.

If an old skill used bounce as ricochet behavior, update it to chain.

---

## 17. Stage 2 Does Not Trigger

### Correct Stage 2 Example

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

### Trigger Options

```text
<Projectile Stage 2 Trigger: progress 60%>
<Projectile Stage 2 Trigger: time 30>
<Projectile Stage 2 Trigger: distance 160>
```

### Common Mistakes

```text
The Stage 2 image is missing.
The trigger distance is too large or too small.
The projectile reaches the target before Stage 2 triggers.
Stage 2 path is written as Behavior.
Stage 2 Count is missing or set to 1.
```

### Fix

Try progress first because it is easiest to see:

```text
<Projectile Stage 2 Trigger: progress 50%>
```

Then test distance-based triggers after the visual works.

---

## 18. Stage 2 Splits, But Only One Hit Happens

Check damage mode:

```text
<Projectile Stage 2 Damage: each>
```

If you only want one final damage result, use:

```text
<Projectile Stage 2 Damage: once>
```

For split attacks, use `each`.

---

## 19. Split Projectiles Miss the Target

### Try Lower Spread

```text
<Projectile Stage 2 Spread: 15>
```

Instead of:

```text
<Projectile Stage 2 Spread: 45>
```

### Try Homing Stage 2

```text
<Projectile Stage 2 Path: homing>
<Projectile Stage 2 Speed: 18>
```

### Safer Split Template

```text
<Projectile: beam_yellow>
<Projectile Path: arc>
<Projectile Arc: 100>
<Projectile Stage 2 Trigger: progress 60%>
<Projectile Stage 2 Image: blast_yellow>
<Projectile Stage 2 Path: homing>
<Projectile Stage 2 Count: 3>
<Projectile Stage 2 Spread: 20>
<Projectile Stage 2 Speed: 20>
<Projectile Stage 2 Damage: each>
```

---

## 20. Repeat Skill Hits Instantly Instead of as Waves

Use waves:

```text
<Projectile Waves: 2>
<Projectile Wave Delay: 120>
```

Or:

```text
<Projectile Waves: 3>
<Projectile Wave Delay: 45>
```

`120` frames is about 2 seconds at 60 FPS.

`60` frames is about 1 second at 60 FPS.

`30` frames is about half a second at 60 FPS.

---

## 21. Multi-Target Projectiles Fire Too Fast

Use sequential timing:

```text
<Projectile Timing: sequential>
```

Use simultaneous timing when all targets should be hit at once:

```text
<Projectile Timing: simultaneous>
```

Recommended:

```text
AOE spell: simultaneous
cinematic multi-hit skill: sequential
chain-like boss attack: sequential
arrow volley: simultaneous
```

---

## 22. Animation Plays Before Projectile Impact

### With Default RPG Maker MV

Make sure the skill’s projectile tags are correct and the action is not duplicated by another battle plugin.

### With Yanfly Battle Engine Core

If using Yanfly action sequences, make sure the projectile action effect is not being duplicated by custom action sequence lines.

Test in this order:

```text
1. Clean project without Yanfly.
2. Project with only Yanfly Battle Engine Core.
3. Project with your full plugin list.
```

If it only breaks in step 3, the issue is likely a plugin conflict or action sequence conflict.

---

## 23. Skill Plays Twice or Damage Applies Twice

### Likely Causes

```text
Yanfly action sequence repeats ACTION EFFECT.
Another battle plugin runs damage separately.
The skill has repeat count plus projectile waves.
The skill has both normal damage and custom action sequence damage.
```

### Fix

Test the skill without custom action sequence commands.

If using Yanfly, keep only one real damage/action effect call for the projectile skill.

---

## 24. Enemy Projectile Does Not Work

### Check Enemy Note

Enemy note example:

```text
<Projectile: sludge_ball>
<Projectile Path: arc>
<Projectile Arc: 60>
<Projectile Speed: 12>
```

### Check Enemy Skill

Make sure the enemy actually uses a skill that allows projectile settings to apply.

For safest testing, put the projectile directly on the enemy skill first:

```text
<Projectile: sludge_ball>
<Projectile Path: arc>
<Projectile Arc: 60>
<Projectile Speed: 12>
```

If that works, move the projectile tags to the enemy note only if you specifically want enemy-based projectile behavior.

---

## 25. Weapon Projectile Does Not Work

### Use Weapon Tags for Attack

Put this on a bow weapon:

```text
<Projectile: arrow_yellow>
<Projectile Path: arc>
<Projectile Arc: 72>
<Projectile Speed: 18>
<Projectile Motion: missile>
```

Then test the normal Attack command.

### Check Skill Element

Weapon projectile tags are most useful when the action uses weapon element / Normal Attack element.

If the skill uses a specific element and has its own projectile, the skill may own its own projectile behavior.

---

## 26. Projectile Override Does Not Work

### Check Format

Correct:

```text
<Projectile Override>
Element: Cold
Projectile: ice_shard
Path: arc
Arc: 96
Trail: true
</Projectile Override>
```

### Check Conditions

Multiple conditions use AND logic.

This means all listed conditions must match:

```text
<Projectile Override>
Skill Type: Magic
Element: Cold
Projectile: ice_shard
Path: arc
</Projectile Override>
```

The override above only works if the skill is both Magic and Cold.

### Test with One Condition First

```text
<Projectile Override>
Element: Cold
Projectile: ice_shard
Path: arc
</Projectile Override>
```

After that works, add more conditions.

---

## 27. Sound Effect Does Not Play

### Launch SE

```text
<Projectile Launch SE: Bow1>
<Projectile Launch SE Volume: 90>
<Projectile Launch SE Pitch: 100>
<Projectile Launch SE Pan: 0>
```

Check:

```text
audio/se/Bow1.ogg
audio/se/Bow1.m4a
```

### Airborne SE

```text
<Projectile Airborne SE: Wind4>
<Projectile Airborne SE Volume: 70>
<Projectile Airborne SE Pitch: 100>
<Projectile Airborne SE Pan: 0>
<Projectile Airborne SE Interval: 12>
```

If airborne sound is too noisy, increase the interval:

```text
<Projectile Airborne SE Interval: 30>
```

---

## 28. Animated Projectile Does Not Animate

### Horizontal Sprite Sheet

```text
<Projectile: FireballSheet>
<Projectile Frames: 8>
<Projectile Animation FPS: 12>
```

Check:

```text
The frames are arranged horizontally.
Each frame is the same width.
The file is in img/projectiles/.
```

### Image Sequence

```text
<Projectile Images: Fire1;Fire2;Fire3;Fire4>
<Projectile Animation FPS: 12>
```

Check:

```text
img/projectiles/Fire1.png
img/projectiles/Fire2.png
img/projectiles/Fire3.png
img/projectiles/Fire4.png
```

---

## 29. Projectile Pool Always Uses the Same Image

### Random Pool

```text
<Projectile Pool: Fire1;Fire2;Fire3>
<Projectile Pool Mode: random>
```

Random results can repeat by chance.

Test several uses before assuming it is broken.

### Cycle Pool

```text
<Projectile Pool: Fire1;Fire2;Fire3>
<Projectile Pool Mode: cycle>
```

Use cycle when you want predictable image cycling.

---

## 30. Projectile Appears Behind Battlers

Set plugin parameter:

```text
Projectile Layer: aboveBattlers
```

If a custom battle plugin changes layers, test in a clean project.

---

## 31. Projectile Causes Lag

### Possible Causes

```text
Too many projectiles at once.
Too many afterimages.
Too many orbiters.
Large PNG files.
High animation FPS.
Very short airborne SE interval.
Many repeated waves.
```

### Fixes

Reduce afterimages:

```text
<Projectile Afterimage Interval: 8>
<Projectile Afterimage Duration: 10>
```

Reduce orbiters:

```text
<Projectile Orbiters: 0>
```

Reduce animation FPS:

```text
<Projectile Animation FPS: 8>
```

Reduce projectile count or wave count.

Use smaller PNGs when possible.

---

## 32. Works in Playtest, Fails in Deployment

### Check File Names

Deployment can be stricter about file paths and capitalization.

Make sure these match exactly:

```text
<Projectile: Fireball>
img/projectiles/Fireball.png
```

These are different on some systems:

```text
Fireball.png
fireball.png
FIREBALL.png
```

### Check Included Files

Confirm the deployed build includes:

```text
img/projectiles/
audio/se/
js/plugins/Okapi_ProjectileCore.js
```

### Recommended

Test the deployed build before uploading to Itch.io or another store.

---

## 33. Clean Project Test

When troubleshooting a serious bug, make a clean RPG Maker MV project and test only this:

```text
1. Add Okapi_ProjectileCore.js.
2. Turn plugin ON.
3. Add img/projectiles/Fireball.png.
4. Add this to one skill:

<Projectile: Fireball>
<Projectile Path: straight>
<Projectile Speed: 14>

5. Give the skill to one actor.
6. Start a test battle.
```

If this works, the plugin basic setup is correct.

Then add your other plugins one at a time.

---

## 34. Minimum Test Skills

Use these to verify the major systems.

### Straight Test

```text
<Projectile: Fireball>
<Projectile Path: straight>
<Projectile Speed: 14>
```

### Arc Test

```text
<Projectile: Arrow>
<Projectile Path: arc>
<Projectile Arc: 90>
<Projectile Speed: 16>
```

### Mortar Test

```text
<Projectile: Boulder>
<Projectile Path: mortar>
<Projectile Launch Height: 160>
<Projectile Mortar Phase: 0.35>
<Projectile Face Path: true>
<Projectile Speed: 14>
```

### Boomerang Test

```text
<Projectile: Boomerang>
<Projectile Path: straight>
<Projectile Behavior: boomerang>
<Projectile Rotation: true>
<Projectile Rotation Speed: 0.35>
<Projectile Speed: 16>
```

### Stage 2 Test

```text
<Projectile: Fireball>
<Projectile Path: straight>
<Projectile Stage 2 Trigger: progress 50%>
<Projectile Stage 2 Image: Fireball>
<Projectile Stage 2 Count: 3>
<Projectile Stage 2 Spread: 20>
<Projectile Stage 2 Speed: 18>
<Projectile Stage 2 Damage: each>
```

---

## 35. Recommended Debug Order

Use this order when something breaks:

```text
1. Test a straight projectile.
2. Test the image path and filename.
3. Turn Strict Missing Projectile Image Check OFF.
4. Remove advanced tags.
5. Remove weapon/equipment projectile tags.
6. Test the skill with a non-weapon element.
7. Test in a clean project.
8. Add other plugins back one at a time.
9. Test deployed build.
```

This prevents chasing five possible problems at once.

---

## 36. What to Include in a Bug Report

When reporting a bug, include:

```text
RPG Maker MV version:
Okapi Projectile Core version:
Operating system:
Playtest or deployed build:
Side-view or front-view battle:
Other battle plugins:
Yanfly Battle Engine Core installed? yes/no:
Skill notetags:
Weapon notetags:
Actor/class/state/enemy notetags:
Projectile image filename:
Console error text:
Screenshots or GIF:
Steps to reproduce:
```

### Bug Report Template

```text
Title:
Short description:

RPG Maker MV version:
Okapi Projectile Core version:
Project type: clean project / main project
Battle view: side-view / front-view
Other plugins:

Skill note box:
[paste here]

Weapon/actor/class/state/enemy note boxes:
[paste here]

Projectile image files:
[paste filenames here]

Expected result:
[paste here]

Actual result:
[paste here]

Steps to reproduce:
1.
2.
3.

Console errors:
[paste here]
```

---

## 37. Best Practices to Avoid Problems

Use clean modern tags:

```text
<Projectile Path: arc>
<Projectile Behavior: boomerang>
```

Prefer these over old all-in-one Mode tags for new projects.

Put named skill behavior directly on the skill.

Use weapon tags mainly for normal attacks and weapon-element skills.

Keep projectile art facing right.

Keep Auto Flip ON.

Keep Strict Missing Projectile Image Check OFF unless you are deliberately testing missing files.

Test deployed builds before release.

Back up your project before changing plugins.

---

## 38. Quick Plain-English Fixes

```text
No projectile? Check image folder and filename.
Arc is straight? Put <Projectile Path: arc> directly on the skill.
Mortar is straight? Use Path, not Behavior.
Upside down? Use Auto Flip, Face Path, and Rotation false.
Wrong start spot? Use Start Offset.
Wrong hit spot? Use Hit Offset.
Weapon overrides skill? Avoid element -1 or use Projectile Override.
Stage 2 does not split? Use progress 50% and confirm Stage 2 image exists.
Too much lag? Reduce afterimages, orbiters, image size, and projectile count.
Works in editor but not export? Check capitalization and deployed files.
```
