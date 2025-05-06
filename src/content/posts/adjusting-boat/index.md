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

> Don't mind the _**love**ly_ spoiler

It's been a while, huh? The weekly progress update I wanted is turning out to be monthly. And that's ok, if we keep rowing forward. [Row, row row!](https://www.youtube.com/watch?v=_Mvx4X78sqk)

# The iceberg

Not wanting to come across as pedantic, but if I were living the hero's journey, this would be the first challenge our hero (me) faced into the unkown. And what is the challenge, you ask? Let me show you:

![Sandbox pinball with flying ape](flying_ape.gif "Sandbox pinball with flying ape")

Yeah, let's unpack them.

## One small step for and ape...

This is the blindingly obvious issue, that is, the Ape reaches [Mach 1](https://en.wikipedia.org/wiki/Mach_number) and crosses what was suppose to be the upper limit of the pinball arena. The funny thing about this bug is that it only happened when I was recording to show the current status to a few friends, I could not reproduce after a few minutes trying.

## Junky flipper pushes

If you excuse the awful compression of the gif, you may be able to see that the flippers - although working at a first glance - moves a little ahead of the ball. That is, it first goes to the position and, between the physics frames, pushes the ball along its trajectory, making its movement lag behind the actual push.

## The setup

Explaining a bit about Physics Engine in the scope of game development, there are three main actors for a physics simulation:

1. **Static Body:** an object that does **not** move, used for floors non-breakable buildings or objects (in our example, it would be the walls of the pinball)
2. **Dynamic or Rigid Body:** an object that moves and collides with all other objects in the physics world **via the simulation** of forces (the pinball ball)
3. **Kinematic Body:** an object that does not react to external forces, pushes dynamic bodies around is moved via some **programming logic** (the flippers)
