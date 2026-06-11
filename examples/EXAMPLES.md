# Okapi Projectile Core — Examples

**RPG Maker MV • Okapi Projectile Core v2.20**  
**Okapi Interactive / McKnight Coding**

This document gives copy-paste projectile examples for common skill, weapon, enemy, state, and demo setups.

Use these examples together with:

- `docs/QUICK_START.md`
- `docs/NOTETAG_GUIDE.md`
- `docs/FAQ.md`
- `docs/COMPATIBILITY.md`

---

## Before using these examples

Put projectile images in:

```text
img/projectiles/
```

Use the image name without `.png` in notetags.

Example file:

```text
img/projectiles/Fireball.png
```

Skill note:

```text
<Projectile: Fireball>
```

Recommended public-release plugin filename:

```text
js/plugins/Okapi_ProjectileCore.js
```

Recommended first plugin settings:

```text
Projectile Folder: projectiles/
Warn Missing Projectile Images: true
Strict Missing Projectile Image Check: false
Default FPS: 60
Default Speed: 14
Default Arc Height: 72
Default Image Facing: right
Auto Flip To Travel Direction: true
Projectile Layer: aboveBattlers
```

---

## 1. Basic skill examples

### Straight Fireball

Good for a simple first test skill.

**Suggested RPG Maker MV skill setup**

| Field | Value |
|---|---|
| Name | Fireball |
| Scope | 1 Enemy |
| Hit Type | Magical Attack |
| Damage Type | HP Damage |
| Formula | `a.mat * 2 - b.mdf` |
| Animation | Fire One |

**Skill note box**

```text
<Projectile: Fireball>
<Projectile Path: straight>
<Projectile Speed: 14>
```

---

### Fast Beam

Good for lasers, bullets, energy bolts, and quick shots.

```text
<Projectile: beam_yellow>
<Projectile Path: straight>
<Projectile Speed: 20>
<Projectile Face Path: true>
<Projectile Rotation: false>
```

---

### Arc Arrow

Good for bows, thrown rocks, thrown knives, and curved magic shots.

```text
<Projectile: Arrow>
<Projectile Path: arc>
<Projectile Arc: 72>
<Projectile Speed: 18>
<Projectile Start: frontMiddle>
<Projectile Hit: frontMiddle>
<Projectile Image Facing: right>
<Projectile Auto Flip: true>
```

---

### Mortar Boulder

Good for bombs, boulders, meteors, rising shots, and artillery-style attacks.

```text
<Projectile: Boulder>
<Projectile Path: mortar>
<Projectile Launch Height: 160>
<Projectile Mortar Phase: 0.35>
<Projectile Face Path: true>
<Projectile Speed: 14>
```

---

### Rolling Boulder

Good for ground attacks, barrels, rolling bombs, and rolling stones.

```text
<Projectile: Boulder>
<Projectile Path: rolling>
<Projectile Start: frontBottom>
<Projectile Hit: frontBottom>
<Projectile Speed: 10>
<Projectile Rotation: true>
<Projectile Rotation Speed: 0.30>
```

---

### Homing Magic Missile

Good for seeker shots and guided magic.

```text
<Projectile: magic_missile>
<Projectile Path: homing>
<Projectile Homing Strength: 0.35>
<Projectile Speed: 14>
<Projectile Face Path: true>
```

---

### Zigzag Lightning

Good for lightning, unstable magic, chaos bolts, and erratic shots.

```text
<Projectile: lightning_bolt>
<Projectile Path: zigzag>
<Projectile Zigzag Width: 40>
<Projectile Zigzag Segments: 6>
<Projectile Speed: 16>
<Projectile Glow: true>
```

---

### Sine Wave / Sonic Shot

Good for wave beams, sonic attacks, and floating magic.

```text
<Projectile: wave_blue>
<Projectile Path: sine>
<Projectile Sine Amplitude: 32>
<Projectile Sine Frequency: 3>
<Projectile Speed: 14>
<Projectile Face Path: true>
```

---

### Wild Chaos Bolt

Good for chaotic magic and unstable enemy attacks.

```text
<Projectile: chaos_bolt>
<Projectile Path: wild>
<Projectile Wildness: 32>
<Projectile Wild Frequency: 5>
<Projectile Wild Interval: 3>
<Projectile Speed: 14>
<Projectile Glow: true>
```

---

## 2. Behavior examples

Path controls how the projectile travels. Behavior controls special behavior added on top.

Use this style for new skills:

```text
<Projectile Path: straight>
<Projectile Behavior: normal>
```

Older `<Projectile Mode: ...>` tags still work, but the clean Path + Behavior style is easier for users to understand.

---

### Boomerang

Good for returning weapons.

```text
<Projectile: Boomerang>
<Projectile Path: straight>
<Projectile Behavior: boomerang>
<Projectile Speed: 16>
<Projectile Rotation: true>
<Projectile Rotation Speed: 0.35>
```

---

### Chain Lightning

Good for attacks that jump from one target to another.

```text
<Projectile: spark_blue>
<Projectile Path: straight>
<Projectile Behavior: chain>
<Projectile Chains: 3>
<Projectile Chain Range: 9999>
<Projectile Chain Repeat: false>
<Projectile Chain Target: nearest>
<Projectile Speed: 18>
<Projectile Glow: true>
```

Chain target choices:

```text
nearest
farthest
random
lowestHp
highestHp
```

---

### Orbit Before Impact

Good for magical or boss attacks that circle before hitting.

```text
<Projectile: orb_purple>
<Projectile Path: straight>
<Projectile Behavior: orbit>
<Projectile Orbit Radius: 64>
<Projectile Orbit Time: 45>
<Projectile Orbit Turns: 1>
<Projectile Speed: 14>
<Projectile Glow: true>
```

---

## 3. Multi-target and repeat examples

### Simultaneous Multi-target Volley

All projectiles launch at the same time.

```text
<Projectile: arrow_yellow>
<Projectile Path: arc>
<Projectile Arc: 72>
<Projectile Speed: 18>
<Projectile Timing: simultaneous>
```

Good for:

- AOE magic
- Multi-shot attacks
- Enemy group attacks

---

### Sequential Multi-target Shot

Targets are handled one by one.

```text
<Projectile: arrow_yellow>
<Projectile Path: arc>
<Projectile Arc: 72>
<Projectile Speed: 18>
<Projectile Timing: sequential>
```

Good for:

- Dramatic boss attacks
- One-target-at-a-time chains
- Skills where each hit should be easy to see

---

### Three-Wave Arrow Volley

Good for repeated arrows or boss barrages.

```text
<Projectile: arrow_yellow>
<Projectile Path: arc>
<Projectile Arc: 72>
<Projectile Speed: 18>
<Projectile Waves: 3>
<Projectile Wave Delay: 45>
```

Alternative seconds format:

```text
<Projectile Waves: 3>
<Projectile Wave Delay Seconds: 1>
```

---

## 4. Sound examples

### Bow Launch Sound

```text
<Projectile Launch SE: Bow1>
<Projectile Launch SE Volume: 90>
<Projectile Launch SE Pitch: 100>
<Projectile Launch SE Pan: 0>
```

---

### Airborne Wind Sound

Use airborne sounds carefully. They can become noisy when many projectiles are on screen.

```text
<Projectile Airborne SE: Wind4>
<Projectile Airborne SE Volume: 70>
<Projectile Airborne SE Pitch: 100>
<Projectile Airborne SE Pan: 0>
<Projectile Airborne SE Interval: 12>
```

---

### Complete Arrow With Sound

```text
<Projectile: Arrow>
<Projectile Path: arc>
<Projectile Arc: 72>
<Projectile Speed: 18>
<Projectile Launch SE: Bow1>
<Projectile Launch SE Volume: 90>
<Projectile Launch SE Pitch: 100>
<Projectile Launch SE Pan: 0>
```

---

## 5. Positioning examples

### Common Front-to-Front Shot

```text
<Projectile Start: frontMiddle>
<Projectile Hit: frontMiddle>
```

---

### Low Rolling Shot

```text
<Projectile Start: frontBottom>
<Projectile Hit: frontBottom>
```

---

### Start Higher

Good for bows, staff casting, and flying enemies.

```text
<Projectile Start Offset Y: -20>
```

---

### Hit Lower

Good for ground explosions and low enemy hits.

```text
<Projectile Hit Offset Y: 20>
```

---

### Back-hit Shadow Blade

Good for assassin, teleport, or trick-shot attacks.

```text
<Projectile: shadow_blade>
<Projectile Path: arc>
<Projectile Hit: backMiddle>
<Projectile Back Attack: true>
<Projectile Back Arc Height: 120>
<Projectile Back Overshoot: 72>
<Projectile Face Path: true>
```

---

## 6. Facing, flipping, and rotation examples

### Recommended Arrow / Beam Setup

Use this when the art naturally points right.

```text
<Projectile Image Facing: right>
<Projectile Auto Flip: true>
<Projectile Face Path: true>
<Projectile Rotation: false>
```

---

### Left-facing Art Setup

Use this only when the projectile art naturally points left.

```text
<Projectile Image Facing: left>
<Projectile Auto Flip: true>
```

---

### Spinning Disc

```text
<Projectile: energy_disc>
<Projectile Path: straight>
<Projectile Rotation: true>
<Projectile Rotation Speed: 0.25>
<Projectile Speed: 16>
```

---

### Spinning Boulder

```text
<Projectile: Boulder>
<Projectile Path: rolling>
<Projectile Rotation: true>
<Projectile Rotation Speed: 0.30>
<Projectile Speed: 10>
```

---

## 7. Stage 2 examples

Stage 2 lets a projectile transform, split, or change movement mid-flight.

---

### Split Burst

A green blast travels first, then splits into three beam projectiles.

```text
<Projectile: blast_green>
<Projectile Path: mortar>
<Projectile Launch Height: 140>
<Projectile Stage 2 Trigger: progress 60%>
<Projectile Stage 2 Image: beam_green>
<Projectile Stage 2 Path: homing>
<Projectile Stage 2 Count: 3>
<Projectile Stage 2 Spread: 35>
<Projectile Stage 2 Speed: 18>
<Projectile Stage 2 Damage: each>
```

---

### Transforming Fireball

A fireball becomes an explosion shot before impact.

```text
<Projectile: Fireball>
<Projectile Path: straight>
<Projectile Speed: 14>
<Projectile Stage 2 Trigger: progress 70%>
<Projectile Stage 2 Image: fire_burst>
<Projectile Stage 2 Path: straight>
<Projectile Stage 2 Speed: 10>
<Projectile Stage 2 Damage: once>
```

---

### Timed Arc Split

A projectile splits after a set number of frames.

```text
<Projectile: seed_green>
<Projectile Path: arc>
<Projectile Arc: 96>
<Projectile Speed: 12>
<Projectile Stage 2 Trigger: time 30>
<Projectile Stage 2 Image: petal_shard>
<Projectile Stage 2 Path: straight>
<Projectile Stage 2 Count: 5>
<Projectile Stage 2 Spread: 50>
<Projectile Stage 2 Speed: 18>
```

---

### Distance-based Split

A projectile changes after traveling a certain distance.

```text
<Projectile: rock_chunk>
<Projectile Path: straight>
<Projectile Speed: 12>
<Projectile Stage 2 Trigger: distance 160>
<Projectile Stage 2 Image: rock_shard>
<Projectile Stage 2 Path: zigzag>
<Projectile Stage 2 Count: 3>
<Projectile Stage 2 Spread: 30>
<Projectile Stage 2 Speed: 16>
```

---

## 8. Image pool and animation examples

### Random Projectile Pool

Picks one image randomly when the skill is used.

```text
<Projectile Pool: Fire1;Fire2;Fire3>
<Projectile Pool Mode: random>
<Projectile Path: straight>
<Projectile Speed: 14>
```

---

### Cycling Projectile Images

Good for simple frame-by-frame projectile animation using separate PNG files.

```text
<Projectile Pool: Water1;Water2;Water3;Water4>
<Projectile Pool Mode: cycle>
<Projectile Animation FPS: 12>
<Projectile Path: straight>
<Projectile Speed: 14>
```

---

### Image Sequence Animation

```text
<Projectile Images: Fire1;Fire2;Fire3;Fire4>
<Projectile Animation FPS: 12>
<Projectile Path: straight>
<Projectile Speed: 14>
```

---

### Horizontal Sprite Sheet Animation

Use when one PNG contains multiple horizontal projectile frames.

```text
<Projectile: FireballSheet>
<Projectile Frames: 8>
<Projectile Animation FPS: 12>
<Projectile Path: straight>
<Projectile Speed: 14>
```

---

## 9. Visual polish examples

### Afterimage / Trail

```text
<Projectile: slash_blue>
<Projectile Path: straight>
<Projectile Speed: 18>
<Projectile Afterimage: true>
<Projectile Afterimage Interval: 4>
<Projectile Afterimage Duration: 18>
```

Alias style also works:

```text
<Projectile Trail: true>
<Projectile Trail Interval: 4>
<Projectile Trail Duration: 18>
```

---

### Glow

```text
<Projectile: orb_blue>
<Projectile Path: straight>
<Projectile Speed: 14>
<Projectile Glow: true>
```

---

### Orbiting Sub-projectiles

Good for magical projectiles with sparks or small satellites around them.

```text
<Projectile: orb_purple>
<Projectile Path: straight>
<Projectile Speed: 14>
<Projectile Orbiters: 3>
<Projectile Orbiter: Spark>
<Projectile Orbiter Radius: 32>
<Projectile Orbiter Speed: 0.15>
<Projectile Glow: true>
```

---

## 10. Weapon, enemy, state, and equipment examples

## Named skill safety rule

For named skills, put the full projectile behavior on the skill note box.

Use raw weapon and equipment projectile tags mainly for:

- Attack
- weapon-element skills
- broad equipment-based behavior

For special named skills, use `<Projectile Override>` blocks if equipment should change the projectile conditionally.

---

### Weapon Attack Projectile

Put this on a bow weapon note box.

```text
<Projectile: Arrow>
<Projectile Path: arc>
<Projectile Arc: 72>
<Projectile Speed: 18>
<Projectile Motion: missile>
```

Use this for the default Attack command or skills that use the weapon element.

---

### Enemy Sludge Attack

Put this on an enemy note box.

```text
<Projectile: sludge_ball>
<Projectile Path: arc>
<Projectile Arc: 64>
<Projectile Speed: 12>
<Projectile Rotation: true>
<Projectile Rotation Speed: 0.15>
```

---

### Enemy Boss Mortar Attack

Put this on a named enemy skill note box.

```text
<Projectile: boss_boulder>
<Projectile Path: mortar>
<Projectile Launch Height: 190>
<Projectile Mortar Phase: 0.35>
<Projectile Face Path: true>
<Projectile Speed: 12>
<Projectile Launch SE: Earth4>
<Projectile Timing: sequential>
```

---

### State Projectile Scale Buff

Put this on a state note box to make projectile visuals larger while the state is active.

```text
<Projectile Scale Bonus: 25%>
```

---

### Armor Projectile Scale Modifier

Put this on armor if you want that armor to reduce projectile size.

```text
<Projectile Scale Multiplier: 0.8>
```

---

## 11. Projectile override examples

Projectile Override blocks are conditional projectile rules. They can be placed on actors, classes, enemies, weapons, armors, states, skills, or items.

---

### Magic Skill Type Override

Use this on an actor, class, weapon, armor, or state note box.

```text
<Projectile Override>
Skill Type: Magic
Projectile: mana_arrow
Path: straight
Speed: 16
Motion: spell
Glow: true
</Projectile Override>
```

---

### Fire Element Override

```text
<Projectile Override>
Element: Fire
Projectile: Fireball
Path: straight
Speed: 14
Glow: true
Launch SE: Fire2
</Projectile Override>
```

---

### Cold Element Override

```text
<Projectile Override>
Element: Cold
Projectile: ice_shard
Path: arc
Arc: 96
Speed: 16
Trail: true
</Projectile Override>
```

---

### Bow Weapon Type Override

```text
<Projectile Override>
Weapon Type: Bow
Projectile: Arrow
Path: arc
Arc: 72
Speed: 18
Motion: missile
Launch SE: Bow1
</Projectile Override>
```

---

### Specific Skill Override

```text
<Projectile Override>
Skill ID: 91
Projectile: shadow_blade
Path: arc
Hit: backMiddle
Back Attack: true
Back Arc Height: 120
Back Overshoot: 72
Speed: 18
</Projectile Override>
```

---

### State-based Override

Only applies when the subject has the named state.

```text
<Projectile Override>
State: Enraged
Projectile: fire_bolt_red
Path: straight
Speed: 20
Scale: 1.25
Glow: true
</Projectile Override>
```

---

## 12. Demo-ready skill pack examples

These are simple skills you can include in a demo project to show the plugin clearly.

---

### Skill 1 — Fireball

| Field | Value |
|---|---|
| Scope | 1 Enemy |
| Hit Type | Magical Attack |
| Damage Type | HP Damage |
| Formula | `a.mat * 2 - b.mdf` |

```text
<Projectile: Fireball>
<Projectile Path: straight>
<Projectile Speed: 14>
<Projectile Glow: true>
```

---

### Skill 2 — Arc Shot

| Field | Value |
|---|---|
| Scope | 1 Enemy |
| Hit Type | Physical Attack |
| Damage Type | HP Damage |
| Formula | `a.atk * 2 - b.def` |

```text
<Projectile: Arrow>
<Projectile Path: arc>
<Projectile Arc: 72>
<Projectile Speed: 18>
<Projectile Motion: missile>
<Projectile Launch SE: Bow1>
```

---

### Skill 3 — Mortar Rock

| Field | Value |
|---|---|
| Scope | 1 Enemy |
| Hit Type | Physical Attack |
| Damage Type | HP Damage |
| Formula | `a.atk * 2.5 - b.def` |

```text
<Projectile: Boulder>
<Projectile Path: mortar>
<Projectile Launch Height: 160>
<Projectile Mortar Phase: 0.35>
<Projectile Face Path: true>
<Projectile Speed: 12>
<Projectile Rotation: true>
<Projectile Rotation Speed: 0.20>
```

---

### Skill 4 — Chain Spark

| Field | Value |
|---|---|
| Scope | All Enemies |
| Hit Type | Magical Attack |
| Damage Type | HP Damage |
| Formula | `a.mat * 1.8 - b.mdf` |

```text
<Projectile: spark_blue>
<Projectile Path: straight>
<Projectile Behavior: chain>
<Projectile Chains: 3>
<Projectile Chain Range: 9999>
<Projectile Chain Target: nearest>
<Projectile Speed: 18>
<Projectile Glow: true>
```

---

### Skill 5 — Split Nature Burst

| Field | Value |
|---|---|
| Scope | 1 Enemy |
| Hit Type | Magical Attack |
| Damage Type | HP Damage |
| Formula | `a.mat * 2.2 - b.mdf` |

```text
<Projectile: seed_green>
<Projectile Path: arc>
<Projectile Arc: 96>
<Projectile Speed: 12>
<Projectile Stage 2 Trigger: progress 60%>
<Projectile Stage 2 Image: petal_shard>
<Projectile Stage 2 Path: homing>
<Projectile Stage 2 Count: 3>
<Projectile Stage 2 Spread: 35>
<Projectile Stage 2 Speed: 18>
<Projectile Stage 2 Damage: each>
<Projectile Glow: true>
```

---

### Skill 6 — Shadow Backstab

| Field | Value |
|---|---|
| Scope | 1 Enemy |
| Hit Type | Physical Attack |
| Damage Type | HP Damage |
| Formula | `a.atk * 3 - b.def` |

```text
<Projectile: shadow_blade>
<Projectile Path: arc>
<Projectile Hit: backMiddle>
<Projectile Back Attack: true>
<Projectile Back Arc Height: 120>
<Projectile Back Overshoot: 72>
<Projectile Face Path: true>
<Projectile Speed: 18>
```

---

### Skill 7 — Boss Barrage

| Field | Value |
|---|---|
| Scope | Random 2 Enemies |
| Hit Type | Physical Attack |
| Damage Type | HP Damage |
| Formula | `a.atk * 2 - b.def` |
| Repeat | 2 or 3 |

```text
<Projectile: arrow_red>
<Projectile Path: arc>
<Projectile Arc: 88>
<Projectile Speed: 18>
<Projectile Timing: simultaneous>
<Projectile Waves: 3>
<Projectile Wave Delay: 45>
<Projectile Launch SE: Bow1>
```

---

## 13. Element-themed examples

### Fire Bolt

```text
<Projectile: fire_bolt>
<Projectile Path: straight>
<Projectile Speed: 16>
<Projectile Glow: true>
```

---

### Ice Shard

```text
<Projectile: ice_shard>
<Projectile Path: arc>
<Projectile Arc: 48>
<Projectile Speed: 18>
<Projectile Face Path: true>
```

---

### Water Wave

```text
<Projectile: water_wave>
<Projectile Path: sine>
<Projectile Sine Amplitude: 24>
<Projectile Sine Frequency: 2>
<Projectile Speed: 14>
<Projectile Trail: true>
```

---

### Earth Boulder

```text
<Projectile: Boulder>
<Projectile Path: rolling>
<Projectile Hit: frontBottom>
<Projectile Speed: 10>
<Projectile Rotation: true>
<Projectile Rotation Speed: 0.30>
```

---

### Lightning Bolt

```text
<Projectile: lightning_bolt>
<Projectile Path: zigzag>
<Projectile Zigzag Width: 40>
<Projectile Zigzag Segments: 6>
<Projectile Speed: 18>
<Projectile Glow: true>
```

---

### Nature Petal Shard

```text
<Projectile: petal_shard>
<Projectile Path: homing>
<Projectile Homing Strength: 0.25>
<Projectile Speed: 16>
<Projectile Trail: true>
```

---

### Sonic Wave

```text
<Projectile: sonic_ring>
<Projectile Path: sine>
<Projectile Sine Amplitude: 20>
<Projectile Sine Frequency: 4>
<Projectile Speed: 18>
<Projectile Glow: true>
```

---

### Sludge Ball

```text
<Projectile: sludge_ball>
<Projectile Path: arc>
<Projectile Arc: 64>
<Projectile Speed: 12>
<Projectile Rotation: true>
<Projectile Rotation Speed: 0.12>
```

---

## 14. Common mistakes these examples avoid

### Do not include `.png` in the notetag unless necessary

Recommended:

```text
<Projectile: Fireball>
```

Avoid:

```text
<Projectile: Fireball.png>
```

The plugin can clean `.png`, but clean docs should teach users to leave it off.

---

### Do not use raw weapon tags to control every named skill

For named skills, put the full behavior on the skill itself:

```text
<Projectile: Boulder>
<Projectile Path: mortar>
<Projectile Launch Height: 160>
```

Use weapon projectile tags mainly for Attack or weapon-element skills.

---

### Keep strict image checking off for normal projects

Recommended:

```text
Strict Missing Projectile Image Check: false
```

This is safer for playtests, browser builds, and deployed builds.

---

### Use Path + Behavior for new examples

Recommended:

```text
<Projectile Path: straight>
<Projectile Behavior: boomerang>
```

Older compatible style:

```text
<Projectile Mode: boomerang>
```

---

## 15. Quick copy-paste checklist

For a new projectile skill:

1. Put the PNG in `img/projectiles/`.
2. Use the image name without `.png`.
3. Put the projectile notetags in the skill note box.
4. Use `<Projectile Path: ...>` for movement.
5. Use `<Projectile Behavior: ...>` only when needed.
6. Draw most projectile art facing right.
7. Keep `<Projectile Auto Flip: true>`.
8. Test first with one enemy and one simple skill.

Minimal working test:

```text
<Projectile: Fireball>
<Projectile Path: straight>
<Projectile Speed: 14>
```

---

## 16. Suggested demo image names

These names match the examples in this document. Your project can use different names as long as the notetags match your PNG filenames.

```text
Arrow.png
Boulder.png
Boomerang.png
Fireball.png
beam_yellow.png
blast_green.png
boss_boulder.png
chaos_bolt.png
energy_disc.png
fire_bolt.png
fire_burst.png
ice_shard.png
lightning_bolt.png
magic_missile.png
orb_blue.png
orb_purple.png
petal_shard.png
rock_chunk.png
rock_shard.png
seed_green.png
shadow_blade.png
slash_blue.png
sludge_ball.png
sonic_ring.png
spark_blue.png
water_wave.png
wave_blue.png
```

---

## 17. Support note

When asking for setup help, include:

- RPG Maker MV version
- Okapi Projectile Core version
- plugin filename
- where the projectile PNG is located
- exact skill/item/weapon/enemy/state notetags
- whether side-view or front-view battle is being used
- a screenshot or console error if available

