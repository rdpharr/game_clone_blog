---
layout: post
title: "Day 4 - 2 Hours - 2D Refactor and All of Week 2 Plans"
date: 2024-12-09
categories: [development, gameplay]
tags: [godot, refactor, projectiles, combat, week-2]
---

## Progress Summary

Sometimes the best path forward is to completely change direction. After yesterday's 3D spatial model confusion, I made the call to pivot to 2D overhead perspective - and in doing so, completed all of Week 2's combat features in a single session. What was intended to take a week happened in 2 hours, and the game is finally *playable*.

**Time Spent:** 2 hours  
**Major Accomplishments:**
- Complete 2D refactor with overhead runner perspective
- Auto-fire projectile system working
- Barrel multi-shot mechanics (shoot-to-open)
- Gate accumulation system (positive/negative values)
- Enemy projectile damage
- All collision layers properly configured

**Milestone:** Week 2 complete - projectile combat system fully functional

---

## Play Current Build

<div class="game-container" style="text-align: center; margin: 2em 0;">
  <iframe src="/game_clone_blog/builds/day-4/" width="800" height="600" frameborder="0" allowfullscreen></iframe>
  <p><em>Controls: Mouse to move horizontally | Projectiles auto-fire</em></p>
</div>

> **Note**: First load may take a moment. The game runs in your browser using WebAssembly.
> 
> **What's playable**: Full Week 2 projectile combat! Movement, auto-firing, barrel shooting mechanics, gate accumulation, enemy combat, and unit management all working.
> 
> **Try these mechanics**: 
> - Watch projectiles auto-fire from your unit swarm
> - Shoot barrels multiple times to open them (see the bullet counter)
> - Shoot gates to increase their value before passing through
> - Thin enemy swarms with projectiles before collision
> - Notice how your unit count physically grows and shrinks

---

## The 2D Decision

Yesterday ended with a clear realization: the 3D spatial model was fighting us at every turn. Camera angles, Z-direction confusion, visual debugging challenges - all symptoms of choosing the wrong perspective for this type of game.

**Why 2D overhead makes sense:**

**1. Match the Gameplay**  
Total Battle's ads show a top-down runner. Not isometric 3D, not first-person - a clean overhead view where:
- Player moves up the screen
- Objects scroll down toward player
- Horizontal movement is purely left/right

**2. Simpler Mental Model**
- Y-axis is "forward" (up the screen)
- X-axis is horizontal positioning  
- No Z-axis confusion
- Camera is just "look at this area"

**3. Better for Auto-Runner**
The genre is all about clarity:
- See enemies approaching from distance
- Quick risk/reward decisions (shoot barrel or avoid?)
- React to upcoming gates and multipliers
- Clear line-of-sight for projectiles

**4. Kenney Assets Work Better**
The 3D models from Kenney.nl actually work great as 2D sprites - they're pre-rendered from useful angles. Using them as Sprite2D nodes is simpler than managing 3D transforms.

## The Refactor Process

The shift to 2D wasn't a complete rewrite - more like a perspective change with some smart simplifications:

**What Changed:**
- `Node3D` → `Node2D`
- `MeshInstance3D` → `Sprite2D`  
- 3D collision shapes → 2D collision shapes
- Camera3D → Camera2D
- Z-axis movement → Y-axis movement

**What Stayed the Same:**
- Scene structure (PlayerManager, EnemyGroup, GameManager)
- Formation logic (units cluster around center point)
- Collision detection patterns
- Basic movement systems

**Key Simplifications:**
- No camera follow complexity - just center on play area
- No rotation calculations - everything faces "up"
- No ground plane - 2D space is naturally flat
- Position.y handles "forward" movement naturally

The existing physical unit swarm system translated perfectly - it's still `Array[Sprite2D]` tracking individual units, just in 2D space now.

## Week 2 Features: All Implemented

With the 2D foundation solid, I tackled all the Week 2 projectile combat features in sequence. The simplified coordinate system made everything click into place.

### 1. Auto-Fire Projectile System

**Core Mechanic:**
- Player units auto-fire every 0.5 seconds
- Each unit fires one projectile (spread across formation width)
- Small yellow projectiles (8x8 pixels) move upward at 200 px/sec
- Despawn after traveling 800px or going off-screen

**Implementation:**
```gdscript
# In PlayerManager
func _on_shoot_timer_timeout():
    for unit in player_units:
        if unit and is_instance_valid(unit):
            spawn_projectile(unit.global_position)

func spawn_projectile(pos: Vector2):
    var projectile = projectile_scene.instantiate()
    var offset_x = randf_range(-FORMATION_RADIUS, FORMATION_RADIUS)
    projectile.global_position = pos + Vector2(offset_x, 0)
    get_parent().add_child(projectile)
```

The spread across formation width creates visual density - more units = more projectiles = more satisfying firepower.

### 2. Barrel Multi-Shot System

**The Risk/Reward:**
- Barrels start **unopened** (dangerous - they damage you)
- Display shows "bullets required" based on value
- Each projectile hit reduces the counter
- When counter reaches 0, barrel opens (turns green, safe)
- Walk through opened barrel = gain units
- Walk through unopened barrel = lose units

**Values scale with strategy:**
- Small barrel (value 5): 1 bullet to open
- Medium barrel (value 15): 2 bullets to open  
- Large barrel (value 25): 3 bullets to open

**State Machine:**
```gdscript
func on_projectile_hit():
    if is_open:
        return
    
    bullets_remaining -= 1
    if bullets_remaining <= 0:
        is_open = true
        bullets_remaining = 0
    
    update_display()  # Color tint + label update
```

This creates **bullet economy tension**: you can't shoot everything, so do you invest bullets in this barrel or save them for enemies?

### 3. Gate Accumulation System

**The Investment Mechanic:**
- Gates display their current value
- Can start positive, zero, or negative
- Each projectile hit adds +5 to value
- Color coding: green (positive), red (negative), white (zero)
- Walk through = gain/lose units based on current value

**Strategic Depth:**
- Negative gate ahead? Shoot it to make positive
- Positive gate? Maybe shoot it anyway for bigger reward
- Low on bullets? Skip the gate entirely
- High unit count? Invest bullets for multiplied value

**Visual Feedback:**
```gdscript
func update_display():
    value_label.text = str(current_value)
    
    var color: Color
    if current_value > 0:
        color = Color.GREEN
    elif current_value < 0:
        color = Color.RED
    else:
        color = Color.WHITE
    
    # Tint all three sprite tiles
    for sprite in [left_sprite, middle_sprite, right_sprite]:
        sprite.modulate = color
```

The three-tile width (96 pixels) makes gates very visible - you have time to make shooting decisions as they approach.

### 4. Enemy Projectile Damage

Enemies aren't just collision hazards anymore:
- Projectiles can hit individual enemy units
- Thin out swarms before they reach you
- Destroyed enemies despawn when all units killed
- Strategic choice: focus fire or spread damage?

**Implementation:**
```gdscript
# In EnemyGroup
func on_projectile_hit():
    if enemy_units.is_empty():
        return
    
    var unit = enemy_units.pop_back()
    if unit and is_instance_valid(unit):
        unit.queue_free()
    
    if enemy_units.is_empty():
        queue_free()  # Group destroyed
```

## The Collision Layer Puzzle

Getting all these interactions working required careful collision layer configuration. This took about 45 minutes of debugging - most of the session.

**The Problem:**  
Projectiles weren't hitting barrels and gates initially.

**The Root Cause:**  
I had set `collision_mask = 10` (decimal) thinking "layers 2 and 4."

But that's not how bit masks work:
- `10` (decimal) = `1010` (binary) = bits 1 and 3 = layers 2 and 4 *when counting from 1*
- But Godot counts from 0, so bit 2 = value 4 = layer 3

**The Fix:**
```gdscript
# Projectile collision settings
collision_layer = 128  # Layer 8 (bit 7)
collision_mask = 6     # Check layers 2 and 3 (bits 1 and 2)
```

Now `6` (decimal) = `0110` (binary) = detects enemies (layer 2, value 2) and collectibles (layer 3, value 4).

**Final Layer Configuration:**

| Layer | Bit | Value | Objects |
|-------|-----|-------|---------|
| 1 | 0 | 1 | Player |
| 2 | 1 | 2 | Enemies |
| 3 | 2 | 4 | Collectibles (Barrel, Gate) |
| 8 | 7 | 128 | Projectiles |

## Smart Collision Detection

Different objects have different scene hierarchies:
- **EnemyGroup:** Node2D → Area2D → CollisionShape2D  
- **Barrel/Gate:** Area2D → Sprite2D → CollisionShape2D

The projectile handles both with a dual-check pattern:

```gdscript
func _on_area_entered(area: Area2D):
    # Direct check (Barrel, Gate)
    if area.has_method("on_projectile_hit"):
        area.on_projectile_hit()
        queue_free()
    # Parent check (EnemyGroup)
    elif area.get_parent() and area.get_parent().has_method("on_projectile_hit"):
        area.get_parent().on_projectile_hit()
        queue_free()
```

Clean, flexible, and works for any future object types.

## Size Hierarchy for Visual Clarity

Different object scales create instant readability:

| Object | Size | Scale | Purpose |
|--------|------|-------|---------|
| Projectile | 8x8 px | 1x | Smallest - doesn't clutter screen |
| Unit (player/enemy) | 16x16 px | 2x | Swarm elements, many visible |
| Barrel | 32x32 px | 4x | Medium - clear collectible |
| Gate | 96x32 px | 3 tiles × 4x | Large - strategic decision point |

This creates natural visual hierarchy - you instantly know what you're looking at and its importance.

## What Week 2 Gives Us

With all these systems working together, the game now has **strategic depth**:

**Bullet Economy:**
- Fire rate: 2 bullets/sec per unit
- Max ~30 active projectiles with 15 units
- Can't shoot everything - prioritize targets

**Risk/Reward Decisions:**
- Shoot unopened barrel (costs bullets) or avoid (costs positioning)?
- How many bullets to invest in a gate?
- Kill enemies early (costs bullets) or dodge them (costs units)?

**Investment Strategy:**
- Negative gates can become positive with shooting
- But is it worth the bullet cost?
- Does your current unit count matter?

**Resource Management:**
- More units = more projectiles = more power
- But also bigger collision target
- Balance growth vs. safety

## Performance Notes

Despite the rapid projectile spawning and numerous moving parts:
- Solid 60 FPS throughout
- No errors in output panel
- Clean collision detection
- Smooth unit spawning/destroying

No pooling system needed yet - the fire rate is low enough that we're only managing ~30 projectiles max. Can add pooling later if needed, but premature optimization avoided.

## What's Next?

**Week 2 is complete**, but there's still Week 3 and Week 4 content planned:

**Week 3 Features:**
- Multiplier zones (2x, 3x value regions)
- Speed scaling (difficulty progression)
- Unobtainable gates (impossible to make positive)
- Particle effects on hits
- Sound effects

**Week 4+ Features:**
- Multiple level designs
- Upgrade system between runs
- Mobile touch controls
- Menu system
- Save/progress tracking

But honestly? **The game is playable now.** All core mechanics are functional. Everything from here is polish and content expansion.

I might tackle Week 3 features faster than planned, or I might focus on level design and content variety. We'll see what feels right tomorrow.

## Lessons Learned

**1. Sometimes You Need to Pivot**  
The 3D approach wasn't working. Rather than force it, switching to 2D unlocked everything. Don't be precious about sunk costs.

**2. Simpler Can Be Faster**  
The 2D refactor wasn't a setback - it was an accelerant. Sometimes reducing complexity lets you move faster.

**3. Collision Layers Are Critical**  
Spent 45 minutes on bit mask math. Now I understand Godot's collision system deeply. Time well invested.

**4. Physical Units Are Satisfying**  
Watching the swarm grow and shrink, seeing individual units fire projectiles - it's viscerally satisfying in a way numbers never are.

**5. Test Frequently**  
Every feature was tested immediately with F5. Caught issues early, fixed them quickly, moved on confidently.

**6. Week Plans Are Guidelines**  
Week 2 was supposed to take a week. It took 2 hours. That's okay - plans adapt to reality.

## Time Breakdown

- 2D refactor planning: 15 min
- Core 2D conversion: 25 min  
- Projectile system: 30 min
- Barrel mechanics: 25 min
- Gate system: 30 min
- Collision layer debugging: 45 min
- Testing and polish: 10 min

**Total:** ~2 hours

---

**Development Time:** 2 hours  
**GitHub Commits:** 1 (Week 2 projectile combat complete)  
**Current Status:** Week 2 complete, game is genuinely playable

Tomorrow: Either dive into Week 3 features or focus on level variety and content. The foundation is solid - now it's about building upward.
