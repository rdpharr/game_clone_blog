---
layout: post
title: "Day 3 - 2 Hours - Godot MCP Server and Week 1 Milestones Completed"
date: 2024-12-08
categories: [development, automation]
tags: [godot, mcp, claude-code, refactor]
---

## Progress Summary

Today brought rapid development but also revealed critical design issues that need addressing. I discovered a much better workflow using the Godot MCP server, had Claude Code blast through the Week 1 milestones in record time, but ended up with an unplayable prototype that highlighted fundamental problems with our spatial understanding.

**Time Spent:** 2 hours  
**Major Accomplishments:**
- Integrated Godot MCP server for better automation
- Completed all Week 1 milestones (movement, collision, HUD)
- Identified critical design issues requiring refactor
- Major design document update for physical unit representation

**Major Setback:**
- Game is technically complete but not playable due to confusion about coordinate systems and camera angles

## Finding a Better Workflow: Godot MCP

After yesterday's somewhat clunky workflow of having Claude Code generate scripts and then manually wiring them up in the Godot editor, I went searching for something better. I found the [godot-mcp](https://github.com/Coding-Solo/godot-mcp) project, which provides an MCP (Model Context Protocol) server specifically for Godot.

**Installation was straightforward:**

```bash
claude mcp add godot C:\\Users\\rdpha\\OneDrive\\Documents\\Workspace\\godot-mcp\\build\\index.js -e GODOT_PATH=C:\\Users\\rdpha\\OneDrive\\Documents\\Workspace\\Godot_v4.5.1\\Godot_v4.5.1-stable_win64.exe
```

This gives Claude Code a rich set of tools for interacting with Godot programmatically:

**Project Management:**
- Get Godot version
- List projects
- Get project metadata

**Running & Debugging:**
- Launch editor
- Run project
- Get debug output
- Stop project

**Scene Creation & Editing:**
- Create scenes
- Add nodes
- Load sprites
- Save scenes

**Advanced Operations:**
- Export mesh libraries
- Manage UIDs (Godot 4.4+)
- Update project resources

The hope was that this would let Claude Code do more of the heavy lifting automatically. And it did... sort of.

## Claude Code Completes Week 1... Too Fast?

With the new MCP tools available, I asked Claude Code to finish the remaining Week 1 milestones. It worked incredibly quickly:

1. Created the main game scene
2. Set up the camera system with proper follow behavior
3. Implemented ground collision
4. Built the HUD with a unit counter display
5. Integrated player movement with horizontal mouse tracking
6. Connected enemy collision with unit reduction
7. Added barrel collection mechanics

All of this happened in maybe 20 minutes. The technical execution was flawless - Claude Code knew exactly what to do and used the MCP tools effectively.

**But there was a problem: the game didn't work.**

## The Z-Direction Confusion

When I ran the game, nothing made sense visually. The camera angle was wrong, objects were moving in unexpected directions, and the whole spatial relationship between player, enemies, and the ground was completely broken.

The core issue? **We had fundamental confusion about coordinate systems and camera orientation.**

Here's what went wrong:

**Original Design Assumption:**
- Player moves forward on Z-axis (positive Z)
- Enemies spawn ahead and move backward toward player
- Camera looks down from behind player

**What Actually Happened:**
- Camera angle didn't match the intended isometric view
- Z-direction conventions weren't clear between design doc and implementation
- "Forward" meant different things in different contexts
- The relationship between camera rotation and object movement wasn't properly coordinated

### The Video Problem

One major limitation became apparent: **Claude Code cannot view MP4 files.**

In Day 1, I had extracted reference frames from Total Battle gameplay videos to analyze the mechanics. Claude Desktop was able to view these images and help me understand the game design. But Claude Code, running in the terminal, doesn't have access to visual files at all.

This meant that when spatial issues arose, I couldn't simply show Claude Code what I was seeing or share reference footage. All debugging had to happen through text descriptions and code inspection, which made resolving visual/spatial issues much harder.

## The Refactor Decision

After wrestling with the coordinate system issues for a while, I realized we needed to step back and rethink our approach. The current implementation was built on shaky spatial foundations.

Looking at the DESIGN.md file and the actual gameplay needs, I made a crucial decision: **the player should be stationary, and objects should move toward them.**

This solves multiple problems:

**Simplified Camera:**
- Camera can have a fixed relationship to the player
- No need to constantly adjust camera position
- Clearer sense of "things coming at you"

**Clearer Spatial Model:**
- Player at Z=0 (always)
- Enemies spawn at negative Z (e.g., -15, -20, -25)
- Objects move from negative Z toward positive Z
- When objects reach positive Z (past player), they despawn

**More Intuitive Movement:**
- Objects advance "toward" the player (increasing Z)
- Horizontal movement is purely X-axis (no Z component)
- Camera looks from positive Z toward negative Z
- Creates natural "top to bottom" visual flow on screen

This required a major update to the design document, which I've committed as: **"Design Update: Physical unit representation system"**

## The Physical Unit Representation System

While refactoring the spatial model, I also made another significant design change: **removing the HUD counter in favor of physical unit representation.**

**Old System:**
- HUD displays number (e.g., "Units: 47")
- Collisions modify this number
- Player character is a single object

**New System:**
- No HUD counter for units
- Each unit is a physical small player model (scale 0.3-0.5)
- Player moves as a tight cluster/swarm of units
- Gain units = spawn new player objects in formation
- Lose units = individual player objects are destroyed
- Enemy groups also spawn as clusters of small units

**Why This Is Better:**

1. **Visual Feedback:** You see your army grow and shrink
2. **Visceral Impact:** Watching individual units get destroyed is more emotionally engaging than a number decreasing
3. **Clear State:** Unit count is obvious without UI
4. **Satisfying Growth:** Building up a swarm feels more rewarding
5. **No UI Needed:** One less thing to manage on screen

**Scale Guidelines:**
- Player/Enemy units: Scale 0.3-0.5 (small, many)
- Collectibles (barrels/gates): Scale 1.0-1.5 (medium, noticeable)
- Bosses (future): Scale 2.0-3.0 (large, imposing)

This creates a much more dynamic and engaging visual experience. Instead of a number going "47 → 52 → 45", you see a crowd of units spawning in, clustering together, and then some getting destroyed in combat. Much more satisfying.

## What's in the Commit

The most recent commit (**"Design Update: Physical unit representation system"**) updates DESIGN.md with:

1. **Spatial Model Changes:**
   - Player stationary at Z=0
   - Objects spawn at negative Z, move to positive Z
   - Camera positioned at positive Z looking toward negative Z
   - Top-to-bottom visual flow

2. **Physical Unit System:**
   - Player units as individual small models
   - Tight formation/cluster behavior
   - Spawn/destroy mechanics for all gains/losses
   - Enemy groups also spawn as clusters
   - Scale guidelines for all object types

3. **Implementation Notes:**
   - Updated player movement code
   - Object movement direction corrections
   - Camera setup documentation
   - Formation management pseudocode

The design document now clearly specifies how the spatial model works and how physical units should be managed. This provides a solid foundation for the actual refactor work.

## What's Next

**Immediate Priority: Week 2 Refactor**

Before moving forward with shooting and other Week 2 features, I need to:

1. **Fix the Spatial Model:**
   - Player stationary at Z=0
   - Correct object spawning (negative Z)
   - Proper camera positioning (positive Z)
   - Verify top-to-bottom visual flow

2. **Implement Physical Units:**
   - Replace single player with player manager
   - Spawn small player unit models in formation
   - Implement spawn/destroy for gains/losses
   - Create enemy group spawners
   - Update all collision code

3. **Ground and Positioning:**
   - Ensure all objects sit on ground properly (not floating)
   - Correct any Y-axis positioning issues
   - Verify visual consistency

**Then Week 2 Features:**
- Projectile auto-fire system
- Barrel shoot-to-open mechanic
- Gate charging system
- Multiplier zones

---

## Play Current Build

<div class="game-container" style="text-align: center; margin: 2em 0;">
  <iframe src="/game_clone_blog/builds/day-3/" width="800" height="600" frameborder="0" allowfullscreen></iframe>
  <p><em>Controls: Mouse to move horizontally</em></p>
</div>

> **Note**: First load may take a moment. The game runs in your browser using WebAssembly.
> 
> **What's playable**: Movement exists but spatial relationships are broken. This is a technical "complete" build that demonstrates why proper spatial planning is crucial.
> 
> **Known issues**: Camera angle incorrect, Z-direction confusing, objects don't appear where expected, unplayable due to spatial bugs. This build serves as a lesson in "done" vs "correct."

---

## Lessons Learned

**The Good:**
- Godot MCP server is powerful and works well
- Claude Code can execute technical tasks rapidly
- Having comprehensive design docs is essential

**The Challenging:**
- Spatial/visual bugs are hard to debug without visual inspection
- Claude Code's inability to view media files is a real limitation
- Fast execution doesn't always mean correct execution
- Getting coordinate systems right is critical before building on top of them

**The Surprising:**
- Physical unit representation is actually simpler conceptually
- A "completed" Week 1 can still be completely unplayable
- Sometimes you need to slow down to speed up

---

**Development Time:** 2 hours  
**GitHub Commits:** 3 (Week 1 completion, Documentation update, Refactor plan)  
**Current Status:** Week 1 technically complete, but requires refactor before Week 2

Tomorrow: Fix the spatial model and implement the physical unit representation system properly. Then we can move forward with confidence.
