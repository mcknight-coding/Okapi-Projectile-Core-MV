# Okapi Projectile Core

**v2.20 · RPG Maker MV · by McKnight-Coding**

A self-contained battle projectile engine for RPG Maker MV. No Yanfly, Action Sequences, SV plugins, or other Okapi plugins required — just drop in the script and add images.

---

## Features

- Projectiles delay damage, animations, and battle log results until impact
- 10+ movement paths: straight, arc, mortar, homing, sine, zigzag, helix, orbit, bounce, wild, and more
- 4 behaviors: normal, boomerang, chain (ricochet), orbit
- Projectile Stages: mid-flight transform, split, or redirect at a trigger point
- Projectile Overrides: conditional visual/behavior swaps based on element, skill type, weapon type, actor, state, and more
- Weapon-element priority system: equippable items and states can define the projectile for Normal Attack skills
- Multi-target simultaneous and sequential timing modes
- Repeat wave support with configurable delay between volleys
- Animated projectiles via sprite sheets or image sequences
- Visual extras: glow, trail/afterimage, orbiting sub-projectiles
- Launch and airborne sound effects with volume, pitch, and pan control
- Missing image safety: warn in console, use a fallback image, or skip the visual without crashing
- Compatible with YEP Battle Engine Core and Action Sequences (optional — not required)

---

## Installation

1. Place `Okapi_ProjectileCore.js` in your project's `js/plugins/` folder.
2. Enable it in the RPG Maker MV Plugin Manager.
3. Create a folder for your projectile images: `img/projectiles/`
4. Drop your projectile PNG files there (e.g. `img/projectiles/Arrow.png`).

---

## Plugin Parameters

| Parameter | Default | Description |
|---|---|---|
| Projectile Folder | `projectiles/` | Subfolder under `img/` for projectile images |
| Missing Projectile Fallback | *(blank)* | Fallback image name if a projectile file is missing |
| Warn Missing Projectile Images | `true` | Log console warnings for missing images |
| Strict Missing Projectile Image Check | `false` | Skip visual on missing file (passive mode recommended) |
| Default FPS | `60` | Frame rate for projectile timing |
| Default Speed | `14` | Pixels per frame |
| Default Arc Height | `72` | Arc height for arc mode |
| Default Rotation Speed | `0.20` | Spin speed in radians per frame |
| Default Start Point | `frontMiddle` | Where projectiles originate on the user |
| Default Hit Point | `frontMiddle` | Where projectiles land on the target |
| Default Image Facing | `right` | Natural facing direction of your projectile art |
| Auto Flip To Travel Direction | `true` | Mirrors sprite based on travel direction |
| Projectile Layer | `aboveBattlers` | Render layer (`aboveBattlers` or `battlefield`) |
| Default Start/Hit Offset X/Y | `0` | Pixel offset adjustments for start and hit points |
| Default Launch SE / Airborne SE | *(blank)* | Default sound effects and their volume, pitch, pan settings |
| Projectile Repeat Wave Delay | `120` | Frames between repeat waves (120 = 2 sec at 60 FPS) |
| Projectile Strike Vertical Spacing | `14` | Vertical pixel gap between simultaneous strike projectiles |
| Replay User Attack Motion Each Projectile | `true` | Re-trigger throw/attack pose for each wave |
| Replay Damage Animation Each Strike | `true` | Re-trigger hit animation for each impact |
| Replay Damage Animation Delay | `0` | Frame delay between forced repeated hit animations |

---

## Quick Start

Add a notetag to any Skill, Item, Weapon, Armor, Actor, Enemy, or State:

```
<Projectile: Arrow>
```

That's it. Damage, animations, and popups are automatically delayed until impact.

---

## Common Notetags

### Core

```
<Projectile: Fireball>
<Projectile Speed: 14>
<Projectile Scale: 1.0>
<Projectile Mode: straight>
<Projectile Start: frontMiddle>
<Projectile Hit: frontMiddle>
<Projectile Rotation: true>
<Projectile Rotation Speed: 0.20>
<Projectile Image Facing: right>
<Projectile Auto Flip: true>
<Projectile Start Offset X: 0>
<Projectile Start Offset Y: 0>
<Projectile Hit Offset X: 0>
<Projectile Hit Offset Y: 0>
```

### Timing

```
<Projectile Timing: simultaneous>   (default)
<Projectile Timing: sequential>
```

### Repeat Waves

```
<Projectile Waves: 2>
<Projectile Wave Delay: 120>
<Projectile Wave Delay Seconds: 2>
```

### Sound Effects

```
<Projectile Launch SE: Bow1>
<Projectile Launch SE Volume: 90>
<Projectile Launch SE Pitch: 100>
<Projectile Launch SE Pan: 0>

<Projectile Airborne SE: Wind4>
<Projectile Airborne SE Volume: 70>
<Projectile Airborne SE Pitch: 100>
<Projectile Airborne SE Pan: 0>
<Projectile Airborne SE Interval: 12>
```

Launch SE plays once on creation. Airborne SE repeats every interval frames during flight.

### Scale Modifiers

Scale can be placed on Skills, Items, Actors, Classes, Enemies, Weapons, Armors, or States. On the projectile source it sets the base size; on other battler sources it multiplies the final size.

```
<Projectile Scale: 1.5>
<Projectile Scale Multiplier: 1.25>
<Projectile Scale Add: 0.25>
<Projectile Scale Bonus: 25%>
```

### Launch Motion (Sideview)

```
<Projectile Motion: weapon>
<Projectile Motion: missile>
<Projectile Motion: spell>
<Projectile Motion: throw>
<Projectile Motion: item>
```

Default behavior (v2.03+): physical skills use the actor's equipped weapon motion; magical skills use spell; item skills use item.

---

## Paths

Use `<Projectile Path: ...>` (new) or `<Projectile Mode: ...>` (legacy — still works).

| Path | Description | Extra Tags |
|---|---|---|
| `straight` | Direct linear flight | — |
| `arc` | Arcing trajectory | `<Projectile Arc: 96>`, `<Projectile Force Arc: true>` |
| `mortar` | Vertical launch then angled descent | `<Projectile Launch Height: 160>`, `<Projectile Mortar Phase: 0.35>` |
| `lob` | Smooth high arc (legacy) | `<Projectile Launch Height: 160>` |
| `homing` | Smooth curved seeker | `<Projectile Homing Strength: 0.35>` |
| `rolling` | Ground-hugging roll | Defaults to `frontBottom` hit point |
| `sine` | Wavy oscillation | `<Projectile Sine Amplitude: 32>`, `<Projectile Sine Frequency: 3>` |
| `zigzag` | Sharp lightning-style movement | `<Projectile Zigzag Width: 40>`, `<Projectile Zigzag Segments: 6>` |
| `spiral` | Spiraling path | `<Projectile Spiral Radius: 36>`, `<Projectile Spiral Turns: 3>` |
| `helix` | Double-wave helix | `<Projectile Helix Radius: 28>`, `<Projectile Helix Turns: 3>` |
| `orbit` | Orbits target before impact | `<Projectile Orbit Radius: 64>`, `<Projectile Orbit Time: 45>` |
| `bounce` | Bouncing-ball motion | — |
| `wild` | Chaotic drift | `<Projectile Wildness: 32>`, `<Projectile Wild Frequency: 5>` |

---

## Behaviors

```
<Projectile Behavior: normal>
<Projectile Behavior: boomerang>
<Projectile Behavior: chain>
<Projectile Behavior: orbit>
```

### Chain (formerly Ricochet/Bounce)

Hits the first target, then jumps to additional valid targets.

```
<Projectile Behavior: chain>
<Projectile Chains: 3>
<Projectile Chain Range: 9999>
<Projectile Chain Repeat: false>
<Projectile Chain Target: nearest>
```

Chain target options: `nearest`, `farthest`, `random`, `lowestHp`, `highestHp`

---

## Start & Hit Points

```
frontTop      frontMiddle      frontBottom
backTop       backMiddle       backBottom
centerTop     centerMiddle     centerBottom
```

### Back Attack Options (v2.04)

```
<Projectile Hit: backMiddle>
<Projectile Back Attack: true>
<Projectile Back Arc Height: 120>
<Projectile Back Overshoot: 72>
```

### Path-Facing (v2.04)

```
<Projectile Face Path: true>
<Projectile Spin With Path: true>
```

Mortar and back-hit paths enable path-facing automatically.

---

## Animated Projectiles

### Sprite Sheet

```
<Projectile: FireballSheet>
<Projectile Frames: 8>
<Projectile Animation FPS: 12>
```

### Image Sequence

```
<Projectile Images: Fire1;Fire2;Fire3;Fire4>
<Projectile Animation FPS: 12>
```

`<Projectile Image: A;B;C>` is an alias for `<Projectile Images: A;B;C>`.

### Projectile Pool

```
<Projectile Pool: WaterBall;FireBall;Spark>
<Projectile Pool Mode: random>

<Projectile Pool: WaterBall;FireBall;Spark>
<Projectile Pool Mode: cycle>
```

Random picks one image per launch. Cycle loops through the list (useful for tri-element effects).

---

## Projectile Stages (v2.05)

A projectile can transform, split, or change behavior mid-flight.

```
<Projectile: blast_green>
<Projectile Mode: mortar>
<Projectile Stage 2 Trigger: progress 60%>
<Projectile Stage 2 Image: beam_green>
<Projectile Stage 2 Mode: homing>
<Projectile Stage 2 Count: 3>
<Projectile Stage 2 Spread: 35>
<Projectile Stage 2 Speed: 18>
```

**Trigger options:**
```
<Projectile Stage 2 Trigger: progress 60%>
<Projectile Stage 2 Trigger: time 30>
<Projectile Stage 2 Trigger: distance 160>
```

**Damage behavior:**
```
<Projectile Stage 2 Damage: each>
<Projectile Stage 2 Damage: once>
```

---

## Projectile Overrides (v2.10)

Conditional rules that layer on top of the skill's base settings. Can be placed on Actors, Classes, Enemies, Weapons, Armors, States, Skills, or Items.

```
<Projectile Override>
Element: Cold
Projectile: ice_shard
Mode: arc
Arc: 96
Trail: true
</Projectile Override>
```

**Supported conditions:**
```
Skill Type: 1          Skill Type: Magic
Element: Cold          Element ID: 3
Damage Type: Cold      Hit Type: Physical
Skill ID: 91           Weapon Type: Bow
Actor ID: 3            Class ID: 7
Enemy ID: 12           State: 45
```

Multiple conditions use AND logic — all must match.

---

## Visual Extras

### Trail / Afterimage

```
<Projectile Trail: true>
<Projectile Afterimage: true>
<Projectile Afterimage Interval: 4>
<Projectile Afterimage Duration: 18>
```

### Glow (Additive Blend)

```
<Projectile Glow: true>
```

### Orbiting Sub-Projectiles

```
<Projectile Orbiters: 3>
<Projectile Orbiter: Spark>
<Projectile Orbiter Radius: 32>
<Projectile Orbiter Speed: 0.15>
```

---

## Weapon-Element Priority

When a skill's damage element is set to **-1 (Normal Attack / Weapon)**, the projectile image and settings can be inherited from the battler's equipment, states, or database entry rather than the skill itself.

**Priority order (highest first):**

1. States on the subject
2. Actor primary weapon (first equipped slot)
3. Actor other weapons (dual-wield offhand)
4. Actor armors
5. Enemy database notes
6. Actor database notes
7. Actor class notes
8. Skill / item notes

The skill note box still acts as the base behavior layer for trails, glow, stages, timing, and sounds. Higher-priority sources may override the projectile image and explicitly-tagged properties.

For non-weapon-element skills, settings come from the skill/item note box only.

---

## Examples

**Fireball**
```
<Projectile: Fireball>
<Projectile Mode: straight>
<Projectile Speed: 14>
<Projectile Rotation: true>
<Projectile Rotation Speed: 0.12>
```

**Bow Shot**
```
<Projectile: Arrow>
<Projectile Mode: arc>
<Projectile Arc: 72>
<Projectile Speed: 18>
```

**Boomerang**
```
<Projectile: Boomerang>
<Projectile Mode: boomerang>
<Projectile Speed: 16>
<Projectile Rotation: true>
<Projectile Rotation Speed: 0.35>
```

**Rolling Boulder**
```
<Projectile: Boulder>
<Projectile Mode: rolling>
<Projectile Speed: 10>
<Projectile Rotation: true>
<Projectile Rotation Speed: 0.30>
```

**Mortar with Path-Facing**
```
<Projectile: shell>
<Projectile Mode: mortar>
<Projectile Launch Height: 160>
<Projectile Face Path: true>
```

**Chain Lightning**
```
<Projectile: Lightning>
<Projectile Behavior: chain>
<Projectile Chains: 3>
<Projectile Chain Target: nearest>
```

---

## YEP Compatibility

Okapi Projectile Core works with **YEP_BattleEngineCore** and **YEP Action Sequences** when those plugins are present — but neither is required. Key compatibility behaviors:

- `BattleManager.isBusy` waits while projectiles are in flight, preventing action sequences from advancing before impact
- ACTION EFFECT interception routes projectile skills into the Okapi wave system instead of the default YEP target loop
- PERFORM ACTION is limited to one launch pose per projectile skill to prevent duplicate weapon motions
- Sequential timing (`<Projectile Timing: sequential>`) routes correctly through YEP's target action queue

---

## Missing Image Safety (v2.11+)

If a projectile image file is missing:

- A console warning is logged (if **Warn Missing Projectile Images** is `ON`)
- The **Missing Projectile Fallback** image is used if configured
- Otherwise, the visual is skipped and the action resolves safely — no crash

**Strict Missing Projectile Image Check** is `OFF` by default. Enabling it uses the filesystem to pre-check image files, but may produce false negatives depending on NW.js project path configuration. Passive mode is recommended.

---

## Terms of Use

Free to use and modify for personal or commercial RPG Maker MV projects.

---

## Credits

**Plugin:** Okapi Projectile Core  
**Author:** McKnight-Coding  
**GitHub:** https://github.com/mcknight-coding  
**Studio:** Okapi Interactive / Okapi Codeworks
