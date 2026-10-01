# PICO-8 Game Boy Jam Project

## Overview

This project is a retro-style game built entirely in **PICO-8**, a fantasy console designed around the hardware and development constraints of early game consoles.

I created the game for a **Game Boy-themed game jam**, which introduced another major constraint: the entire game could use only **four colors on screen at once**.

Between the Game Boy-inspired visual restrictions and PICO-8's limited cartridge resources, building the game became an exercise in **optimization, system design, and problem solving under strict technical constraints**.

## Why PICO-8?

PICO-8 intentionally gives developers a very small virtual console to work with. Code, sprites, maps, sound, and other resources all have to fit inside a limited cartridge.

Code itself has a **token budget**, so adding another mechanic isn't completely free. As the project grew, I had to continually consider how much code each feature required and whether existing systems could be reused or simplified.

That changed the way I approached development. Instead of asking only:

> "How do I implement this feature?"

I also had to ask:

> "How do I implement this feature with the resources I have left?"

This pushed me toward reusable functions, simpler state management, shared systems, and removing unnecessary logic.

## Designing a Level System That Could Evolve

One of the more interesting engineering challenges came from how I originally planned to store my levels.

My initial idea was to represent each level as a **compact string of bytes/data**. The game could read that representation and reconstruct or print the corresponding level.

Conceptually, the system was simple:

`Level Data → Decode Data → Draw Level`

This was attractive for PICO-8 because it allowed me to represent a large amount of level information compactly without writing separate logic for every scene.

However, the game's central mechanic evolved beyond displaying a static level.

The environment could **change depending on how the player interacted with it**. A scene was no longer necessarily represented by one fixed set of tiles. Player actions could cause parts of the environment to transition into different states.

That meant my original model:

`Level → One Tile Representation`

was no longer sufficient.

I now needed something closer to:

`Level + Player Interaction + Current State → Scene Representation`

This created an interesting design problem. I could have continued adding special cases to the original system, but doing so would have increased complexity and consumed more of PICO-8's limited token budget.

Instead, I had to rethink how level data and game state were represented so that the environment could change dynamically while still remaining compact enough for PICO-8.

This was one of the biggest lessons from the project: **the simplest representation at the beginning of a project is not always the right representation once the requirements evolve.**

## Dynamic Scene Changes

Because the game world changes based on player interaction, I had to separate the idea of the **base level** from the **current state of the level**.

Rather than thinking of the map as something that was simply drawn to the screen, I began treating it as data that could be interpreted differently depending on the state of the game.

This required reasoning about:

- How level data should be represented
- How player interactions modify world state
- How different scene states should be stored
- How to avoid duplicating entire maps
- How to keep transitions consistent
- How to implement the system without exceeding PICO-8's resource limits

The result was not just a gameplay problem—it became a **data representation and architecture problem**.

## Optimization Under Constraints

As the game became more complicated, I had to continually optimize how systems were implemented.

Some of the techniques I used included:

- Reusing functions instead of duplicating behavior
- Consolidating repeated logic
- Representing game information as data instead of hardcoding every case
- Simplifying conditionals and state transitions
- Reusing map and sprite resources
- Evaluating features based on their token cost
- Refactoring systems when the original architecture no longer supported the game's requirements

PICO-8 made optimization something I had to consider **during development rather than after development**.

## Visual Constraints

The Game Boy jam also restricted the game to **four simultaneous colors**.

That meant the same four-color palette had to communicate the environment, player, objects, UI, and different gameplay states.

I couldn't solve readability problems by simply adding another color. Instead, I had to use contrast, shapes, animation, tile changes, and composition to communicate state to the player.

This turned an artistic restriction into another engineering and design problem.

## Problem-Solving Takeaways

The most valuable part of this project was repeatedly encountering situations where my first implementation was no longer sufficient.

Instead of simply adding more code, I had to step back and ask:

**What information does the game actually need to store?**

**What can be generated instead of stored?**

**What systems can share logic?**

**How can I represent multiple world states without duplicating everything?**

**Is this feature worth its token and cartridge cost?**


Ultimately, working in PICO-8 forced me to approach the game less like an unlimited prototype and more like a **resource-constrained software system where architecture and implementation decisions had measurable costs**.
