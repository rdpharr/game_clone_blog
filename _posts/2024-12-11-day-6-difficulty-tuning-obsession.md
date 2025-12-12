---
layout: post
title: "Day 6 - 4 Hours - When Difficulty Tuning Becomes an Obsession"
date: 2024-12-11
categories: [development, game-design]
tags: [godot, difficulty, balance, game-feel, tuning]
---

## Progress Summary

I set out to add a few balance tweaks. What happened instead was a 4-hour deep dive into difficulty tuning that turned the game from "too easy" to "actually challenging" to "I can't stop playing with these values." The game is mechanically complete, visually polished, and now... I'm obsessed with making the numbers *feel* right.

**Time Spent:** 4 hours  
**Major Accomplishments:**
- Unlimited unit accumulation system (no more 30-unit cap)
- Dynamic formation crowding (units visually stack)
- Significantly harder collectible requirements (5x bullets per barrel, 10 per gate value)
- Individual unit collision detection with proximity activation
- 2-minute auto-spawning system with difficulty scaling
- Death particles and smooth animations
- Extensive balance tuning across all systems

**Milestone:** Game now has proper difficulty curve and replayability

---

## Play Current Build

<div class="game-container" style="text-align: center; margin: 2em 0;">
  <iframe src="/game_clone_blog/builds/day-6/" width="800" height="600" frameborder="0" allowfullscreen></iframe>
  <p><em>Controls: Mouse to move horizontally | Projectiles auto-fire</em></p>
</div>

> **Note**: First load may take a moment. The game runs in your browser using WebAssembly.
> 
> **What's new**: Significantly harder difficulty! Barrels now require 5x more bullets to open, gates need 10 bullets per value point, and collectibles are larger (easier to see, harder to avoid). Your unit count is unlimited and displayed above the formation. Individual units have collision detection - watch them get picked off at the edges of your swarm!
> 
> **Try surviving**: The difficulty ramps up over 2 minutes with continuous spawning. Can you manage your resources effectively? Notice how your formation dynamically reforms as you gain and lose units.

---

## The Slippery Slope of "Just One More Tweak"

Yesterday's build was satisfying. Combat worked, visuals were polished, the game felt good. But when I played it for testing, something nagged at me: **it was too easy.**

Barrels opened with 1-3 bullets. You'd hit the 30-unit cap within seconds. Gates were trivial to charge up. The strategic tension I'd designed for... wasn't really there.

"I'll just make barrels a bit harder to open," I thought. "Quick 15-minute balance pass."

**Four hours later**, I had:
- Completely rewritten the unit accumulation system
- Made collectibles 2-4x larger
- Increased barrel costs by 5x
- Implemented individual unit collision detection
- Added formation reformation logic
- Built an auto-spawning difficulty system
- Created death particle effects
- Tuned, retuned, and retuned every single numeric value in the game

This is what happens when you start asking "but what if..." and can't stop playtesting.

## The 30-Unit Cap Problem

The original system had a hard cap at 30 units. Once you hit it, barrels and gates just... stopped mattering. You'd play defensively, avoid damage, and coast.

**The issues:**
- No incentive to collect after cap
- Strategic decisions stopped mattering
- Progression felt limited
- "Win state" happened too early

**The fix: Unlimited accumulation with visual overflow**

```gdscript
# scripts/player_manager_2d.gd
var player_units: Array[Area2D] = []  # Physical units (capped at 200 for performance)
var total_unit_count := 0  # Total units including overflow (unlimited)

func add_units(amount: int) -> void:
    total_unit_count += amount  # Always track total
    
    # Only spawn physical units up to cap
    var units_to_spawn := min(amount, MAX_PLAYER_UNITS - player_units.size())
    for i in units_to_spawn:
        spawn_player_unit()
```

**What this enables:**
- Unit count can go to hundreds, thousands, whatever
- Physical units cap at 200 (performance limit)
- Floating label shows true total: "Units: 347"
- When you lose units, they respawn from the overflow pool
- Strategic value continues scaling indefinitely

Now there's **always** a reason to collect barrels and hit gates, no matter how many units you have.

## Making Collectibles Actually Hard

With unlimited accumulation, the old barrel costs were laughable. A barrel giving 25 units for 3 bullets? That's not a trade-off, that's a gift.

### Barrel Rebalance

**Old system:**
- Small barrel (5 units): 1 bullet
- Medium barrel (15 units): 2 bullets  
- Large barrel (25 units): 3 bullets

**New system:**
- Small barrel (5 units): **5 bullets** (1:1 ratio)
- Medium barrel (15 units): **15 bullets** (1:1 ratio)
- Large barrel (25 units): **25 bullets** (1:1 ratio)

Yes, that's right: **bullets required = units gained**. Every barrel is now a 1:1 exchange of projectiles for units.

**Why this works:**
- Clear mental math: "Is 5 units worth 5 bullets right now?"
- With 15 player units firing 2 bullets/sec each, that's 30 bullets/sec - a small barrel takes ~0.17 seconds to open
- But you're also shooting enemies, charging gates, managing threats
- **Bullet economy actually matters now**

### Gate Rebalance

**Old system:**
- Each bullet added +5 to gate value
- Could trivially turn -10 gates into +50 gates

**New system:**
- Base value (before shooting): 10, 20, or 30
- Each bullet adds +1 to value
- **10 bullets per 10 units gained**

A +30 gate? That's already 30 units if you don't shoot it. Want to add 20 more? That'll cost you 20 bullets (1 second of shooting with 10 units).

Negative gates are still worth converting, but now it's a real investment: -20 gate → +20 gate = 40 bullets to fully turn around.

### Size Increases

To match the increased difficulty, collectibles got bigger:

**Barrels:**
- Old: 64×64 pixels
- New: **128×128 pixels** (2x scale)
- Reasoning: They're harder to open, so you need more time to shoot them. Bigger = more warning, fairer challenge

**Gates:**
- Old: 64×64 pixels (3 tiles)
- New: **96×96 pixels** (3 tiles)
- Reasoning: Bigger decision points. You have more time to evaluate whether to invest bullets.

The larger sizes also improve readability and create better visual hierarchy. Collectibles are now **distinctly larger** than units and enemies.

## Individual Unit Collision Detection

This was the change that made the biggest difference to game feel.

### The Old System (Formation Collision)

```gdscript
# PlayerManager had one Area2D
# Collision = instant loss of multiple units based on damage value
```

Problems:
- All-or-nothing: one collision = lose 5-10 units instantly
- Felt random and punishing
- No skill in positioning individual parts of your swarm

### The New System (Per-Unit Collision)

```gdscript
# scripts/player_unit.gd
extends Area2D

func _on_area_entered(area: Area2D) -> void:
    # Check if we're close enough to parent formation
    var parent_manager = get_parent()
    var distance = global_position.distance_to(parent_manager.global_position)
    
    if distance > parent_manager.FORMATION_RADIUS * 1.5:
        return  # Too far from formation, don't trigger
    
    if area.is_in_group("enemy"):
        parent_manager.remove_units(1)  # This specific unit hit
```

**What changed:**
- **Each unit** has collision detection
- Units on the edge of your swarm take hits first
- You lose units **one at a time** instead of in chunks
- Proximity check prevents stray units from triggering false collisions

**Why this feels better:**
- You can see which part of your swarm is vulnerable
- Positioning matters - keep your formation tight
- Gradual attrition vs. sudden catastrophic loss
- More "fair" - you know exactly what hit you

Watch your swarm when playing - units at the front and sides get picked off first. If you're careless with positioning, you'll bleed units slowly. Stay centered and dodge well, and you'll keep your formation intact.

## Formation Reformation with Crowding

With unlimited units and individual collision, the formation system needed an upgrade.

### Dynamic Crowding

```gdscript
const FORMATION_RADIUS := 60.0  # Base radius
const MAX_PLAYER_UNITS := 200   # Performance cap

func update_unit_positions():
    # Units distributed in circular formation
    for i in range(player_units.size()):
        var angle = (i / float(player_units.size())) * TAU
        var target_pos = Vector2(
            cos(angle) * FORMATION_RADIUS,
            sin(angle) * FORMATION_RADIUS
        )
        player_units[i].global_position = global_position + target_pos
```

**Visual behavior:**
- 10 units: Loose, spread out circle (60px radius)
- 50 units: Dense ring formation
- 100 units: Tightly packed, overlapping sprites
- 200 units: Complete visual swarm (sprites stacked on top of each other)

The crowding creates a natural "power fantasy" escalation:
- Early game: Small, vulnerable squad
- Mid game: Growing army
- Late game: Dense, overwhelming swarm

And when you take damage, the formation **instantly reforms** around the remaining units - it's visually satisfying to watch.

## The Auto-Spawning Difficulty System

With all these balance changes, I needed a proper difficulty curve. Manual spawning wasn't cutting it anymore.

### 2-Minute Escalation

```gdscript
# scripts/spawn_manager.gd
const SPAWN_DURATION := 120.0  # 2 minutes
const ENEMY_SPAWN_INTERVAL_START := 3.0
const ENEMY_SPAWN_INTERVAL_END := 1.0
const COLLECTIBLE_SPAWN_INTERVAL_START := 5.0
const COLLECTIBLE_SPAWN_INTERVAL_END := 2.0

func _process(delta: float):
    elapsed_time += delta
    var progress = min(elapsed_time / SPAWN_DURATION, 1.0)
    
    # Spawn rates increase over time
    enemy_timer = lerp(ENEMY_SPAWN_INTERVAL_START, ENEMY_SPAWN_INTERVAL_END, progress)
    collectible_timer = lerp(COLLECTIBLE_SPAWN_INTERVAL_START, COLLECTIBLE_SPAWN_INTERVAL_END, progress)
```

**Progression:**

| Time | Enemy Spawn | Collectible Spawn |
|------|-------------|-------------------|
| 0:00 | Every 3 sec | Every 5 sec |
| 0:30 | Every 2.5 sec | Every 4.25 sec |
| 1:00 | Every 2 sec | Every 3.5 sec |
| 1:30 | Every 1.5 sec | Every 2.75 sec |
| 2:00 | Every 1 sec | Every 2 sec |

**By the end:**
- Enemy every second
- Barrel/gate every 2 seconds
- Constant decision-making pressure
- Bullet economy is **tight**

The difficulty doesn't just get harder - it gets **denser**. More threats, more opportunities, more decisions per second.

## Death Particles and Polish

Because dying should feel impactful, I added particle effects when units are destroyed.

### Unit Death Animation

```gdscript
# scripts/player_unit.gd (when unit is destroyed)
func die():
    # Spawn particle
    var particle = preload("res://scenes/fade_particle.tscn").instantiate()
    particle.global_position = global_position
    get_parent().get_parent().add_child(particle)
    
    queue_free()
```

```gdscript
# scripts/fade_particle.gd
extends Sprite2D

func _ready():
    # Fade out and shrink over 0.5 seconds
    var tween = create_tween()
    tween.tween_property(self, "modulate:a", 0.0, 0.5)
    tween.parallel().tween_property(self, "scale", Vector2.ZERO, 0.5)
    tween.tween_callback(queue_free)
```

**Visual feedback:**
- Unit collision → small sprite spawns at location
- Fades from opaque to transparent (0.5 sec)
- Shrinks from full size to nothing (0.5 sec)
- Leaves brief "ghost" showing where you took damage

It's a small touch, but it makes losses feel more visceral and gives you visual feedback about where your formation is getting hit.

## The Obsessive Tuning Loop

Here's what my afternoon actually looked like:

**13:00** - "Let me just make barrels harder"  
**13:30** - "Okay, gates need work too"  
**14:00** - "Wait, the unit cap doesn't make sense anymore"  
**14:30** - "Individual collision would be cool"  
**15:00** - "The formation should reform dynamically"  
**15:30** - "I need better spawning logic"  
**16:00** - "Death particles would be satisfying"  
**16:30** - "Let me test this new barrel cost..."  
**17:00** - "What if gates were 10 bullets per value instead of 8?"

Every change revealed another thing to tune. Every playtest suggested another tweak. The numbers kept whispering "but what if you changed me just a little bit more?"

### The Values That Changed (Partial List)

**Barrel costs:**  
1-3 bullets → 5-25 bullets (tried 3, 7, 10, finally 1:1 ratio)

**Gate charging:**  
+5 per bullet → +2 per bullet → +1 per bullet (tried scaling, settled on linear)

**Formation radius:**  
80px → 70px → 60px (tighter = more strategic positioning)

**Enemy chase speed:**  
80px/s → 120px/s → 100px/s (balance between threat and dodgeability)

**Spawn timing:**  
Static 5-second intervals → Dynamic 5→2 second scaling (tried 3→1, too frantic)

**Collectible sizes:**  
64px barrels → 96px → 128px (128 feels right for the new difficulty)

**Max physical units:**  
100 → 150 → 200 (200 creates the right "swarm" density)

I probably playtested 40+ different configurations. Some were too hard (gates costing 20 bullets each). Some were too easy (5 bullets per gate value). Some just felt *wrong* in ways I couldn't articulate.

But by the end, the numbers started to **sing**.

## What Good Difficulty Tuning Feels Like

After all those iterations, the game hits a sweet spot:

**Early game (0-30 seconds):**
- Low spawn pressure
- Easy to open barrels (fire rate > barrel cost)
- Building your unit count
- Learning the mechanics
- Feels achievable

**Mid game (30-60 seconds):**
- Spawn rates increasing
- Have to choose: shoot enemies or charge gates?
- Barrel costs feel significant
- Unit count fluctuating
- Strategic decisions matter

**Late game (60-120 seconds):**
- Constant spawning pressure
- Bullet economy is tight
- Every decision has weight
- Losing units feels bad, gaining units feels great
- High skill expression

**The moment of mastery:**

When you're juggling:
- Dodging enemies on the left
- Shooting a barrel on the right
- Deciding whether to invest in a +30 gate ahead
- Managing your formation positioning to minimize edge hits
- Tracking your unit count to know if you can afford losses

...and you pull it off? **That's when the game feels good.**

## What This Means for the Project

The game is now in a state I'd call **feature complete**:

✅ Core movement and combat  
✅ All Week 1-2 mechanics implemented  
✅ Polished visual identity  
✅ Difficulty curve that scales properly  
✅ Replayability through challenge  
✅ Individual unit dynamics

**What's left is optional expansion:**
- Week 3 features (multipliers, unobtainable gates)
- Multiple level variants
- Menu system and UI polish
- Sound effects
- Mobile touch controls
- Meta-progression (upgrades between runs)

But the core game? It's **done**. It's playable. It's fun. It has depth.

I could ship this as-is and it would be a complete game experience.

## Lessons Learned

**1. Balance Tuning Is Addictive**  
Once you start tweaking numbers, it's hard to stop. "Just one more value" turns into hours of iteration. But it's time well spent.

**2. 1:1 Ratios Are Mentally Clean**  
Bullets = units for barrels. It's simple math. Players can evaluate trades instantly. Complexity can come from context, not from the numbers themselves.

**3. Individual Unit Collision >>> Formation Collision**  
Feels fairer, more transparent, more skill-expressive. The change from "formation takes damage" to "units get picked off" is night and day.

**4. Bigger Collectibles = Better for Hard Difficulty**  
If something is costly to interact with (barrels now need 5-25 bullets), players need more time to see it coming and make decisions. Size = reaction time.

**5. Auto-Spawning > Manual Spawning**  
The 2-minute difficulty curve creates a natural arc. Early game: learn. Mid game: decide. Late game: survive. Manual spawning never felt this smooth.

**6. Death Feedback Matters**  
Those little fade particles? They make losses feel concrete instead of abstract. Visual feedback → emotional impact.

**7. Unlimited Systems Beat Capped Systems**  
The 30-unit cap felt restrictive. Unlimited accumulation with visual overflow? Freedom with clarity.

**8. Playtesting Is How You Find "Feel"**  
You can't math your way to good game feel. You have to play it, feel it, adjust, and play again. And again. And again.

## The Obsession Continues

Honestly? I'm still not done tuning.

Even as I write this, I'm thinking:
- "What if barrels gave 10/20/30 instead of 5/15/25?"
- "Should gates start at 5/10/20 instead of 10/20/30?"
- "Is 200 the right physical unit cap or should it be 250?"
- "Could enemies spawn slightly faster at the 90-second mark?"

This is the dangerous part of game development: **knowing when to stop**. The numbers are "good enough" now. Great, even. But there's always this nagging feeling that they could be **perfect** with just one more change.

I need to resist the urge. The game is balanced. It's fun. It's challenging but fair. Time to move on.

...But maybe I'll test one more barrel cost tweak first.

## Time Breakdown

- Unlimited unit system implementation: 45 min
- Barrel/gate balance redesign: 30 min
- Individual unit collision detection: 40 min
- Formation reformation logic: 25 min
- Auto-spawning difficulty system: 35 min
- Death particles and animation: 20 min
- **Obsessive playtesting and tuning:** 85 min

**Total:** ~4 hours

---

**Development Time:** 4 hours  
**GitHub Commits:** 3 total
- Feature: Unlimited unit accumulation + balance adjustments
- Visual & gameplay improvements  
- Documentation: Update for latest features

**Current Status:** Feature complete, difficulty tuned, genuinely fun to play

Tomorrow: Either Week 3 features (multipliers, effects) or content variety (multiple level layouts). The foundation is rock solid - time to build upward or outward.
