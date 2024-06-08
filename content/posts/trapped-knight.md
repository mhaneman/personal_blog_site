+++ 
draft = true
date = 2024-05-05T15:58:27-07:00
title = "The Trapped Knight"
description = ""
slug = ""
authors = ["Michael Haneman"]
tags = ["python", "manim", "matplotlib"]
categories = ["Simulation"]
externalLink = ""
series = []
+++

# Introduction

A little while ago while browing through the youtube channel Numberphile. I came across a video about this simulation called "The Trapped Knight". Given some simple rules, some intersting chaotic patterns emerge.  

{{< youtube id=RGQe8waGJ4w&t=240s >}}

Unfortunalty, the video leaves the viewer a little empty. There are a handful of interesting variations that are begging to be simulated. But for now, this post will be about creating the simulation while the next post will explore variations.

# Rules summary
Let's quickly layout the rules descripted in the video. 

1. Suppose an infinitly large chessboard with a point of origin, labled with the numeric value of "zero".
2. From this origin, the rest of the real integers will be labled on the other squares in a spiral shape.
3. The knight will start at the origin
4. The knight will move to the square with the lowest value that has not been visited yet

(insert manim graph)

For generating the spiral shape, two decisions have to be made. Firstly, which square around the origin will contain the value of "one": The top, bottom, left, or right square? Which direction will the spiral be generated: clockwise or counter clockwise? Ultimatly, these decisions are abritrary because changing these conditions will create reflections and rotations in the eventual graph. The shape and logic are not dependent on these decisions. However to match the results found in the video, the square with the value of "one" with start on the square below the origin and the spiral will be produced counter-clockwise. 

# Result

# A Tour of the Code

## Generating the Spiral

## Moving the knight and checking visited squares
The intuative approach to marking visited squares will be to have a list of coordinates. Each time the knight moves to a new square, the coordinate is appended to this list. The problem with this approach is, everytime we check if a square is visited, validating if the value is in this list is an O(n) time complexity and O(n) memory. For example, if the knight is on move 10,000 and the next lowest valid square needs to be found. For each of the eight possible knight moves, we need to check if that specific coordinate exist inside of this list. In total, 80,000 list elements need to be checked. 

So instead, we will modify the value of the square when visited. To mark a visitation, the value of the square will become negative. Therefore for each of the eight squares, we just need to check that it is greater than or equal to zero.

## Determining the lowest valid next square
1. create list of possible squares
2. if list is empty, knight is trapped!
3. sort list by key and pop off list

