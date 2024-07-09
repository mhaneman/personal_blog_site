+++ 
draft = false
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

A little while ago while browing through the youtube channel Numberphile, I came across a video about this simulation called "The Trapped Knight". Given some simple rules, some intersting chaotic patterns emerge.

Feel free to watch before continuing, but the rules will be summarized below. 

{{< youtube id=RGQe8waGJ4w >}}

# Rules summary
Let's quickly layout the rules descripted in the video. 

1. Suppose an infinitly large chessboard with a point of origin, labled with the numeric value of "zero".
2. From this origin, the rest of the real integers will be labled on the other squares in a spiral shape.
3. The knight will start at the origin
4. The knight will move to the square with the lowest value that has not been visited yet

(insert manim graph)

For generating the spiral shape, two decisions have to be made. Firstly, which square around the origin will contain the value of "one": The top, bottom, left, or right square? Which direction will the spiral be generated: clockwise or counter clockwise? Ultimatly, these decisions are abritrary because changing these conditions will create reflections and rotations in the eventual graph. The shape and logic are not dependent on these decisions. However to match the results found in the video, the square with the value of "one" with start on the square below the origin and the spiral will be produced counter-clockwise. 

# Result

{{< youtube id=ZXOjEcl4wm4 >}}

# A Tour of the Code

## Generating the Spiral

To Generate the spiral, we will keep track of a 2d vector that rotates 90 degrees after it has moved m squares.
The number of squares the vector will move will be m+1 after 2 rotations. 
Technically, we could need an infinite number of values squares to run this simulation. But, we will artifically cap the size of the sprial at `spiral_size`. 
```python
def gen_spiral(self, spiral_size, negate_primes):
        square_length = 1
        dir = [0, 1]
        current_square = [0, 0]
        current_value = 0

        for _ in range(0, spiral_size):
            for _ in range(0, 2):
                for _ in range(0, square_length):
                    self.coord_vals[tuple(current_square)] = current_value
                    current_square = [sum(i) for i in zip(current_square, dir)]
                    current_value += 1
                dir = self.rot_vec2_90(dir)
            square_length += 1

```

## Moving the knight and checking visited squares

We will have a base class called Knight. But why? Why not just implement everything in the knight class? In the future, we may want to create new rules to apply to the knight. So, for each implementation of these rules, the Knight class will be inherited and the abstract method `find_next_square` will be implemented. In this way, each implementation is organized in each class and we will not have to duplicate the basic code for the knight. 

```python
class Knight:

    def __init__(self, start_pos: tuple = (0,0)) -> None:
        self.curr_pos = start_pos
        self.move_offsets = [(1, 2), (-1, 2), (1, -2), (-1, -2), (2, 1), (-2, 1), (2, -1), (-2, -1)]
        self.pos_history = []
```

Lets implement the rules for finding the next square in a class called `NKnight`. Its called NKnight because n represents the nth lowest value the knight will go to. For example, if n=0 and we have the possible square values `[2, 9, 5]`, the knight will go to the square wil value 2. For n=1, the knight will go to the square with value 5. n=3, square with value 9. n=4, since there are only three options, we must take the square with value 9.

```python
class NKnight(Knight):
    def __init__(self, n=0, start_pos: tuple = (0,0)) -> None:
        Knight.__init__(self, start_pos)
        self.n = n

    def find_next_square(self, coord_vals: Dict[tuple, int])-> Optional[tuple]:
    ...
```

First, lets collect all the possible squares the knight can go to.

The intuative approach to marking visited squares will be to have a list of coordinates. Each time the knight moves to a new square, the coordinate is appended to this list. The problem with this approach is, everytime we check if a square is visited, validating if the value is in this list is an O(n) time complexity and O(n) memory. For example, if the knight is on move 10,000 and the next lowest valid square needs to be found. For each of the eight possible knight moves, we need to check if that specific coordinate exist inside of this list. In total, 80,000 list elements need to be checked. 

So instead, we will modify the value of the square when visited. To mark a visitation, the value of the square will become negative. Therefore for each of the eight squares, we just need to check that it is greater than or equal to zero.


```python
    def find_next_square(self, coord_vals: Dict[tuple, int])-> Optional[tuple]:
        visited = []
        
        for offset in self.move_offsets:
            pot_pos = tuple(map(lambda i, j: i + j, offset, self.curr_pos))
            pot_val = coord_vals[pot_pos]

            if pot_val > 0:
                visited.append((pot_pos, pot_val))

        if len(visited) == 0:
            return None

```

This is the logic for calculating each step of the knight moves. Notice how we will return False when the knight is trapped.
```python
def calc_move(self) -> bool:
        next_pos = self.knight.find_next_square(self.board.coord_vals)
        
        if next_pos is None:
            return False

        self.board.coord_vals[next_pos] *= -1 # update board
        return True
```

Next, lets sort the array of avaliable squares by value. We want the lowest value squares so that way we can treat this list like a stack an pop off the lowest value form the end.

```python
        visited.sort(reverse=True, key=lambda e: e[1])

        for _ in range(self.n):
            if (len(visited) == 1):
                break
            visited.pop()

        result = visited.pop()[0]
        self.curr_pos = result
        self.pos_history.append(result)
        return result
```

Link to source code on github:
https://github.com/mhaneman/trapped-knight/tree/main
