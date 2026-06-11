# Okapi Projectile Core - Expanded Notetag Guide

_RPG Maker MV - v2.20 - End-User Documentation_

**Okapi Interactive / McKnight Coding**

**What this guide is for**

Use this as the full notetag reference for Okapi Projectile Core. It explains what each major tag does, where to place tags, which tags are best for common skill setups, and how to avoid common setup problems.

## Contents

- 1. Fast rules for users
- 2. Where notetags can go
- 3. Basic projectile tags
- 4. Path tags
- 5. Behavior tags
- 6. Movement tuning tags
- 7. Positioning, offsets, and back attacks
- 8. Facing, flipping, rotation, and scale
- 9. Sound, timing, and repeat waves
- 10. Stage 2 projectiles
- 11. Pools, animation, trails, glow, and orbiters
- 12. Weapon-element priority and named-skill safety
- 13. Projectile override blocks
- 14. Copy-paste recipes
- 15. Troubleshooting
- 16. Full quick reference tables

## 1. Fast rules for users

- Put projectile PNGs in img/projectiles/ unless the plugin parameter uses a different folder.
- Use the image name without .png in the notetag. Example: `<Projectile: Fireball>`.
- For new skills, prefer `<Projectile Path: ...>` and `<Projectile Behavior: ...>` instead of older `<Projectile Mode: ...>` tags.
- Draw most projectile art facing right, then keep Auto Flip enabled.
- Put named skill behavior directly on the skill note box so the skill owns its path, timing, stages, and behavior.
- Use weapon and equipment projectile tags mainly for Attack or weapon-element skills.
- Keep Strict Missing Projectile Image Check OFF for normal projects unless you specifically want strict file validation.

**Best first test skill:**

```text
<Projectile: Fireball>
<Projectile Path: straight>
<Projectile Speed: 14>
```

**Recommended tag style**

Use the clean Path/Behavior tags for new skills. Older Mode tags still work, but Path + Behavior is easier to read and maintain.

## 2. Where notetags can go

The plugin can read projectile notes from multiple RPG Maker MV database objects. Where you place the notetag controls how broadly the rule applies.

| Tag / Value | Use it for | Example / Notes |
| --- | --- | --- |
| Skill / Item | Best place for named attacks. | `<Projectile: Fireball>` |
| Weapon | Best for Attack or weapon-element skills. | `<Projectile: Arrow>` |
| Armor | Can modify projectile behavior or scale. | `<Projectile Scale Multiplier: 0.8>` |
| Actor / Class | Good for character-wide projectile style. | `<Projectile Override>` ... |
| Enemy | Good for enemy Attack projectiles. | `<Projectile: sludge_ball>` |
| State | Best for temporary changes and overrides. | `<Projectile Override>` ... |

**Best practice**

Put the complete projectile behavior on the skill itself for named skills. Use weapons, actors, classes, enemies, and states for Attack, weapon-element skills, modifiers, or override blocks.

## 3. Basic projectile tags

These are the tags most users should learn first.

| Tag / Value | Use it for | Example / Notes |
| --- | --- | --- |
| `<Projectile: name>` | Main projectile image. | `<Projectile: Fireball>` |
| `<Projectile Image: name>` | Alias for a single image or image list. | `<Projectile Image: Fireball>` |
| `<Projectile Images: A;B;C>` | Separate PNG image sequence. | `<Projectile Images: Fire1;Fire2;Fire3>` |
| `<Projectile Speed: n>` | Travel speed. | `<Projectile Speed: 14>` |
| `<Projectile FPS: n>` | Timing basis. Default 60. | `<Projectile FPS: 60>` |
| `<Projectile Scale: n>` | Base image scale. | `<Projectile Scale: 1.25>` |
| `<Projectile Scale: n%>` | Percent scale format. | `<Projectile Scale: 125%>` |

**Full basic template:**

```text
<Projectile: Fireball>
<Projectile Path: straight>
<Projectile Speed: 14>
<Projectile FPS: 60>
<Projectile Start: frontMiddle>
<Projectile Hit: frontMiddle>
<Projectile Scale: 1.0>
<Projectile Image Facing: right>
<Projectile Auto Flip: true>
```

## 4. Path tags

Path controls how the projectile travels. These are the clean modern tags to use in new projects.

| Tag / Value | Use it for | Example / Notes |
| --- | --- | --- |
| straight | Direct travel from user to target. | `<Projectile Path: straight>` |
| arc | Curved shot. Good for arrows and throws. | `<Projectile Path: arc>` |
| mortar | Launches upward, then turns toward target. | `<Projectile Path: mortar>` |
| rolling | Ground-style rolling movement. | `<Projectile Path: rolling>` |
| bounce | Bouncing-ball movement. | `<Projectile Path: bounce>` |
| homing | Smooth seeker-style travel. | `<Projectile Path: homing>` |
| sine | Wave / oscillation travel. | `<Projectile Path: sine>` |
| zigzag | Sharp unstable movement. | `<Projectile Path: zigzag>` |
| helix | Double-wave helix travel. | `<Projectile Path: helix>` |
| wild | Unstable chaotic drift. | `<Projectile Path: wild>` |

**Arc arrow example:**

```text
<Projectile: Arrow>
<Projectile Path: arc>
<Projectile Arc: 72>
<Projectile Speed: 18>
```

**Mortar blast example:**

```text
<Projectile: Boulder>
<Projectile Path: mortar>
<Projectile Launch Height: 160>
<Projectile Mortar Phase: 0.35>
<Projectile Face Path: true>
```

**Legacy compatibility**

Old `<Projectile Mode: ...>` tags still work. For new skills, write `<Projectile Path: ...>` when the value is a movement path.

## 5. Behavior tags

Behavior controls special behavior added to the path. This is separate from the movement shape.

| Tag / Value | Use it for | Example / Notes |
| --- | --- | --- |
| normal | Standard one-way projectile. | `<Projectile Behavior: normal>` |
| boomerang | Travels to target, applies effect, then returns. | `<Projectile Behavior: boomerang>` |
| chain | Hits and then jumps/chains to other valid targets. | `<Projectile Behavior: chain>` |
| orbit | Orbits before impact. | `<Projectile Behavior: orbit>` |

**Boomerang example:**

```text
<Projectile: Boomerang>
<Projectile Path: straight>
<Projectile Behavior: boomerang>
<Projectile Speed: 16>
<Projectile Rotation: true>
<Projectile Rotation Speed: 0.35>
```

**Chain example:**

```text
<Projectile: spark_blue>
<Projectile Path: straight>
<Projectile Behavior: chain>
<Projectile Chains: 3>
<Projectile Chain Range: 9999>
<Projectile Chain Repeat: false>
<Projectile Chain Target: nearest>
```

## 6. Movement tuning tags

Use these tags to tune specific path types.

| Tag / Value | Use it for | Example / Notes |
| --- | --- | --- |
| `<Projectile Arc: n>` | Arc height for arc shots. | `<Projectile Arc: 96>` |
| `<Projectile Force Arc: true>` | Forces arc display when needed. | `<Projectile Force Arc: true>` |
| `<Projectile Launch Height: n>` | Mortar / vertical launch height. | `<Projectile Launch Height: 160>` |
| `<Projectile Mortar Height: n>` | Alias-style mortar height tag. | `<Projectile Mortar Height: 160>` |
| `<Projectile Mortar Phase: n>` | How long the upward phase lasts. | `<Projectile Mortar Phase: 0.35>` |
| `<Projectile Homing Strength: n>` | How strongly homing turns. | `<Projectile Homing Strength: 0.35>` |
| `<Projectile Turn Rate: n>` | Alias for homing turn strength. | `<Projectile Turn Rate: 0.35>` |
| `<Projectile Sine Amplitude: n>` | Wave height for sine path. | `<Projectile Sine Amplitude: 32>` |
| `<Projectile Sine Frequency: n>` | Wave frequency for sine path. | `<Projectile Sine Frequency: 3>` |
| `<Projectile Zigzag Width: n>` | Width of zigzag movement. | `<Projectile Zigzag Width: 40>` |
| `<Projectile Zigzag Segments: n>` | Number of zigzag segments. | `<Projectile Zigzag Segments: 6>` |

| Tag / Value | Use it for | Example / Notes |
| --- | --- | --- |
| `<Projectile Spiral Radius: n>` | Radius for spiral-style path. | `<Projectile Spiral Radius: 36>` |
| `<Projectile Spiral Turns: n>` | Number of spiral turns. | `<Projectile Spiral Turns: 3>` |
| `<Projectile Helix Radius: n>` | Radius for helix path. | `<Projectile Helix Radius: 28>` |
| `<Projectile Helix Turns: n>` | Number of helix turns. | `<Projectile Helix Turns: 3>` |
| `<Projectile Orbit Radius: n>` | Orbit distance around target. | `<Projectile Orbit Radius: 64>` |
| `<Projectile Orbit Time: n>` | Frames spent orbiting. | `<Projectile Orbit Time: 45>` |
| `<Projectile Orbit Turns: n>` | Number of orbit turns. | `<Projectile Orbit Turns: 1>` |
| `<Projectile Wildness: n>` | Amount of chaotic drift. | `<Projectile Wildness: 32>` |
| `<Projectile Wild Frequency: n>` | Frequency of wild drift. | `<Projectile Wild Frequency: 5>` |
| `<Projectile Wild Interval: n>` | Interval for wild updates. | `<Projectile Wild Interval: 3>` |

**Zigzag lightning:**

```text
<Projectile: lightning_bolt>
<Projectile Path: zigzag>
<Projectile Zigzag Width: 40>
<Projectile Zigzag Segments: 6>
<Projectile Speed: 16>
```

**Sine wave attack:**

```text
<Projectile: wave_blue>
<Projectile Path: sine>
<Projectile Sine Amplitude: 32>
<Projectile Sine Frequency: 3>
<Projectile Speed: 14>
```

## 7. Positioning, offsets, and back attacks

Positioning controls where the projectile starts from and where it visually hits the target.

| Tag / Value | Use it for | Example / Notes |
| --- | --- | --- |
| frontTop | Front upper anchor. | `<Projectile Start: frontTop>` |
| frontMiddle | Front middle anchor. Default. | `<Projectile Start: frontMiddle>` |
| frontBottom | Front lower anchor. Good for rolling. | `<Projectile Hit: frontBottom>` |
| backTop | Back upper anchor. | `<Projectile Hit: backTop>` |
| backMiddle | Back middle anchor. | `<Projectile Hit: backMiddle>` |
| backBottom | Back lower anchor. | `<Projectile Hit: backBottom>` |
| centerTop | Center upper anchor. | `<Projectile Hit: centerTop>` |
| centerMiddle | Center middle anchor. | `<Projectile Hit: centerMiddle>` |
| centerBottom | Center lower anchor. | `<Projectile Hit: centerBottom>` |

| Tag / Value | Use it for | Example / Notes |
| --- | --- | --- |
| `<Projectile Start Offset X: n>` | Move launch point left/right. | `<Projectile Start Offset X: 20>` |
| `<Projectile Start Offset Y: n>` | Move launch point up/down. | `<Projectile Start Offset Y: -20>` |
| `<Projectile Hit Offset X: n>` | Move hit point left/right. | `<Projectile Hit Offset X: 10>` |
| `<Projectile Hit Offset Y: n>` | Move hit point up/down. | `<Projectile Hit Offset Y: 20>` |
| `<Projectile Back Attack: true>` | Makes a back-hit style attack. | `<Projectile Back Attack: true>` |
| `<Projectile Back Arc Height: n>` | Back attack loop arc height. | `<Projectile Back Arc Height: 120>` |
| `<Projectile Back Overshoot: n>` | How far it overshoots target. | `<Projectile Back Overshoot: 72>` |

**Back-hit example:**

```text
<Projectile: shadow_blade>
<Projectile Path: arc>
<Projectile Hit: backMiddle>
<Projectile Back Attack: true>
<Projectile Back Arc Height: 120>
<Projectile Back Overshoot: 72>
```

## 8. Facing, flipping, rotation, and scale

| Tag / Value | Use it for | Example / Notes |
| --- | --- | --- |
| `<Projectile Image Facing: right>` | Use when art naturally faces right. | `<Projectile Image Facing: right>` |
| `<Projectile Image Facing: left>` | Use when art naturally faces left. | `<Projectile Image Facing: left>` |
| `<Projectile Auto Flip: true>` | Automatically flips by travel direction. | `<Projectile Auto Flip: true>` |
| `<Projectile Rotation: true>` | Spin the sprite. | `<Projectile Rotation: true>` |
| `<Projectile Rotation Speed: n>` | Spin speed. | `<Projectile Rotation Speed: 0.20>` |
| `<Projectile Face Path: true>` | Face along travel path. | `<Projectile Face Path: true>` |
| `<Projectile Path Rotation: true>` | Alias-style path-facing tag. | `<Projectile Path Rotation: true>` |
| `<Projectile Spin With Path: true>` | Spin while path-facing. | `<Projectile Spin With Path: true>` |
| `<Projectile Scale: n>` | Base image scale. | `<Projectile Scale: 1.25>` |
| `<Projectile Scale Multiplier: n>` | Multiplies final scale. | `<Projectile Scale Multiplier: 1.25>` |
| `<Projectile Scale Add: n>` | Adds to scale calculation. | `<Projectile Scale Add: 0.25>` |
| `<Projectile Scale Bonus: n%>` | Percent scale bonus. | `<Projectile Scale Bonus: 25%>` |

**Recommended arrow / beam facing setup:**

```text
<Projectile Image Facing: right>
<Projectile Auto Flip: true>
<Projectile Face Path: true>
<Projectile Rotation: false>
```

**Recommended spinning boulder setup:**

```text
<Projectile: Boulder>
<Projectile Path: rolling>
<Projectile Rotation: true>
<Projectile Rotation Speed: 0.30>
```

## 9. Sound, timing, and repeat waves

| Tag / Value | Use it for | Example / Notes |
| --- | --- | --- |
| `<Projectile Launch SE: name>` | Sound at launch. | `<Projectile Launch SE: Bow1>` |
| `<Projectile Launch SE Volume: n>` | Launch volume, 0-100. | `<Projectile Launch SE Volume: 90>` |
| `<Projectile Launch SE Pitch: n>` | Launch pitch, 50-150. | `<Projectile Launch SE Pitch: 100>` |
| `<Projectile Launch SE Pan: n>` | Launch pan, -100 to 100. | `<Projectile Launch SE Pan: 0>` |
| `<Projectile Airborne SE: name>` | Repeating travel sound. | `<Projectile Airborne SE: Wind4>` |
| `<Projectile Airborne SE Interval: n>` | Frames between airborne SE. | `<Projectile Airborne SE Interval: 12>` |
| `<Projectile Timing: simultaneous>` | All target shots launch together. | `<Projectile Timing: simultaneous>` |
| `<Projectile Timing: sequential>` | Targets are handled one-by-one. | `<Projectile Timing: sequential>` |
| `<Projectile Waves: n>` | Number of repeated waves. | `<Projectile Waves: 2>` |
| `<Projectile Wave Delay: n>` | Frames between waves. | `<Projectile Wave Delay: 120>` |
| `<Projectile Wave Delay Seconds: n>` | Delay in seconds. | `<Projectile Wave Delay Seconds: 2>` |

**Bow launch sound:**

```text
<Projectile Launch SE: Bow1>
<Projectile Launch SE Volume: 90>
<Projectile Launch SE Pitch: 100>
<Projectile Launch SE Pan: 0>
```

**Three projectile waves:**

```text
<Projectile: arrow_yellow>
<Projectile Path: arc>
<Projectile Arc: 72>
<Projectile Waves: 3>
<Projectile Wave Delay: 45>
```

**Usage advice**

Airborne sounds can get noisy when many projectiles are on screen. Use them for slow, dramatic, or boss projectiles rather than every basic arrow.

## 10. Stage 2 projectiles

Stage 2 tags let a projectile transform, split, or change movement mid-flight before the final impact.

| Tag / Value | Use it for | Example / Notes |
| --- | --- | --- |
| `<Projectile Stage 2 Trigger: progress 60%>` | Trigger when travel progress reaches a percent. | `<Projectile Stage 2 Trigger: progress 60%>` |
| `<Projectile Stage 2 Trigger: time 30>` | Trigger after a frame count. | `<Projectile Stage 2 Trigger: time 30>` |
| `<Projectile Stage 2 Trigger: distance 160>` | Trigger after traveling a distance. | `<Projectile Stage 2 Trigger: distance 160>` |
| `<Projectile Stage 2 Image: name>` | Image used after transform. | `<Projectile Stage 2 Image: beam_green>` |
| `<Projectile Stage 2 Images: A;B;C>` | Image sequence after transform. | `<Projectile Stage 2 Images: A;B;C>` |
| `<Projectile Stage 2 Path: path>` | Stage 2 path. | `<Projectile Stage 2 Path: homing>` |
| `<Projectile Stage 2 Behavior: value>` | Stage 2 behavior. | `<Projectile Stage 2 Behavior: normal>` |
| `<Projectile Stage 2 Count: n>` | Number of split projectiles. | `<Projectile Stage 2 Count: 3>` |
| `<Projectile Stage 2 Spread: n>` | Spread angle / spacing. | `<Projectile Stage 2 Spread: 35>` |
| `<Projectile Stage 2 Speed: n>` | Speed after transform. | `<Projectile Stage 2 Speed: 18>` |
| `<Projectile Stage 2 Damage: each>` | Each split projectile can damage. | `<Projectile Stage 2 Damage: each>` |
| `<Projectile Stage 2 Damage: once>` | Only one final damage result. | `<Projectile Stage 2 Damage: once>` |
| `<Projectile Stage 2 Targeting: original>` | Current supported targeting style. | `<Projectile Stage 2 Targeting: original>` |

**Split burst example:**

```text
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

**Stage 2 design note**

Use Damage: each for true multi-hit split attacks. Use Damage: once if the split is mostly visual and should not multiply damage.

## 11. Pools, animation, trails, glow, and orbiters

| Tag / Value | Use it for | Example / Notes |
| --- | --- | --- |
| `<Projectile Pool: A;B;C>` | Projectile image pool. | `<Projectile Pool: Fire1;Fire2;Fire3>` |
| `<Projectile Pool Mode: random>` | Randomly choose one image. | `<Projectile Pool Mode: random>` |
| `<Projectile Pool Mode: cycle>` | Cycle through image list. | `<Projectile Pool Mode: cycle>` |
| `<Projectile Frames: n>` | Horizontal sheet frame count. | `<Projectile Frames: 8>` |
| `<Projectile Animation FPS: n>` | Animation speed. | `<Projectile Animation FPS: 12>` |
| `<Projectile Anim FPS: n>` | Short alias. | `<Projectile Anim FPS: 12>` |
| `<Projectile Afterimage: true>` | Enable afterimages. | `<Projectile Afterimage: true>` |
| `<Projectile Trail: true>` | Trail alias. | `<Projectile Trail: true>` |
| `<Projectile Afterimage Interval: n>` | Frames between afterimages. | `<Projectile Afterimage Interval: 4>` |
| `<Projectile Afterimage Duration: n>` | How long afterimages stay. | `<Projectile Afterimage Duration: 18>` |
| `<Projectile Glow: true>` | Additive glow effect. | `<Projectile Glow: true>` |
| `<Projectile Orbiters: n>` | Number of orbiting sub-projectiles. | `<Projectile Orbiters: 3>` |
| `<Projectile Orbiter: name>` | Orbiter image. | `<Projectile Orbiter: Spark>` |
| `<Projectile Orbiter Radius: n>` | Orbiter radius. | `<Projectile Orbiter Radius: 32>` |
| `<Projectile Orbiter Speed: n>` | Orbiter speed. | `<Projectile Orbiter Speed: 0.15>` |

**Random rock toss:**

```text
<Projectile Pool: rock_small;rock_medium;rock_large>
<Projectile Pool Mode: random>
<Projectile Path: arc>
<Projectile Arc: 80>
<Projectile Speed: 14>
```

**Animated fireball sheet:**

```text
<Projectile: FireballSheet>
<Projectile Frames: 8>
<Projectile Animation FPS: 12>
```

**Glow and trail:**

```text
<Projectile: holy_bolt>
<Projectile Path: straight>
<Projectile Trail: true>
<Projectile Afterimage Interval: 4>
<Projectile Afterimage Duration: 18>
<Projectile Glow: true>
```

## 12. Weapon-element priority and named-skill safety

Weapon-element means the skill/item uses RPG Maker MV damage element -1, often called Normal Attack or Weapon element. For those actions, the plugin can look at battler-related note boxes.

**Priority for weapon-element actions**

Highest to lowest: states on the subject, actor primary weapon, actor other weapons, actor armors, enemy notes, actor notes, actor class notes, then skill/item notes.

**Example bow weapon note:**

```text
<Projectile: arrow_yellow>
<Projectile Path: arc>
<Projectile Arc: 72>
<Projectile Speed: 18>
<Projectile Motion: missile>
```

For non-weapon-element named skills that have their own projectile notebox, v2.20 treats the skill as owning its movement path, hit point, timing, stages, and behavior tags. Raw weapon tags should not turn that named skill into a straight shot unless the weapon/equipment uses a matching Projectile Override block.

**Safe named skill example:**

```text
<Projectile: arrow_yellow>
<Projectile Path: arc>
<Projectile Arc: 90>
<Projectile Speed: 16>
```

**Practical rule**

For project setup, put the full path and behavior on every named skill. Use weapon note tags for Attack and weapon-element skills.

## 13. Projectile override blocks

Projectile Overrides are conditional blocks that layer projectile changes on top of the action being used. They can be placed on actors, classes, enemies, weapons, armors, states, skills, or items.

**Basic override block:**

```text
<Projectile Override>
```

Element: Cold

```text
Projectile: ice_shard
```

Path: arc Arc: 96 Trail: true

```text
</Projectile Override>
```

| Tag / Value | Use it for | Example / Notes |
| --- | --- | --- |
| Skill Type: 1 | Match by skill type ID. | Skill Type: 1 |
| Skill Type: Magic | Match by skill type name. | Skill Type: Magic |
| Element: Cold | Match by element name. | Element: Cold |
| Element ID: 3 | Match by element ID. | Element ID: 3 |
| Damage Type: Cold | Can be used as an element-style condition. | Damage Type: Cold |
| Hit Type: Physical | Match physical/magical/certain. | Hit Type: Physical |
| Skill ID: 91 | Match exact skill. | Skill ID: 91 |
| Weapon Type: Bow | Match equipped or required weapon type. | Weapon Type: Bow |
| Actor ID: 3 | Match actor. | Actor ID: 3 |
| Class ID: 7 | Match actor class. | Class ID: 7 |
| Enemy ID: 12 | Match enemy. | Enemy ID: 12 |
| State: 45 | Match active state. | State: 45 |

**Magic projectile override:**

```text
<Projectile Override>
```

Skill Type: Magic

```text
Projectile: mana_arrow
```

Motion: missile

```text
</Projectile Override>
```

**Cold element override:**

```text
<Projectile Override>
```

Element: Cold

```text
Projectile: ice_shard
```

Path: arc Arc: 96 Trail: true

```text
</Projectile Override>
```

**AND logic**

If an override block has multiple conditions, all of them must match. For example, Skill Type: Magic plus Element: Cold means the action must be both Magic and Cold.

## 14. Copy-paste recipes

### Straight Shot

**`<Projectile: beam_yellow>`**

```text
<Projectile Path: straight>
<Projectile Speed: 18>
```

### Arc Shot

**`<Projectile: arrow_yellow>`**

```text
<Projectile Path: arc>
<Projectile Arc: 90>
<Projectile Speed: 16>
<Projectile Start: frontMiddle>
<Projectile Hit: frontMiddle>
```

### Mortar Blast

**`<Projectile: blast_red>`**

```text
<Projectile Path: mortar>
<Projectile Launch Height: 160>
<Projectile Mortar Phase: 0.35>
<Projectile Face Path: true>
<Projectile Speed: 14>
```

### Homing Magic Missile

**`<Projectile: magic_missile>`**

```text
<Projectile Path: homing>
<Projectile Homing Strength: 0.35>
<Projectile Speed: 14>
<Projectile Glow: true>
```

### Rolling Boulder

**`<Projectile: Boulder>`**

```text
<Projectile Path: rolling>
<Projectile Speed: 10>
<Projectile Rotation: true>
<Projectile Rotation Speed: 0.30>
<Projectile Hit: frontBottom>
```

### Boomerang Toss

**`<Projectile: boomerang>`**

```text
<Projectile Path: straight>
<Projectile Behavior: boomerang>
<Projectile Speed: 16>
<Projectile Rotation: true>
<Projectile Rotation Speed: 0.35>
```

### Lightning Zigzag

**`<Projectile: lightning_bolt>`**

```text
<Projectile Path: zigzag>
<Projectile Zigzag Width: 40>
<Projectile Zigzag Segments: 6>
<Projectile Speed: 16>
<Projectile Glow: true>
```

### Split Burst

**`<Projectile: beam_yellow>`**

```text
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

## 15. Troubleshooting

### Projectile does not appear

- Make sure the PNG is inside img/projectiles/.
- Make sure the filename matches the notetag exactly except for .png.
- Make sure the plugin is ON in Plugin Manager.
- Keep Strict Missing Projectile Image Check set to false for normal project use.
- Open the developer console and look for missing projectile image warnings.

### Damage happens but no visible projectile

- The plugin may have safely resolved the action because the visual could not be created.
- Check the projectile folder parameter.
- Add a Missing Projectile Fallback image if you want a backup visual.
- Verify that your project includes img/projectiles/ and the PNG files.

### Arc or mortar looks straight

- Use `<Projectile Path: arc>` or `<Projectile Path: mortar>` on the skill itself.
- Give arc shots a visible `<Projectile Arc: 90>` or higher.
- For mortar, set `<Projectile Launch Height: 160>` and `<Projectile Mortar Phase: 0.35>`.
- Check whether the skill is using weapon element -1 and being influenced by equipment notes.

### Projectile is upside down

- Draw the projectile art facing right when possible.
- Use `<Projectile Image Facing: right>` and `<Projectile Auto Flip: true>`.
- For arrows and beams, use `<Projectile Face Path: true>` and `<Projectile Rotation: false>`.
- Do not use normal rotation on arrow art unless you want it to spin.

### Projectile starts in the wrong place

- Change `<Projectile Start: frontMiddle>` to frontTop, frontBottom, centerMiddle, etc.
- Use `<Projectile Start Offset X: 20>` to move the start point horizontally.
- Use `<Projectile Start Offset Y: -20>` to move the start point upward.

### Projectile hits too high or too low

- Change `<Projectile Hit: frontMiddle>` to centerMiddle or frontBottom.
- Use `<Projectile Hit Offset Y: 20>` to move the impact lower.
- Use `<Projectile Hit Offset Y: -20>` to move the impact higher.

### Too much noise or visual clutter

- Reduce Glow, Trail, Orbiters, and Airborne SE usage on basic attacks.
- Use visual extras mainly for boss attacks, specials, and rare skills.
- Keep common attacks simple so the important showcase skills stand out.

## 16. Full quick reference tables

This section groups the most useful tags into quick reference tables. Some aliases are included so older projects are easier to understand, but new projects should prefer the clean tags.

### Core and image tags

| Tag / Value | Use it for | Example / Notes |
| --- | --- | --- |
| `<Projectile: name>` | Main image. | `<Projectile: Fireball>` |
| `<Projectile Image: name>` | Single image alias. | `<Projectile Image: Fireball>` |
| `<Projectile Images: A;B;C>` | Image sequence. | `<Projectile Images: A;B;C>` |
| `<Projectile Pool: A;B;C>` | Image pool. | `<Projectile Pool: A;B;C>` |
| `<Projectile Pool Mode: random\|cycle>` | Pool behavior. | `<Projectile Pool Mode: random>` |
| `<Projectile Frames: n>` | Sheet frames. | `<Projectile Frames: 8>` |
| `<Projectile Animation FPS: n>` | Animation speed. | `<Projectile Animation FPS: 12>` |

### Path and behavior tags

| Tag / Value | Use it for | Example / Notes |
| --- | --- | --- |
| `<Projectile Path: straight>` | Direct shot. | Recommended clean tag. |
| `<Projectile Path: arc>` | Arcing shot. | Use Arc height. |
| `<Projectile Path: mortar>` | Launch upward first. | Use Launch Height. |
| `<Projectile Path: rolling>` | Ground rolling. | Use Rotation. |
| `<Projectile Path: bounce>` | Bouncing path. | Use Bounce tags. |
| `<Projectile Path: homing>` | Seeker movement. | Use Homing Strength. |
| `<Projectile Path: sine>` | Wave movement. | Use amplitude/frequency. |
| `<Projectile Path: zigzag>` | Lightning movement. | Use width/segments. |
| `<Projectile Path: helix>` | Helix movement. | Use radius/turns. |
| `<Projectile Path: wild>` | Chaotic drift. | Use wildness/frequency. |
| `<Projectile Behavior: boomerang>` | Return behavior. | Use with straight path. |
| `<Projectile Behavior: chain>` | Target chaining. | Use chain tags. |
| `<Projectile Behavior: orbit>` | Orbit behavior. | Use orbit tags. |

### Position and visual tags

| Tag / Value | Use it for | Example / Notes |
| --- | --- | --- |
| `<Projectile Start: point>` | Launch anchor. | `<Projectile Start: frontMiddle>` |
| `<Projectile Hit: point>` | Impact anchor. | `<Projectile Hit: frontMiddle>` |
| `<Projectile Start Offset X/Y: n>` | Move launch point. | `<Projectile Start Offset Y: -20>` |
| `<Projectile Hit Offset X/Y: n>` | Move impact point. | `<Projectile Hit Offset Y: 20>` |
| `<Projectile Image Facing: right\|left>` | Natural art direction. | `<Projectile Image Facing: right>` |
| `<Projectile Auto Flip: true>` | Flip by travel direction. | `<Projectile Auto Flip: true>` |
| `<Projectile Rotation: true>` | Spin projectile. | `<Projectile Rotation: true>` |
| `<Projectile Face Path: true>` | Face along path. | `<Projectile Face Path: true>` |
| `<Projectile Glow: true>` | Glow/additive visual. | `<Projectile Glow: true>` |
| `<Projectile Trail: true>` | Trail alias. | `<Projectile Trail: true>` |
| `<Projectile Afterimage: true>` | Afterimage visual. | `<Projectile Afterimage: true>` |

### Stage 2 tags

| Tag / Value | Use it for | Example / Notes |
| --- | --- | --- |
| `<Projectile Stage 2 Trigger: ...>` | When Stage 2 starts. | progress/time/distance |
| `<Projectile Stage 2 Image: name>` | Stage 2 image. | `<Projectile Stage 2 Image: beam>` |
| `<Projectile Stage 2 Path: path>` | Stage 2 path. | `<Projectile Stage 2 Path: homing>` |
| `<Projectile Stage 2 Count: n>` | Split count. | `<Projectile Stage 2 Count: 3>` |
| `<Projectile Stage 2 Spread: n>` | Split spread. | `<Projectile Stage 2 Spread: 35>` |
| `<Projectile Stage 2 Speed: n>` | Stage 2 speed. | `<Projectile Stage 2 Speed: 18>` |
| `<Projectile Stage 2 Damage: each\|once>` | Damage mode. | `<Projectile Stage 2 Damage: each>` |
| `<Projectile Stage 2 Targeting: original>` | Current targeting. | `<Projectile Stage 2 Targeting: original>` |

### Sound, timing, and waves

| Tag / Value | Use it for | Example / Notes |
| --- | --- | --- |
| `<Projectile Launch SE: name>` | Launch sound. | `<Projectile Launch SE: Bow1>` |
| `<Projectile Airborne SE: name>` | Travel sound. | `<Projectile Airborne SE: Wind4>` |
| `<Projectile Airborne SE Interval: n>` | Travel sound interval. | `<Projectile Airborne SE Interval: 12>` |
| `<Projectile Timing: simultaneous>` | Launch together. | Default timing. |
| `<Projectile Timing: sequential>` | Launch one by one. | Cinematic timing. |
| `<Projectile Waves: n>` | Repeated waves. | `<Projectile Waves: 3>` |
| `<Projectile Wave Delay: n>` | Delay in frames. | `<Projectile Wave Delay: 60>` |
| `<Projectile Wave Delay Seconds: n>` | Delay in seconds. | `<Projectile Wave Delay Seconds: 1>` |
