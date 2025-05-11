---
title: Adjusting the course of the boat
published: 2025-05-06
description: "A tiny vessel in a sea of problems"
image: 'love_gh_banner.png'
tags: [Project Marbles]
category: 'Game Dev Log'
draft: false
lang: ''
---

> Don't mind the _**love**ly_ spoiler.

It's been a while, huh? The weekly progress update I wanted is turning out to be monthly. And that's ok, if we keep rowing forward. [Row, row row!](https://www.youtube.com/watch?v=_Mvx4X78sqk)

In this entry we will be covering the issue I found during the developement of the Godot demo, some alternative physics engines and even how floating number precision works. Fasten your seatbelts, it's going to be a ride.

# The iceberg

If i were to use some metaphor about piloting a boat, I´d be Nami from One Piece having no idea on how to navigate in the Grand Line at first. Except I'm no expert in game development, even so in physics engines.

But indeed I've indeed stumbled across a couple of issues in the default 2D physics system implemented in Godot we need to unpack. Let me show you a quick gif:

![Sandbox pinball with flying ape](flying_ape.gif "Sandbox pinball with flying ape")

## One small step for and ape...

This is the blindingly obvious issue, that is, once the Ape reaches [Mach 1](https://en.wikipedia.org/wiki/Mach_number) and crosses what was suppose to be the upper limit of the pinball arena. The funny thing about this bug is that it only happened once, when I was recording to show the current status to a few friends. I could not reproduce after a few minutes trying. That is the importance of multiple testers, my dear reader.

## Junky flipper pushes

If you excuse the awful compression of the gif and ignore the unintended VHS effect on the background, you may be able to see that the flippers - although working at a first glance - moves a little ahead of the ball. That is, it first goes to the end position and, between the physics frames, pushes the ball along its trajectory, making its movement lag behind the actual push.

## The setup

Explaining a bit about Physics Engine in the scope of game development in general, there are usually three main actors for a physics simulation:

1. **Static Body:** an object that **does not** move at all, used for floors, non-breakable buildings or non-movable objects (in our example, it would be the walls of the pinball)
2. **Dynamic/Rigid Body:** an object that moves and collides with all other objects in the physics world **via the simulation** of forces and momentum (the pinball ball)
3. **Kinematic Body:** an object that does not react to external forces, albeit pushing dynamic bodies around and is moved via some **programming logic** (the flippers)

And how was the ball able to trespass the ceiling of the arena? I have a few ideas, and onde of them may be my fault, but baby steps.

### Walls with duct tape

How the walls were builded is important. Each square block is a different static body, placed each aside another in what was supposed to be a pixel perfect combination of collision polygons, making the trespassing impossible. But the fact is, I did not build a single polygon, there is non-nullable gap between the walls.

### Movement before collision check

Here is where I deduct, based on my shallow knowledge on how physics simulation works, what happened to the ball clip through the wall:
1. The ball casts a ray on the direction of its velocity to check for any obstacle before its movement...
2. ... but it targets perfectly the void between the walls. So it finds no obstacle and continue moving.
3. Once it moves inside the gap between the walls, its sphere detects collision with the walls and  - as it can't be inside another object - its plunges itself upwards to respect the laws of physics and not let two bodies occupy the same space at the same time.
4. A new astronaut ape is launched into the stratosphere.

## Choices

So, I love Godot, but it seems the default behavior for this scenario is not good. Our an easy fix would do and could be a skill issue of mine. Nevertheless, I was not satisfied with the result.

With the release of [Godot 4.4](https://godotengine.org/releases/4.4/), came an alternative physics engine to the custom one implemented by Godot, [Jolt Physics](https://github.com/jrouwe/JoltPhysics), but it is 3D only. That lead to me question, what were the other physics engine alternatives, not necessarily for Godot, but for other game engines as well.

* [Unity](https://docs.unity3d.com/Manual/PhysicsSection.html): Uses Nvidia PhysX for 3D and Box2D for 2D.
* [Game Maker](https://manual.gamemaker.io/monthly/en/GameMaker_Language/GML_Reference/Physics/Physics.htm): Uses Box2D.
* [Defold](https://defold.com/manuals/physics/): Uses Bullet Physics for 3D and Box2D for 2D.
* [LÖVE](https://love2d.org/wiki/love.physics): Uses Box2D.
* [Befy](https://bevyengine.org/): Has no embedded physics engine but two of the most famous are [Avian](https://github.com/Jondolf/avian) and [Rapier plugin](https://github.com/dimforge/bevy_rapier).

I refuse, as a true aquarian (I'm born in September 7th), to give in and use Unity, a popular game engine choice, with a lot of assets ready to buy and use. But I don't like things the easy way.

Game Maker is a cool engine, but it is too much GUI-oriented. I'd get irritated to work on a large project with it.

With two options out of the way, we go to the fun options, where I get the opportunity to learn new languages:
* Rust: if I choose Bevy.
* Lua: if I choose Defold or LÖVE.

### Bevy

I started learning Rust intermittently since 2023, and was not able to use it in a large project. But it is a language in my radar to learn for the future. And that future would be now, if Bevy was not so new in the game engine market, as it's still in early development stage and doesn't even have a built-in physics engine.
