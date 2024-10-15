+++ 
draft = false
date = 2024-10-15
title = "Building a Subway Surfers Clone in Godot"
description = ""
slug = ""
authors = ["Michael Haneman"]
tags = ["Godot"]
categories = ["Game Dev"]
externalLink = ""
series = []
+++

# Introduction

First we are going to create a minimal viable project

then introduce free audio and 3D assets

I'm going to try giving some instructions in Gherkin Cucumber. It's pretty self explanitory to follow, but heres some documentation if you get stuck.
[Gherkin Cucumber Tutorial](https://google.com)

# Goals and Features

1. Create Minimal Viable Project 
- create player controller 
- create block manager 
- create simple flat platform 
- create simple barrel obstacle
- create coin entity

2. Wanted Features 
- foo 

# Theory

The player doesn't move forward, the floor does.
This solves two problems. The first one, the player will move with an extremely predictable speed because its the scene thats moving the player, not the player kinematics. If the player kinematics were responsible for moving the player forward, we will have to account for slow downs on going up slopes, speed ups when going down slopes. We will also have to account for potential physics collisions.

The player also doesn't left and right, the floor does.
This is because the player will be in lanes and we cant rely on the physics engine to move the player to integer coordinates in a smooth manner. This is expecally true if the player has kinematic collisions with other objects.

The only kinematics the player will have is jumping up and down. There shouldn't be and physics problems related to this and this is the simplest approach virtical movement.

Basically, don't fight the physics engine. If theres kinematics that need to be highly controlled and/or precise, try not to implement through the physics engine.

# Gathering Everything needed

## download godot

[Godot Offical Site](https://google.com)
If only creating a minimal viable project, continue to Project Setup

## download blender

[Blender Offical Site](https://google.com)

## download assets

[Kenny Assets](https://google.com)

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


