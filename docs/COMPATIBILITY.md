# Compatibility Guide

**Plugin:** Okapi Projectile Core  
**Current documented plugin version:** v2.20  
**Engine target:** RPG Maker MV  
**Author/Brand:** McKnight-Coding / Okapi Interactive

Okapi Projectile Core is designed as a self-contained RPG Maker MV battle projectile engine. It does **not** require Yanfly, Action Sequences, SV motion plugins, or any other Okapi plugin to run.

---

## Quick Compatibility Summary

| System / Plugin | Status | Notes |
|---|---:|---|
| RPG Maker MV | Supported | Primary target engine. Place the plugin in `js/plugins/`. |
| RPG Maker MZ | Not officially supported | This plugin is written for MV's battle/plugin structure. Use only after porting and testing. |
| Vanilla MV battle system | Supported | Works without Yanfly or other battle plugins. |
| Front-view battles | Supported | Projectile visuals and impact timing work. Side-view-only throw motions safely fall back. |
| Side-view battles | Supported | Optional throw/launch motion is attempted when available. |
| YEP_BattleEngineCore | Compatible / optional | Not required. The plugin includes compatibility handling for delayed projectile impact flow. |
| Yanfly Action Sequences | Compatible / optional | Not required. `ACTION EFFECT` is routed through projectile timing for projectile skills. |
| YEP_AutoPassiveStates | Optional compatibility | Passive-state notetags are only treated as passive projectile sources when YEP_AutoPassiveStates is enabled. |
| Other SV motion plugins | Likely compatible, test required | Projectile Core does not require SV motion plugins. Put Projectile Core below battle/motion plugins if conflicts happen. |
| Image/preload plugins | Usually compatible | Keep projectile images in the configured projectile folder. Test deployed builds. |

---

## Required Files and Folders

By default, projectile images go here:

```text
img/projectiles/
```

Example:

```text
img/projectiles/Fireball.png
img/projectiles/Arrow.png
img/projectiles/Boomerang.png
img/projectiles/Boulder.png
```

The plugin file goes here:

```text
js/plugins/Okapi_ProjectileCore.js
```

A repository-level `assets/` folder is **not required** for the plugin to run. You may include one in the GitHub repo for screenshots, GIFs, thumbnails, sample source images, or marketing art, but RPG Maker MV only needs the plugin file and the in-project image folder.

---

## Recommended Plugin Order

Use this order when your project includes Yanfly battle plugins:

```text
YEP_CoreEngine
YEP_BattleEngineCore
YEP_X_ActionSequencePack1 / Pack2 / Pack3, if used
Other battle/motion plugins
Okapi_ProjectileCore
```

If your project does not use Yanfly, place Okapi Projectile Core with your other battle plugins and keep it below plugins that heavily rewrite battle flow.

Why: Projectile Core delays damage, popups, battle log results, and animations until the projectile reaches the target. If another plugin rewrites battle action flow after Projectile Core loads, it may bypass that delayed-impact behavior.

---

## Yanfly Battle Engine Notes

Yanfly is **not required**. Projectile Core is meant to work in plain MV projects.

When YEP_BattleEngineCore or Action Sequences are present, Projectile Core includes special handling for projectile skills:

- Projectile actions wait while projectiles are active.
- `ACTION EFFECT` is delayed until projectile impact.
- Sequential projectile timing is routed into the sequential target queue.
- Default target actions are collapsed into one projectile volley to avoid weapon strikes continuing after the first projectile.
- Duplicate projectile launches are guarded against while a projectile action is already waiting.

Best practice: test projectile skills that use multi-target, random-target, repeats, and Yanfly action sequences before release.

---

## Skill, Weapon, and Equipment Notetag Priority

Projectile Core supports projectile notetags on:

- Skills
- Items
- Weapons
- Armors
- Actors
- Enemies
- Classes
- States

For **Normal Attack / weapon-element skills** using RPG Maker MV damage element `-1`, projectile settings may come from the battler's note sources. Priority is:

1. States on the subject
2. Actor primary weapon
3. Actor other weapons
4. Actor armors
5. Enemy database notes
6. Actor database notes
7. Actor class notes
8. Skill/item notes

This lets a bow, staff, sword, armor, state, actor, or enemy define the projectile used by weapon-element attacks.

### v2.20 named-skill safety rule

If a **non-weapon-element skill/item** has its own projectile note box, that skill/item owns its movement path, hit point, timing, stages, and behavior tags.

That means raw weapon/equipment projectile tags should **not** turn a named skill into a straight shot or override its custom path. For named skills, weapon/equipment changes should be placed inside a matching `<Projectile Override>` block.

---

## Modern Path + Behavior Tags

Recommended clean tags:

```text
<Projectile Path: straight>
<Projectile Path: arc>
<Projectile Path: mortar>
<Projectile Path: rolling>
<Projectile Path: bounce>
<Projectile Path: homing>
<Projectile Path: sine>
<Projectile Path: zigzag>
<Projectile Path: helix>
<Projectile Path: wild>

<Projectile Behavior: normal>
<Projectile Behavior: boomerang>
<Projectile Behavior: chain>
<Projectile Behavior: orbit>
```

Older tags such as `<Projectile Mode: arc>` still work for compatibility.

Important legacy notes:

- `figure8` is no longer a public supported path and safely falls back to `straight`.
- `trickShot` and `randomPath` behave as `zigzag` for backward compatibility.
- Old target-to-target `bounce` / `ricochet` behavior is now better represented as `chain` behavior.
- New `bounce` is for bouncing-ball projectile movement.

---

## Missing Projectile Image Safety

The plugin has a missing-image safety system.

Recommended settings:

```text
Warn Missing Projectile Images: true
Strict Missing Projectile Image Check: false
Missing Projectile Fallback: optional
```

`Strict Missing Projectile Image Check` is off by default because RPG Maker MV project paths can differ between editor, playtest, deployment, and NW.js builds. Passive mode lets MV attempt the normal image load instead of blocking a valid projectile because of a false file-check failure.

If strict mode is enabled and an image is missing:

1. The plugin warns in the console, if warnings are enabled.
2. It uses `Missing Projectile Fallback` if configured and valid.
3. If no valid image is available, it skips the projectile visual and resolves the action safely.

---

## Side-view Motion Compatibility

In side-view battle, Projectile Core may attempt to play the MV `throw` motion before launching a projectile.

This is safe and optional. If the project is front-view, no SV battler exists, or the motion is unavailable, the skill continues normally.

Supported motion override tags include:

```text
<Projectile Motion: weapon>
<Projectile Motion: attack>
<Projectile Motion: swing>
<Projectile Motion: thrust>
<Projectile Motion: missile>
<Projectile Motion: skill>
<Projectile Motion: spell>
<Projectile Motion: item>
<Projectile Motion: throw>
```

Default behavior:

- Physical projectile skills use the actor's equipped weapon motion.
- Sword, axe, and flail-style weapons use swing-like motion.
- Bow, crossbow, and gun-style weapons use missile-like motion.
- Magical projectile skills use spell motion.
- Item projectiles use item motion.

---

## Multi-target, Repeats, and Timing

Projectile Core supports simultaneous and sequential projectile timing:

```text
<Projectile Timing: simultaneous>
<Projectile Timing: sequential>
```

Default is simultaneous.

For skills/items with **Invocation Repeat** greater than 1, each repeat becomes a delayed projectile wave instead of stacking all damage instantly. Duplicate random target rolls are preserved.

Optional repeat tags:

```text
<Projectile Waves: 2>
<Projectile Wave Delay: 120>
<Projectile Wave Delay Seconds: 2>
```

---

## Known Safe Defaults

For most end users, recommend this simple starting setup:

```text
Projectile Folder: projectiles/
Missing Projectile Fallback: blank or a known-safe fallback image
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

## Compatibility Testing Checklist

Before shipping a game or demo, test:

- A basic single-target straight projectile.
- An arc projectile in front-view and side-view, if your game supports both.
- A mortar projectile.
- A projectile using a left-facing or right-facing image.
- A weapon-element attack using weapon projectile tags.
- A named skill that has its own projectile path while the actor has a weapon projectile tag.
- Multi-target simultaneous projectiles.
- Sequential projectiles.
- Repeat-count projectile waves.
- Missing image behavior with warnings enabled.
- Deployed Windows build, not only MV playtest.
- Browser/web deployment, if you offer a web demo.

---

## Troubleshooting Compatibility Issues

### Weapon tags are overriding a named skill

Use v2.20 or later. For non-weapon-element skills with their own projectile notes, the skill should own the path and behavior. Put equipment-based changes inside `<Projectile Override>` blocks instead of raw weapon projectile tags.

### Arc or mortar looks like a straight shot

Use the clean path tags:

```text
<Projectile Path: arc>
```

or

```text
<Projectile Path: mortar>
```

Also check that another plugin is not overriding battle flow after Projectile Core.

### A left-flying projectile appears upside down

Use:

```text
<Projectile Image Facing: right>
<Projectile Auto Flip: true>
```

If the art naturally faces left, use:

```text
<Projectile Image Facing: left>
<Projectile Auto Flip: true>
```

v2.20 uses horizontal flip plus rotation for path-facing so left-flying projectiles should not need a 180-degree upside-down rotation fix.

### Projectile image is missing in deployment

Check:

- The file is inside `img/projectiles/` or the configured projectile folder.
- The filename matches exactly, including capitalization.
- The notetag does not include `.png`; both can be cleaned, but extensionless names are cleaner.
- The image is included in your deployed build.
- `Strict Missing Projectile Image Check` is left off unless you intentionally want strict checking.

---

## License / Terms Reminder

The plugin header states that it is free to use and modify for personal or commercial RPG Maker MV projects.
