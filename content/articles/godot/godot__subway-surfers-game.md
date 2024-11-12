+++ 
draft = false
date = 2024-10-15
title = "Building a Subway Surfers Clone in Godot pt.1"
description = "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat."
slug = ""
authors = ["Michael Haneman"]
tags = ["Godot", "Procedural Generation"]
categories = ["Game Dev"]
externalLink = ""
series = []
+++

# Introduction

(This post is a work-in-progress)

Welcome to Part one!

In part 1, we are going to:

1. Download Godot and other Resources
2. Create a Minimal Viable Product (MVP)

In Part 2, we are going to:

1. introduce free audio and 3D assets

I'm going to try giving some instructions in Gherkin Cucumber. It's pretty self explanitory what it is, but heres some documentation if you get stuck or if it's confusing.

[Gherkin Cucumber Tutorial](https://google.com)

## Goals

- create MVP player and player controller
- create blocks manager
- create simple flat platform
- create simple barrel obstacle
- create coin entity

# Theory

Infinite runners like Subway Surfers are unique. They involve a good amount of visual trickery to make the player believe in the movement present on screen. What does this mean?

Firstly, The player doesn't move forward, everything else does. Why?

Having the floor move solves two major problems. The first problem being consistent speed. If the floor moves and the player stays in place, the player will appear to move with an extremely predictable speed. If the player kinematics were responsible for moving the player forward, the player kinematics would have to account for accelerations on slopes and physics collisions with the player would have to be adjusted for both objects.

The player doesn't move left and right, everything else does.
In a game like subway surfers, the player is forced into lanes with integer precsision. Even if the player is outside the lane some float error, eventually the player will start drifting out of the lane. Not only will it look weird, but it's a reliability. The game cant rely on the physics engine to move the player to integer coordinates in a smooth manner. This is expecally true if the player has kinematic collisions with other objects.

The only kinematics the player will have is jumping up and down. There shouldn't be and physics problems related to this and this is the simplest approach virtical movement.

Don't fight the physics engine. If there is movement that needs to be highly controlled and/or precise, try not to implement through the physics engine.

# Gathering Everything needed

## Install Godot

[Godot Offical Site](https://google.com)
If only creating a minimal viable project, continue to Project Setup

## Install Blender

[Blender Offical Site](https://google.com)

## Download Assets

[Kenny Assets - Prototype Kit](https://kenney.nl/assets/prototype-kit)

[Kenny Assets - Mini Characters 1](https://kenney.nl/assets/mini-characters-1)

[Kenny Assets - Train Kit](https://kenney.nl/assets/train-kit)

[Audio Resource](https://google.com)

# Project Setup

## Project Architecture

```
res://
* assets/
** audio/
** 3D/

* game_scene/
** _player/
** blocks/
*** platforms/
*** obstacles/
*** entities/

* start_menu/

* death_menu/
```

As a personal preference, I like to prefix folders with an underscore that contain a scene of a specific entity. When looking through a large project, it signals to me that this is the lowest folder. Folders that don't have the underscore contain more folders with potentially more entities. For example in the \_player folder, the folder is only going to contain the scene file, the script file, and other potential 3D resouces. Thats it. There will not be a "gun" entity inside of the \_player folder. Again, this is purely a preference and not a requirement.

## Assets Folder

Import and load assets before starting the project.
It's a good habit to already have a place / system for importing assets before starting a new project. Importing assets can be kind of a pain in a mid to large sized project.

```cucumber
Given the user has the file explore visible
```

## Create Singleton

create singleton for player attributes (eg. coins, high score, etc ...)

# Create Game Scene

In the left hand panel under "Create Root Node", click "3d Scene"
Save the scene under the res://game/ directory
Attach script to scene

# Creating The Player

## Create a new CharacterBody3D Scene

```cucumber
Given the user has the file explore visible
When the user "right clicked" on "res://game/_player/" folder
And the user "hovered" on "Create New"
And the user "clicked" on "Scene"
Then the "Create New Scene" popup window appeared

When the user sets the name to "Player"
And the user sets the node type to "CharacterBody3D"
Then new scene is created
```

## Attach Script to the new scene

```cucumber
Given the user has the file explore visible
When the user clicks "Add Script" icon
And the user saved script
Then a script is attached to the scene in the same folder
```

## Adding The Player Body

1. add a capsole collision shape to the root node
2. add a capsole mesh to the collision shape

## Adding The Camera

For now, the camera will be glued to the player. The camera will just be an offset of the players coordinates.

1. Add camera as child node
2. Give the camera an offset

## Script

The player script is going to be pretty simple. In fact, its going to be more simple than the default template script.

```gdscript
extends CharacterBody3D
class_name Player

@export var jump_velocity = 4.5


func _physics_process(delta: float) -> void:
	# Add the gravity.
	if not is_on_floor():
		velocity += get_gravity() * delta

	# Handle jump.
	if Input.is_action_just_pressed("ui_accept") and is_on_floor():
		velocity.y = jump_velocity

	move_and_slide()
```

# Minimal Viable Platform Block

Let's quickly create something the player can stand on and something our blocks_manager can spawn

1. in `res://game/blocks/platforms` create a new `static body` Scene
2. add a collision shape to the root Node
3. add a mesh to the collision shape Node
4. set the dimentions to 8x32

# Minimal Viable Entity - Coin

1. in `res://game/blocks/entites` create a new `area3D` Scene
2. create cylander collision shape and rotate
3. add mesh to collision shape
4. attach script to coin
5. signal -> when `Player` body entered, despawn
6. coins++ to global singleton

Note: designing the player class to contain the coin count is also fine.
However, since there is only one entity that can hold 'currency', it will be easier to store has a global variable.
this is expecally true if you want to implement a shop.
Instead of having to reference the player class (assuming its initalized), we only have to call a variable

# Blocks Manager

Create "blocks_manager" scene
All spawned platforms, obsticals, and entites are going to be spawned in the "blocks_manager".
We are going to deligate some of the movement illusions to this scene. More specifically, moving the player left and right, and moving the scene elements towards the player

```cucumber
Given the user has the file explore visible
When the user "right clicked" on "res://game/blocks/" folder
and the user "hovered" on "Create New"
and the user "clicked" on "Scene"
Then the user saw "Create New Scene" popup window
```

# Managing Blocks

To give the illusion of the player moving forwards, each individual platform is going to move in the -z axis at a constant rate

## Spawning and Despawning Objects

Lets have an Area3D node behind the player. Once an object leaves the area, the object despawns and a new one is replaced at the end
