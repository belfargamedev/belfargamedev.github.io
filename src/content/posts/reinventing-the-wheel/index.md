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

From a **technical** standpoint, the current framework (LÖVE) is far more capable of dealing with high-fidelity 2D physics than the last one (Godot), so that was not the problem. The issue I faced was designing interesting **game mechanics** that would encompass what I wanted: a strategic team dynamic with our marbles, against an engaging opponent or obstacle.

With that, I did a light exploration of the physics game world to understand the main characteristics I wanted, which are:

* Ball-throwing is the main control method for our marbles;
* Different marbles have different enough abilities to considerably change the gameplay style;
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
* **Rogue-like** elements, like random levels and meta upgrades for each run;

But game design was not the culprit for holding me back this many months. Let's get to the weak spot of our game engine of choice: the **user interface**.

# UI elements from scratch

LÖVE has no built-in UI elements whatsoever. All it provides is the ability to show text, images and basic shapes, in a very simple manner, like "Print a filled blue rectangle with this width and height at x and y position". It also provides the ability to detect mouse position via a couple of methods, so we have all the tools to create a working user interface, right?

Yes, but the system will be built from the ground up. Which can be a good thing, as I've always struggled a bit with the Control nodes in Godot Engine when it comes to resizing. It was _almost_ **never** the way I wanted, with a good scaling and mantaining its proportions.

I want a system to create the UI via simple shapes, provided by the existing drawing functions, in order to increase future compatibility with mobile and web exports. Inspired by the HTML capabilities, padding and margins would be customizable, with rounded borders and even shadow to imitate neumorphism components in the near future. Everything packed in a declarative way to be able to reuse components when needed, lightly inspired in React components.

## In theory, practice is something else entirely

In short, turns out that it is quite a challenge to create a user interface library from scratch, but I'm liking the process so far. The system is, at this moment, able to show colored boxes in various manners, with primitive tiling capability, splitting its content into pages. I promise it makes sense, check it out:

![Prototype UI](prototype_ui.jpg "Prototype UI")

In the image above, the dark box has a tiling view of - the first page - the red element to the left and a grey element to the right with a view of - the second page - of the pink element. The blue element is the first page of the grey element and the light blue one is in the second page of the dark box.

Their visibility is only on now to validate if the spacing is being done correctly, they will be cut off from the view of the components with the scissors functions, to crop what is displayed in a portion of the screen.

And that's about it.

> Actually, no, there is another tangent.

## Design Styles

This year, Apple has launched a new design for their systems, [Liquid Glass](https://www.apple.com/br/newsroom/2025/06/apple-introduces-a-delightful-and-elegant-new-software-design/), and - as the giant it is - should influence the direction overall design heads toward.

At first, I thought Liquid Glass was a not so great change. The transparency with blur and distortion makes the content below weirdly present yet unable to be read. But with each passing day of watching videos reviewing their presentation, more it grew in me, and I'll explain why - but we have to go to a bit of the early history of design. And it all started with **skeumorphism**.

### Skeumorphism

When smartphones were launched, its tech was still alien to the average user: people had phones with small screens and big keyboards, not a single slab of screen. This big screen paved the way for the smartphone to become a swiss-army knife of tools.

### Flat Design

![History of Instagram Logo](https://cdn.logojoy.com/wp-content/uploads/20230511115846/Instagram-logo-evolution-infographic.jpg)
> You can check the source of this image and a much more comprehensive analysis in https://looka.com/blog/instagram-logo/ 
