---
title: 🛞 Reinventing the wheel
published: 2025-08-16
description: "Not everything, don't worry"
image: 'sketch.png'
tags: [Project Marbles]
category: 'Game Dev Log'
draft: false 
lang: ''
---

... And more than _three months_ have passed. How have you been? I know in the last entry I said I'd be writting posts monthly, and I promise I tried, but life happens and sometimes you just can't be at your laptop and in the right state of mind to write about your passions. Or work on them, for that matter.

Anyway, this entry will update you on the current status of the project and explore some of the difficulties I've been facing. And, as always, a bit of random rambling.

# The boat did a one-eighty

We finished the last post more certain about what type of game I wanted: a pinball.
So... hehehe. That has changed. Let's understand the rationale behind it.

From a **technical** standpoint, the current framework (LÖVE) is far more capable of dealing with high-fidelity 2D physics than the last one (Godot), so that was not the problem. The issue I faced was designing interesting **game mechanics** that would encompass what I wanted: a strategic team dynamic with our marbles, against an engagin opponent or obstacle.

With that, I did a light exploration of the physics game world to understand the main characteristics I wanted:

* Ball-throwing would be the main control method for our marbles;
* Different marbles have different abilities to considerably change the gameplay style;
* Enemies to be defeated;

What that reminded me of? Angry Birds.

## The Angry Birds tangent

In case you were living under a rock for the past seventeen years, [Angry Birds](https://www.angrybirds.com/explore/hall-of-games/) is a successful franchise that started in the gaming world and is even now launching its third movie, created by _Jaakko Iisal_ and owned by _Rovio Entertainment Ltd_.

The premise of the first game (back in 2009), which I played the most, is simple: the player has a set of birds, each with its own ability, a slingshot to launch them and a few green pigs behind destructible blocks to hit, either with the winged projectiles or smashing them below their own constructions.

Each level presents a new type or combination of birds, along with new types of blocks and various buildings, making it harder for the player to hit all pigs with the limited amount of available birds. It is a pretty solid experience for the genre of physics-based puzzle. And, as I said in the previous blog entry, it uses the same physics engine LÖVE uses underneath: Box2D.

## Seasonings of my own

As I don't want to copy a game - be it because of legal issues or lack creativity - our marbles will work a bit differently:

* The player will have a set of **round animals** - selected of a random pool - to use in a level;
* Every level will have a different layout with a different set of enemies - in the form of **square marbles** - to defeat in order to clear the level;
* With every passing level, there will be a choice to **upgrade** their current set of marbles or **acquire** a new one;
* A **bigger challenge** will await the player each set of levels;
* Rogue-like elements, like random levels and meta upgrades for each run;

But game design was not the culprit for holding me back this many months. Let's get to the weak spot of our game engine of choice.

# UI elements from scratch

LÖVE has no built-in UI elements whatsoever. All it provides is the ability to show text, images and basic shapes, in a very simple manner, like "Print a filled blue rectangle with this width and height at x and y position". It also provides the ability to detect mouse position via a couple of methods, so we have all the tools to create a working user interface, right?

## The Goal

I wanted a system to create the UI via simple shapes, provided by the existing drawing functions, in order to increase future compatibility with mobile and web exports - possibly - so external modules that depended upon compiled libraries were not taken into account. Also, the target was to have simple yet customizable components to match the style of the game, leaning towards a simple aesthetic, with a few

