# Adventures In AI Coding 2 
Creating [Smash Bors](https://github.com/Vince-0/smash-bors) because I misspelt Bro's.

A continuation from [Adventures In AI Coding](https://github.com/Vince-0/AdventuresInAICoding) where I created [BTCNMAN](https://github.com/Vince-0/btcnman) using [Augment Code](https://www.augmentcode.com/).

## For fun
I saw [ThreeJS](https://threejs.org/) web games from [Pieter](https://x.com/levelsio): [Fly game](fly.pieter.com) and [2025 Vibe Coding Gae Jam](https://jam.pieter.com/). I wanted to try put out the vibe.

This probably took way more time than it should but I had some fun tinkering with it and it sat in a folder for a long while before putting up a demo.

I didn't use any development processes like unit testing, GitHub workflows or design documentation, I just vibed with a [task list](https://github.com/Vince-0/smash-bors/blob/76dd1b421659ffb7766d628d22811e4cd46d2d44/.augment/augment-tasklist).

Augment Code pulled in open assets like:

- 8-bit sound effects from [OpenGameArt.org](https://opengameart.org/content/512-sound-effects-8-bit-style)
- Minecraft skins from [Mojang.com](https://api.mojang.com)
- Tetris style graphics

Try here: [Demo](http://open-stock.bnr.la:3000), fight your friends.

<p align="center">
  <img src="https://github.com/Vince-0/smash-bors/blob/f757e6f2a91481d26d8935fb05aaf19f124f63ce/smash-bors.png" />
</p>

The rest hereunder is written by a GPT-5 agent in Augment Code, makes it sound much better than it is.

# Smash-Bors Dev Journey: From Cubes to Competitive Chaos

Building a real-time, physics-driven multiplayer brawler on the web. What started as a few cubes on a platform grew into animated 3D avatars, finely tuned physics, and a competitive round system that feels responsive, readable, and fun.

---

## Social-Ready Snippets
- “From cubes to combat-ready 3D avatars: real-time WebSocket brawler with CCD, impulse physics, and server authority. Built with Node.js + Three.js 0.175.0.”
- “We prevented avatar tunneling by adding CCD + TOI and tuned restitution/friction for responsive edge battles. No more ghost hits.”
- “Authoritative server + client prediction keeps controls snappy. Snapshot interpolation cleans up the rest. K/D and round flow included.”
- “Particles, trails, and a Tetris x Smash aesthetic—readable chaos at 60fps thanks to pooling and snapshot throttling.”

---

## TL;DR Highlights
- Stack: Node.js + Express + WebSockets + Three.js 0.175.0
- Real-time multiplayer with client prediction + server reconciliation
- 3D physics: gravity, collision detection, bounce, friction, inertia-based motion
- Advanced avatars: from cubes → animated 3D humanoids with skin textures
- Continuous collision detection to prevent tunneling and stuck avatars
- Dynamic platform physics: players can push each other off edges
- Round system: K/D tracking, scoreboard, countdowns, effects, and sounds

---

## Tech Stack and Architecture
- Node.js + Express server for HTTP + static content
- WebSockets for low-latency real-time communication (rooms, events, state sync)
- Three.js 0.175.0 for rendering (migrated from older APIs; updated materials/controls)
- Physics: custom impulse-based solver with gravity, restitution (bounce), and friction
- Server-authoritative simulation with client-side prediction for snappy controls

### Data Flow
- Client inputs (move, jump, crush) → sent via WebSocket with input timestamps
- Server simulates world, resolves collisions, assigns kill credit, and broadcasts snapshots
- Clients reconcile snapshots, correct positions, and smoothly interpolate other players

---

## Real-Time Multiplayer Implementation
- Server tick: deterministic update loop with fixed dt
- Broadcast cadence tuned for bandwidth/latency balance
- Client prediction + reconciliation: stores input history, reapplies unacknowledged inputs after authoritative updates
- Snapshot interpolation for remote players; fallbacks to extrapolation on packet loss
- Lag handling: timestamped inputs, dead-reckoning, and soft-correction to avoid rubber-banding

---

## 3D Physics Engine
- Gravity and jump system with configurable single/double jump velocities
- Inertia-based horizontal movement (acceleration, deceleration, max speed)
- Impulse-based collision resolution with restitution (avatarBounce) and friction (avatarFriction)
- Min-separation handling to avoid clipping and prevent sticky contacts
- Dynamic player-on-player collisions with a tunable push factor for edge battles

### Continuous Collision Detection (CCD)
- Swept-volume tests along velocity vectors to avoid pass-through at high speeds
- Time of Impact (TOI) estimation per tick for precise collision timing
- Sub-stepping on high-velocity frames; corrective integration to prevent tunneling

---

## Advanced Avatar System: From Cubes to Humanoids
- Iteration path: basic cubes → colored capsules → fully rigged humanoid meshes
- Animation state machine tied to physics (idle, run, jump, fall, crush)
- Materials and textures tailored for readability and silhouette clarity
- Optional Minecraft-like skin textures; nameplate labels always visible
- Version migration: updated Three.js 0.175.0 APIs and loaders for consistent animation blending

---

## Dynamic Platform Mechanics
- Ground/platform collision with precise top-surface alignment (no platform interpenetration)
- Player-on-player edge pressure: impulses allow pushing opponents off
- Death zone at the bottom; respawn logic with offset to prevent crowding
- “Crush” action adds vertical pressure with a one-shot trail effect

---

## Scoring, Rounds, and Game Flow
- Server-side scoring: last-collision attribution for kills; separate death tracking
- Persistent scoreboard at top-center (compact layout; K/D ratio shown)
- Round flow: 3-minute rounds, 10-second breaks, 5-second pre-fight countdown
- Feedback layer: explosion effects, larger “FIGHT!” message, end-of-round summaries
- 8-bit sound palette with short global cooldowns to prevent audio spam

---

## Client–Server Sync for Physics Parameters
- Single source of truth: physics config lives server-side
- Clients receive read-only values (gravity, jump, friction, bounce, push factor)
- Prevents desyncs and exploits; enables live tuning between rounds

---

## Performance Optimizations
- Object pooling for repeat particle systems and ephemeral meshes
- Throttled network updates and delta-compressed snapshots
- Culling and LOD for off-screen effects; GPU-friendly materials
- Animation blending tuned to minimize CPU; skeletal updates gated by visibility
- Profiler-led wins: reduced GC, stable 60fps on mid-range hardware

---

## Development Challenges (and How We Solved Them)
1) Avatar collision physics
- Problem: Stuck avatars and jitter on contact; occasional tunneling
- Solution: Min-separation thresholds, CCD with TOI, and impulse clamping; tuned restitution/friction

2) Platform collision + gravity
- Problem: Edge snagging and inconsistent landings at high fps variance
- Solution: Fixed-step integrator, surface-aligned snaps, slope-safe normals, and sub-stepping

3) Client-server synchronization
- Problem: Divergent physics due to floating-point drift and timing
- Solution: Server authority, deterministic ticks, client reconciliation, read-only client configs

4) Real-time performance under load
- Problem: Spikes during crowd fights (particles + animation + network)
- Solution: Batching draw calls, pooling effects, snapshot throttling, and animation gating

---

## Visual and UX Innovations
- Visual progression: geometric primitives → stylized 3D humanoids with animations
- Tetris x Smash aesthetic: bold outlines, punchy colors, and explosive feedback
- Particle bursts and trails on key actions (jump, crush, KOs)
- Minimalist UI with transparent borders; readable nameplates and scoreboard
- Round messaging: “FIGHT!” with explosive flair; “ROUND OVER” with end-of-round breakdown

---

## Iterative Development Timeline (Selected Milestones)
- M1: Core loop with cubes on a flat platform; gravity and simple jump
- M2: WebSocket rooms + authoritative server; initial K/D tracking
- M3: Improved physics with inertia-based movement and friction/bounce
- M4: Continuous collision detection; no more avatar pass-through
- M5: Dynamic platform interactions; push players off edges via impulses
- M6: Visual upgrades: particles, trails, explosion animations, UI pass
- M7: Avatar overhaul: rigged 3D humanoids with animation states and skins
- M8: Performance pass: pooling, snapshot throttling, culling, and profiling

---

## For Developers: Key Implementation Notes
- Use a fixed timestep on the server (e.g., 60Hz) and integrate with sub-steps for high-speed states
- Keep client prediction simple and deterministic; reconcile with gentle position correction
- Attribute kills via last significant collision impulse before a fall
- Separate physics from rendering; keep simulation on server; keep visuals readable
- Ship physics constants from server to clients as read-only config

---

## What’s Next
- Rollback netcode experiments for even tighter competitive play
- Expanded moveset with more readable telegraphs and counters
- Ranked lobbies with MMR and seasonal resets
- Skin pipeline integrations and in-game customization flows
