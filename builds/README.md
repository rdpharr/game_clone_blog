# Game Builds

This directory contains HTML5 exports of the game at different stages of development.

## Directory Structure

- `day-X/` - Snapshot of the game on day X of development
- `latest/` - Always points to the most recent playable build

## Build Naming Convention

Builds are organized by day number to match blog posts:
- `day-1/` - First working prototype
- `day-2/` - Second day's progress
- etc.

## Embedding in Blog Posts

Each build can be embedded in blog posts using:

```html
<iframe src="/game_clone_blog/builds/day-X/" width="800" height="600" frameborder="0"></iframe>
```

## File Size Management

HTML5 exports can be large (5-20MB). To manage repository size:
- Keep only significant milestone builds
- Use Git LFS for large binary files if needed
- Consider removing very old builds once the project is complete

## How to Add a New Build

1. Export game from Godot as HTML5
2. Create new directory: `builds/day-X/`
3. Copy all exported files into that directory
4. Update `builds/latest/` with the same files
5. Commit and push to GitHub
