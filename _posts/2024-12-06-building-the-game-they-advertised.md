---
layout: post
title: "Day 1 - 2 hours - Feasibility and Project Planning"
date: 2024-12-06
categories: [planning, learning]
---

## The Problem with Total Battle

If you've spent any time on mobile gaming, you've seen the ads for Total Battle. They show a fun, arcade-style combat game where you control troops running through enemy formations, collecting resources, and strategically building your army as you go.

That's not what the actual game is.

What you get is a city-building game wrapped in layers of monetization mechanics. Timers, energy systems, pay-to-win upgrades—all the standard mobile game dark patterns. The fun combat game from the ads exists, but it's buried behind a system designed to extract money from you.

I don't want to pay my way through artificial barriers to reach the game they advertised. I just want to play that game.

So I'm going to build it myself.

## Starting from Zero

I've never made a game before. As a data analyst, I do write some code for a living, but that's not game development. I have no idea where to start, but since AI is a thing I figure I'll kick the tires. First, Claude and I will have a feasibility conversation, then we'll make a plan.

I started by downloading gameplay footage using `yt-dlp`—videos showing the actual combat mechanics that Total Battle advertises. Then I set up two directories: one for project notes in Obsidian, and one for the code repository.

With the videos in hand, I opened Claude Desktop for that feasibility conversation. I uploaded the gameplay videos and gave this instruction: "I want to create a personal clone of this game for my Google Android phone. Please look at the video and assess the feasibility and difficulty of this project."

## The Feasibility Assessment

Claude analyzed the videos and broke down the core mechanics:
- Lane-based runner movement with auto-advancing units
- Automatic combat when units encounter enemies
- Interactive elements: destructible objects, value-accumulating gates, multiplier zones
- Resource collection through tap controls
- Simple numerical combat system

The assessment was surprisingly optimistic. These mechanics are well-documented patterns in game development. The technical challenges are manageable. With a proper game engine (Godot 4 was recommended for being free, open source, and beginner-friendly), this is absolutely achievable.

The key insight: start simple, iterate upward. Build the core gameplay loop first—troops moving, basic combat, resource collection. Skip the complexity initially. Get something playable quickly, then improve.

Claude even outlined a phased development approach, estimated timelines, and identified free resources for assets and learning materials. The recommendation was clear: this is feasible for a motivated developer, even one without game development experience.

This is actually very cool. I have a personalized tutorial based on my experience and goals. But, lol, who knows if it will work.

## What's Next

I have a solid plan. The scope is manageable. The tools are free and well-documented. Most importantly, I'll be building something I actually want to play—the game Total Battle advertised, without the predatory monetization.

Next steps:
- Install Godot 4 and complete a beginner tutorial
- Set up the project structure with Git
- Build a minimal prototype: units that move and can engage in basic combat
- Iterate from there

This is a learning project. I expect mistakes, dead ends, and plenty of problem-solving. But I'm documenting it all here—both for my own reference and to show what's possible when you remove the artificial barriers and just focus on building something fun.

---

## Full Feasibility Assessment

Below is the complete technical assessment from my conversation with Claude:

# Total Battle Clone - Feasibility Assessment

## Executive Summary

**Project:** Android clone of Total Battle casual runner/combat game  
**Assessment Date:** December 6, 2025  
**Overall Feasibility:** MODERATE to HIGH  
**Estimated Difficulty:** INTERMEDIATE (5/10)  
**Recommended Timeline:** 3-5 months for MVP  
**Recommended Engine:** Godot 4  
**Development Approach:** Claude Code + Zero Budget  
**Total Cost:** $0

---

## Game Analysis

### Core Game Mechanics Identified

1. **Runner/Auto-Battle Hybrid**
   - Vertical scrolling lane-based movement
   - Player units automatically advance forward
   - Automatic engagement with enemy units
   - Tap-to-collect treasure chests during run

2. **Combat System**
   - Simple numerical damage system (visible damage numbers)
   - Unit stacking/grouping (units cluster together)
   - Health bars for player units
   - Auto-attack when encountering enemies
   - Different unit types (archers, melee fighters, cavalry)

3. **Progression Elements**
   - **Treasure Chests (Multiple Types):**
     - Barrels: Destructible chests that you shoot to open
     - Gates: Pass-through chests that increase value when shot before walking through
     - Standard chests: Walk-through collection with fixed values (30, 45, 90, 120, etc.)
   - **Multipliers:** Floor-based multiplier zones (x2, x20, etc.) that boost unit count
   - Unit collection/merging mechanics
   - Progressive difficulty (enemy groups get larger)
   - Multiple unit types that can be combined

4. **Interactive Elements**
   - **Shooting Mechanic:** Units automatically shoot at destructible objects (barrels)
   - **Value Accumulation:** Gates accumulate damage/shots and convert to unit value
   - **Multiplier Zones:** Walk-over areas that multiply unit count
   - **Lane-based Movement:** Units stay in lanes while auto-advancing
   - **Touch Collection:** Tap to pick up collectibles while moving

5. **Visual Style**
   - 3D isometric/perspective view
   - Colorful, casual mobile game aesthetic
   - Simple particle effects (sparkles, damage indicators)
   - Stylized medieval/fantasy theme
   - Textured ground path with foliage borders

---

## Feasibility Analysis

### ✅ HIGHLY FEASIBLE Components

1. **Game Loop** - Very straightforward
   - Linear progression path
   - Simple state management (running, fighting, collecting)
   - Clear win/loss conditions

2. **Core Mechanics** - Well-documented patterns
   - Endless runner mechanics widely available
   - Auto-combat systems are common in mobile games
   - Touch controls are simple (tap to collect)

3. **2D Alternative** - Can simplify significantly
   - Could use 2D sprites instead of 3D models
   - Top-down or side-scrolling perspective
   - Still maintain the core gameplay feel

### ⚠️ MODERATE CHALLENGE Components

1. **Shooting & Collision System**
   - Auto-targeting for destructible objects
   - Projectile spawning and collision detection
   - Damage accumulation on gates (track shots before walking through)
   - Different object types (barrels vs gates) with different behaviors

2. **Multiplier Zones**
   - Trigger detection when units walk over zones
   - Unit count multiplication logic
   - Visual feedback for multiplier activation
   - Smooth unit spawning when count increases

3. **3D Graphics** (if pursuing authentic look)
   - Requires 3D modeling skills or asset purchasing
   - Animation system for characters
   - Camera positioning and movement
   - Particle effects system

4. **Unit Management System**
   - Grouping/formation logic
   - Unit type differentiation
   - Merging/stacking mechanics
   - Collision detection between multiple units

5. **Balance & Polish**
   - Enemy wave generation algorithm
   - Difficulty curve tuning
   - Reward/progression pacing
   - Visual feedback and juice

### ❌ LOW COMPLEXITY (Not in this game)

- No complex AI needed (auto-combat)
- No multiplayer components
- No complex physics
- No procedural generation (can use fixed levels)
- No complex UI/menus visible in gameplay

---

## Technical Stack Recommendations

### Godot 4 (STRONGLY RECOMMENDED)

**Why Godot 4 is perfect for this project:**

✅ **100% Free & Open Source**
- No licensing fees, ever
- No revenue caps or splash screen requirements
- Your game is completely yours

✅ **Excellent for 3D Games**
- Built-in 3D engine with modern rendering
- Low-poly 3D perfect for this game style
- Fixed camera 3D is simple (like 2.5D)
- Matches the original game's look

✅ **Ideal for Claude Code Development**
- GDScript files (.gd) are plain text—easy for AI to read/write
- Scene files are text-based (not binary like Unity)
- Simple, clear project structure
- Claude Code can navigate and modify everything

✅ **Zero-Budget Friendly**
- Built-in tools for everything (no paid plugins needed)
- Large free asset community
- No hidden costs or upsells

✅ **Great Android Export**
- One-click export to Android APK
- Good performance on mobile devices
- Touch input handling built-in

✅ **Beginner-Friendly**
- GDScript is Python-like (easier than C#)
- Excellent documentation
- Growing tutorial ecosystem
- Smaller learning curve than Unity

**What you get out of the box:**
- Physics engine (3D and 2D)
- 3D rendering with PBR materials
- Animation system with blend trees
- Particle effects (3D particles)
- UI system
- Audio engine with 3D spatial sound
- Built-in code editor (or use VS Code)

**Godot for This Specific Game:**
- Runner mechanics: CharacterBody3D with simple Z-axis movement
- Shooting system: Area3D collision detection
- 3D models: Direct import of .glb/.gltf files from Kenney
- Particle effects: GPUParticles3D (hardware accelerated)
- UI: Control nodes overlay on 3D view
- Camera: Fixed Camera3D (set once, never moves)
- Touch input: Native touch event handling

**Free Asset Sources:**
- **Kenney.nl** - Thousands of free 3D models (.glb format)
  - Battle/Medieval packs for units and enemies
  - Platformer Kit for environment
  - All low-poly, optimized for mobile
- OpenGameArt.org - Community 3D models
- itch.io - Free 3D asset packs
- Godot Asset Library - Free plugins and tools

**Download:** https://godotengine.org/download

---

## Development Phases

### Phase 1: Core Prototype (2-3 weeks)
- [ ] Basic runner movement (auto-forward)
- [ ] Single unit following path
- [ ] Lane-based movement system
- [ ] Simple auto-shooting mechanic
- [ ] Destructible objects (barrels)
- [ ] Tap to collect mechanic
- [ ] Basic collision detection

### Phase 2: Interactive Mechanics (2-3 weeks)
- [ ] Gate system (accumulate value before walking through)
- [ ] Multiplier zones (floor triggers)
- [ ] Enemy encounters with auto-combat
- [ ] Unit count multiplication logic
- [ ] Projectile collision with gates/barrels

### Phase 3: Combat System (2-3 weeks)
- [ ] Multiple unit types
- [ ] Unit grouping/formations
- [ ] Enemy waves with different types
- [ ] Damage calculation system
- [ ] Health bars and visual feedback

### Phase 4: Visual Polish (2-3 weeks)
- [ ] Art assets (create or purchase)
- [ ] Particle effects (shots, explosions, multiplier activation)
- [ ] Animations (shooting, walking, collecting)
- [ ] UI elements (unit count, multiplier display)
- [ ] Sound effects (shooting, barrel destruction, gate activation)

### Phase 5: Progression & Balance (2-3 weeks)
- [ ] Level design
- [ ] Difficulty curve
- [ ] Reward system
- [ ] Unit upgrade mechanics
- [ ] Save/load system

### Phase 6: Android Optimization (1-2 weeks)
- [ ] Performance optimization
- [ ] Touch control refinement
- [ ] Screen size adaptation
- [ ] Battery optimization
- [ ] Testing on actual devices

---

## Difficulty Rating by Skill Level

### Beginner (0-1 year programming/game dev)
**Difficulty: 7/10**
- Will need to learn game engine basics
- Recommend starting with 2D version
- Follow existing tutorials for similar games
- Use free asset packs initially
- Estimated time: 4-6 months

### Intermediate (1-3 years experience)
**Difficulty: 4/10**
- Should be able to complete in 2-3 months
- Can experiment with 3D graphics
- Good opportunity to learn mobile optimization
- Estimated time: 2-3 months

### Advanced (3+ years, game dev experience)
**Difficulty: 2/10**
- Straightforward implementation
- Main challenge is polish and feel
- Could complete MVP in 3-4 weeks
- Estimated time: 1-2 months

---

## Resource Requirements

### Zero-Budget Setup (RECOMMENDED)
- Development PC (moderate specs fine—Godot is lightweight)
- Android device for testing (your phone)
- Godot 4 (free)
- Free art assets from Kenney.nl, OpenGameArt, itch.io
- Free sound effects from freesound.org, OpenGameArt
- GIMP (free) for image editing if needed
- Audacity (free) for audio editing if needed
- **Total Cost: $0**

### Optional Upgrades (if budget allows later)
- Better Android device for testing ($100-300)
- Commissioned custom art ($50-500)
- Professional sound effects pack ($10-50)
- **Estimated Cost: $50-500** (only if you want to upgrade later)

---

## Key Success Factors

### Critical for MVP
1. **Smooth runner movement** - Must feel good
2. **Satisfying combat feedback** - Numbers, particles, shake
3. **Clear progression** - Player must feel they're getting stronger
4. **Responsive controls** - Touch must be instant
5. **Performance** - Must run at 60fps on mid-range devices

### Nice to Have
1. Multiple character skins
2. Different environments/themes
3. Special abilities/power-ups
4. Achievement system
5. Sound effects and music

---

## Risk Assessment

### High Risk
- **Performance on older devices** - Mitigation: Start with 2D, optimize early
- **Scope creep** - Mitigation: Stick to MVP features first

### Medium Risk
- **Art asset quality** - Mitigation: Use proven asset packs, iterate
- **Balance tuning** - Mitigation: Extensive playtesting, adjustable parameters

### Low Risk
- **Core mechanics** - Well-understood patterns
- **Platform compatibility** - Godot handles Android well
- **Technical feasibility** - No blocking technical challenges

---

## Recommendations

### For Success
1. **Start with 2D version** - Get mechanics working first
2. **Use existing assets** - Don't create art from scratch initially
3. **Build one feature at a time** - Test each thoroughly
4. **Playtest early and often** - Feel is everything in these games
5. **Keep scope small** - You can always add features later

### Learning Resources
- **Godot Documentation** - https://docs.godotengine.org
- **GDQuest** - Free Godot tutorials on YouTube
- **Heartbeast** - Excellent Godot game dev tutorials
- **Godot Recipes** - https://kidscancode.org/godot_recipes/
- **Reddit r/godot** - Active, helpful community
- **Godot Discord** - Real-time help and support
- **Free Godot courses** on YouTube (search "Godot 4 beginner")

### Alternative Approach
If 2-4 months seems too long, consider:
- **Ultra-minimal version** - Single unit type, simple graphics, 5 levels
- **Time investment**: 2-4 weeks
- **Purpose**: Proof of concept and learning

---

## Implementation Notes for Key Mechanics

### Shooting System
**Complexity:** Medium  
**Implementation:**
- Units automatically fire projectiles when moving
- Area3D collision detection for projectile hits
- Two target types: instant-destroy (barrels) and accumulate-value (gates)
- Visual feedback: 3D projectile mesh, hit particles

**Godot 4 Approach (3D):**
```gdscript
# Projectile scene with Area3D and MeshInstance3D
# Auto-spawn projectiles on timer
# Use area_entered signal for collision
# Track "damage" counter on gate objects
# Use CharacterBody3D for units
# RigidBody3D for projectile physics (optional)
```

### Gate Value Accumulation
**Complexity:** Medium  
**Implementation:**
- Gates have a "current value" that increases with each hit
- Display value above gate (Label node)
- When player walks through, add accumulated value to unit count
- Reset gate after collection

**Key Challenge:** Balancing how much shooting increases the gate value

### Multiplier Zones
**Complexity:** Low  
**Implementation:**
- Floor trigger zones (Area3D with CollisionShape3D)
- body_entered signal: multiply current unit count
- Spawn additional units smoothly (not instant pop)
- Visual effect when multiplier activates (GPUParticles3D)

**3D Approach:** Flat 3D plane on ground with particle effect above

### Lane Management
**Complexity:** Low  
**Implementation:**
- Define lane positions (e.g., left lane x=-1, center x=0, right x=1)
- Units snap to nearest lane
- Constrain horizontal movement
- Optional: swipe to change lanes

---

## Updated Difficulty Assessment

With the additional mechanics identified:

**Overall Difficulty: Still INTERMEDIATE (5/10)**

The shooting and multiplier mechanics add complexity but are well within reach:
- ✅ Shooting is common in Godot tutorials
- ✅ Multipliers are simple math + trigger zones
- ✅ Gates are just counter variables + UI
- ⚠️ Balancing will take playtesting

**Updated Timeline: 3-5 months for full MVP** (was 2-4 months)
- Extra month accounts for shooting mechanics and multiplier system polish
- Using Claude Code will help accelerate development
- Godot's simplicity reduces learning curve time

---

## Claude Code Development Tips

### Project Structure for AI Development
```
game_clone/
├── project.godot          # Godot project file
├── scenes/                # Game scenes
│   ├── main.tscn
│   ├── player.tscn
│   ├── enemy.tscn
│   └── projectile.tscn
├── scripts/               # GDScript files
│   ├── player.gd
│   ├── enemy.gd
│   ├── game_manager.gd
│   └── projectile.gd
├── assets/                # Art and audio
│   ├── sprites/
│   ├── audio/
│   └── fonts/
└── README.md              # Project documentation
```

### Working with Claude Code

**Claude Code can help you:**
- Write GDScript for game mechanics
- Debug collision detection issues
- Implement game logic (scoring, health, etc.)
- Create reusable components
- Optimize performance
- Structure your project properly

**Example prompts for Claude Code:**
- "Create a player movement script with lane switching"
- "Implement an auto-shooting system that fires every 0.5 seconds"
- "Add collision detection for projectiles hitting gates"
- "Create a multiplier zone that doubles unit count on entry"

**Best practices:**
- Keep scripts focused and modular
- Use clear variable and function names
- Comment your code for Claude to understand context
- Test small features incrementally
- Use Git to track changes (Claude Code integrates with Git)

---

## Conclusion

**This project is DEFINITELY FEASIBLE for a motivated solo developer.**

The core mechanics are straightforward, the technical challenges are manageable, and there are abundant resources available. The main variables are:

1. Your current skill level with game development
2. Whether you pursue 2D or 3D graphics
3. How much polish you want in the final product
4. Time you can dedicate per week

**My recommendation**: Start with a minimal 2D prototype focusing solely on the core loop (run, fight, collect). Once that feels good, gradually add complexity and visual polish.

The game's simplicity is actually its strength—it's an excellent learning project that can teach you mobile game development fundamentals without overwhelming complexity.

---

## Next Steps

1. **Download and install Godot 4** (https://godotengine.org/download)
2. **Set up Git repository** in C:\Users\rdpha\OneDrive\Documents\Workspace\game_clone
3. **Complete a Godot beginner tutorial** (2-4 hours)
   - GDQuest's "Your First 2D Game" is perfect
   - Learn basic nodes: Node2D, Sprite2D, Area2D, CharacterBody2D
4. **Create initial project structure** (see Claude Code tips above)
5. **Build Phase 1 prototype** with Claude Code:
   - Player that auto-moves forward
   - Simple lane switching
   - One enemy to collide with
   - Basic unit counter
6. **Gather free assets** from Kenney.nl for placeholder art
7. **Set up Android export** in Godot (one-time configuration)
8. **Review Stage Design Guide** (see Stage Design Guide.md)

**Ready to start development with Claude Code!**

---

## Additional Documentation

- **Week 1 Build Plan.md** - Detailed day-by-day guide with Claude Code prompts (START HERE!)
- **Stage Design Guide.md** - Comprehensive guide on designing and implementing stages
- **Asset Management Guide.md** - How to get and manage free assets (Claude Code's role)

---

*This is a learning project with no commercial intent. Just one developer trying to build the game that was advertised.*
