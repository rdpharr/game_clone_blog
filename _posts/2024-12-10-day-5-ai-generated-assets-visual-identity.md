---
layout: post
title: "Day 5 - 3 Hours - AI-Generated Assets & Visual Identity"
date: 2024-12-10
categories: [development, art]
tags: [godot, pixellab, ai-art, visual-design, assets]
---

## Progress Summary

The game was mechanically solid after Day 4's projectile combat implementation, but visually it felt... generic. Tiny 16x16 pixel sprites, roguelike dungeon crawler aesthetic, everything too small to appreciate. It worked, but it didn't have *personality*.

So I spent 3 hours creating a complete visual identity using AI-generated pixel art, and the transformation is striking. Same mechanics, completely different feel. The game finally looks as good as it plays.

**Time Spent:** 3 hours  
**Major Accomplishments:**
- Created 8 custom AI-generated pixel art assets using Pixellab
- Complete visual overhaul from roguelike to cohesive fantasy theme  
- Doubled sprite sizes (16x16 → 32x32 units, 32x32 → 64x64 collectibles)
- Enhanced projectile visibility and game feel
- Movement and background polish

**Milestone:** Game now has distinct visual identity - no longer "too small and roguelike"

---

## Play Current Build

<div class="game-container" style="text-align: center; margin: 2em 0;">
  <iframe src="/game_clone_blog/builds/day-5/" width="800" height="600" frameborder="0" allowfullscreen></iframe>
  <p><em>Controls: Mouse to move horizontally | Projectiles auto-fire</em></p>
</div>

> **Note**: First load may take a moment. The game runs in your browser using WebAssembly.
> 
> **What's new**: Complete visual overhaul! Custom AI-generated fantasy-themed pixel art with 2x larger sprites, cohesive color palette, and distinct character designs. Same mechanics from Day 4, but now with personality.
> 
> **Notice the difference**: 
> - Blue knight player units vs purple enemy creatures
> - Large, detailed wooden barrels and gates
> - Bright orange projectiles that pop visually
> - Stone pathway with grass and tree borders
> - Everything is bigger, clearer, more readable

---

## The Roguelike Problem

Yesterday's build was a huge mechanical success - Week 2 features complete, full projectile combat working, genuinely playable. But when I played it for more than a few minutes, something felt off.

**The issues:**
- **Too small:** 16x16 pixel sprites felt cramped on a 1920x1080 screen
- **Generic aesthetic:** Kenney assets are great for prototyping but lack personality
- **Roguelike vibe:** Everything had that dungeon crawler feel - brown, grey, generic
- **No visual cohesion:** Sprites didn't feel like they belonged together
- **Hard to appreciate:** Unit swarms were just tiny colored squares

The game worked, but it didn't *feel* special. It needed its own identity.

## Enter Pixellab AI

I'd been avoiding custom art because I can't draw and didn't want to spend weeks learning pixel art. But then I discovered [Pixellab](https://pixellab.ai/) - an AI pixel art generator with a free tier.

**Why this was perfect:**
- Still maintains my zero-budget constraint
- Generate custom sprites with text prompts
- Get exactly the aesthetic I want
- Professional-quality pixel art in minutes
- Full commercial rights to generated assets

I could describe what I wanted and get usable sprites immediately. No art skills required.

## The Asset Creation Process

I started with the most important sprites - the player and enemy units - and built the rest of the visual theme around them.

### Player Unit (32x32)

![Player Unit](https://raw.githubusercontent.com/rdpharr/3d-runner-game/main/assets/pixellab/player.png)

**Prompt:** *"Blue knight warrior character, pixel art, 32x32, game sprite, medieval fantasy, simple design, front-facing"*

The blue knight became the visual anchor for the entire game. Clean, readable, distinct. This established the medieval fantasy theme.

### Enemy Unit (32x32)

![Enemy Unit](https://raw.githubusercontent.com/rdpharr/3d-runner-game/main/assets/pixellab/enemy.png)

**Prompt:** *"Purple monster creature, pixel art, 32x32, game sprite, enemy character, menacing but simple, front-facing"*

Purple/magenta creates strong visual contrast with the blue player units. Immediately readable as hostile even in swarms.

### Barrel (64x64)

![Barrel](https://raw.githubusercontent.com/rdpharr/3d-runner-game/main/assets/pixellab/barrel.png)

**Prompt:** *"Wooden barrel with metal bands, pixel art, 64x64, game collectible, medieval fantasy, detailed, top-down view"*

Classic fantasy barrel design. Large enough to display value labels clearly. Fits the medieval theme perfectly.

### Gate (64x64)

![Gate](https://raw.githubusercontent.com/rdpharr/3d-runner-game/main/assets/pixellab/gate.png)

**Prompt:** *"Wooden gate archway with stone pillars, pixel art, 64x64, medieval fantasy, game obstacle, top-down view"*

Three-tile gate structure that spans the playable area. Stone and wood materials match the overall theme.

### Projectile (16x16)

![Projectile](https://raw.githubusercontent.com/rdpharr/3d-runner-game/main/assets/pixellab/projectile.png)

**Prompt:** *"Orange energy orb, pixel art, 16x16, game projectile, glowing effect, simple bright magic spell"*

Bright orange/yellow for maximum visibility. Small but distinct. Pops against the browns and greens of the environment.

### Ground Tiles (128x128)

![Ground](https://raw.githubusercontent.com/rdpharr/3d-runner-game/main/assets/pixellab/ground.png)

**Prompt:** *"Stone pathway cobblestones, pixel art, 128x128, tileable texture, medieval fantasy, top-down, warm earth tones"*

Seamlessly tiling stone pathway. Defines the playable area with warm browns that complement the overall palette.

### Grass (16x16)

![Grass](https://raw.githubusercontent.com/rdpharr/3d-runner-game/main/assets/pixellab/grass.png)

**Prompt:** *"Green grass tuft, pixel art, 16x16, simple ground decoration, bright green, tileable"*

Small grass tufts for the border areas. Frames the playable path with natural greenery.

### Tree (16x16)

![Tree](https://raw.githubusercontent.com/rdpharr/3d-runner-game/main/assets/pixellab/tree.png)

**Prompt:** *"Small tree or bush, pixel art, 16x16, simple stylized, medieval fantasy, green foliage, border decoration"*

Environmental borders that add depth to the edges. Creates a "path through the forest" feeling.

## Visual Cohesion Achieved

With all 8 assets generated, the game suddenly had a **consistent visual identity**:

**Color Palette:**
- Blues (player, sky/water)
- Purples (enemies, danger)
- Browns (wood structures, earth)
- Greens (grass, trees, nature)
- Orange (projectiles, magic)

**Theme:** Medieval fantasy path/runner - a blue knight charging through an enemy-filled forest path

**Style:** Clean pixel art with clear outlines, readable at all sizes, consistent level of detail

**Size Hierarchy:**
- Projectiles: 16x16 (smallest)
- Background tiles: 16x16 (grass/trees) to 128x128 (ground)
- Units: 32x32 (readable swarms)
- Collectibles: 64x64 (clear targets)

Everything feels like it belongs in the same game world now.

## The Migration

Moving from Kenney to Pixellab assets was straightforward:

**1. Created directory structure:**
```
assets/
  pixellab/
    player.png
    enemy.png
    barrel.png
    gate.png
    projectile.png
    ground.png
    grass.png
    tree.png
```

**2. Imported into Godot:**
- Dragged all PNGs into Godot project
- Godot auto-generated .import files
- Set proper import settings (filter off for pixel art)

**3. Updated scene references:**
- PlayerUnit scene: Kenney sprite → Pixellab player.png
- EnemyUnit scene: Kenney sprite → Pixellab enemy.png
- Barrel scene: Kenney sprite → Pixellab barrel.png
- Gate scene: Kenney sprite → Pixellab gate.png
- Projectile scene: Kenney sprite → Pixellab projectile.png
- Background: New ground/grass/tree textures

**4. Adjusted scales:**
- Units: 2x scale (16x16 source → 32x32 displayed)
- Collectibles: 1x scale (already 64x64)
- Background tiles: Various scales for proper tiling

Total migration time: ~30 minutes. Most time was in Pixellab generating the assets.

## Visual Polish Pass

With new assets in place, I refined the overall game feel:

### Enhanced Projectile Visibility

The original projectiles were tiny yellow dots. Hard to track, especially with 30+ on screen. The new orange orbs are:
- Larger (16x16 vs 8x8)
- Brighter (orange vs pale yellow)
- More distinct shape (orb vs dot)
- Higher contrast against background

**Result:** Much easier to see your firepower, more satisfying combat feel.

### Movement Refinements

Updated player movement to feel more responsive with the new, larger sprites:

```gdscript
const HORIZONTAL_SPEED := 80.0  # Increased from 60
```

The bigger sprites meant movement needed to feel snappier to match the visual scale.

### Background Scrolling

Adjusted scroll speed to work with the new ground tiles:

```gdscript
const SCROLL_SPEED := 80.0  # Tuned for new tile sizes
```

The stone pathway tiles create a better sense of forward movement than the previous generic ground.

### Border Framing

Added grass and tree borders at x = ±332 pixels:
- Grass fills the outer areas
- Trees mark the exact playable boundary
- Creates "path through forest" aesthetic
- Helps players understand the play area visually

## Before vs. After

**Before (Kenney Assets):**
- 16x16 pixel units (tiny on screen)
- Generic roguelike dungeon aesthetic
- Brown/grey color palette
- Sprites didn't feel cohesive
- Hard to appreciate unit swarms
- Functional but bland

**After (Pixellab Assets):**
- 32x32 pixel units (2x larger, readable)
- Medieval fantasy forest runner theme
- Rich blue/purple/brown/green palette
- All assets feel like one game world
- Unit swarms have personality
- Visually distinctive and appealing

**Same mechanics, completely different experience.**

The game still plays exactly the same - same collision detection, same combat systems, same projectile behavior. But now it *looks* like a game worth playing.

## The Power of AI Asset Generation

This experience completely changed my view on AI art tools for game development:

**Accessibility:**
- No pixel art skills required
- No weeks learning to draw
- Professional results immediately
- Free tier sufficient for small projects

**Iteration Speed:**
- Don't like a sprite? Regenerate in 30 seconds
- Try different prompts until it's right
- Adjust colors/styles easily
- Much faster than commissioning art

**Customization:**
- Get exactly what you envision
- Maintain consistent style across assets
- Create unique visual identity
- Not limited to existing asset packs

**Budget-Friendly:**
- Free tier gave me everything I needed
- Would have cost $100+ to commission
- Maintains zero-budget constraint
- Can upgrade if project grows

**Rights:**
- Full commercial usage rights
- No attribution required
- Can modify freely
- Own the generated assets

For solo developers and learning projects, AI pixel art generation is a game-changer. It removes the art barrier completely.

## Prompt Engineering Lessons

Getting good results required some trial and error with prompts:

**What worked:**
- Specific size requests (32x32, 64x64)
- Style keywords ("pixel art", "game sprite")
- Theme keywords ("medieval fantasy")
- View angle ("front-facing", "top-down")
- Simplicity guidance ("simple design", "clean")

**What didn't work:**
- Too vague ("make a character")
- Too complex ("armored knight with sword and shield standing on stone path")
- Conflicting styles ("realistic medieval pixel art")
- No size specification (got inconsistent results)

**Best approach:**
1. Start simple and specific
2. Generate multiple options
3. Refine prompt based on results
4. Be explicit about technical needs (size, view angle)
5. Iterate until satisfied

Most sprites took 2-3 generations to get right.

## Performance Impact

Adding larger, more detailed sprites had minimal performance impact:

- Still solid 60 FPS
- No increase in memory usage (small PNGs)
- Godot handles sprite scaling efficiently
- Texture atlas keeps draw calls low

The benefits far outweigh any technical cost.

## What This Means for the Project

**Before today:** Mechanically complete, visually generic prototype  
**After today:** Mechanically complete, visually distinctive game

The visual identity gives the project:
- **Clearer direction:** Medieval fantasy runner theme is established
- **Better testing:** More enjoyable to play = more testing time
- **Showcase potential:** Can share screenshots/videos without embarrassment
- **Motivation boost:** Looks professional, feels like a real project
- **Foundation for polish:** Future particle effects and UI can match the theme

This was time well spent. The game finally looks like something I'd want to show people.

## Lessons Learned

**1. Visual Identity Matters Early**  
I waited until Day 5 to address aesthetics. Could have done this on Day 2-3 and been playtesting with better visuals all along.

**2. AI Tools Democratize Game Art**  
You don't need to be an artist anymore. You need to be able to describe what you want. That's a much lower barrier.

**3. Consistent Theme > Quality Assets**  
8 mediocre sprites that fit together > 8 amazing sprites that clash. Theme and cohesion trump individual quality.

**4. Size Matters for Readability**  
32x32 is the sweet spot for unit sprites in this game. 16x16 was too small. Bigger isn't always better, but small enough to be unclear is always worse.

**5. Good Art Improves Game Feel**  
The exact same mechanics feel more satisfying with better visuals. Polish isn't just surface-level - it affects the core experience.

**6. Free Tier Is Sufficient**  
Pixellab's free tier gave me everything I needed. No need to pay for a learning project. Can always upgrade later if needed.

**7. Iteration Is Key**  
Most sprites needed 2-3 tries. That's fine - iteration is fast with AI generation. Don't settle for first results.

## What's Next

With both mechanics (Week 2) and visuals (Day 5) solid, I have options:

**Option A: Week 3 Features**
- Multiplier zones (2x, 3x, 5x value regions)
- Difficulty scaling (speed increases)
- Unobtainable gates (negative traps)
- Particle effects on collisions
- Sound effects

**Option B: Content Variety**
- Design multiple stage layouts
- Vary enemy spawn patterns
- Create different barrel/gate distributions
- Test difficulty curves
- Build replayability

**Option C: Polish & UX**
- Main menu system
- Game over / restart flow
- Score display
- High score tracking
- Tutorial elements

I'm leaning toward **Option A** - the core loop is fun, adding multipliers and difficulty scaling will make it more engaging. But we'll see what feels right tomorrow.

The foundation is rock solid now - good mechanics AND good visuals.

## Time Breakdown

- Pixellab asset generation: 90 min
  - Player/Enemy units: 30 min (many iterations)
  - Barrels/Gates: 25 min
  - Projectile/Background: 35 min
- Asset migration & scene updates: 30 min
- Movement/background polish: 45 min
- Testing & iteration: 15 min

**Total:** ~3 hours

---

**Development Time:** 3 hours  
**GitHub Commits:** 3 total
- Session 4: UI/UX Polish & Gameplay Refinements Complete (Dec 9 evening)
- Asset Migration: Pixellab pack + Movement/Background updates (Dec 10)
- Visual Polish: Enhanced projectile visibility (Dec 10)

**Current Status:** Week 2 mechanics complete, visual identity established, game looks professional

Tomorrow: Week 3 features (multipliers, difficulty, effects) or content variety. The game is ready for expansion.
