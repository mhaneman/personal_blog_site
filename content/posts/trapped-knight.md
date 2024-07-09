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

Not too long ago while browsing, The Youtbe aglorithm blessed me with a Numberphile video called "The Trapped Knight". Given simple rules for how a knight moves on a chess board, some intersting chaotic, patterns emerge.

{{< youtube id=RGQe8waGJ4w >}}

# Rules Summary
1. Suppose an infinitly large chessboard. Give a square a numeric value of 1. This square will be the point of origin. 
2. From this origin, the rest of the real, positive integers will be put on the remaining squares in a spiral shape.
3. The knight will begin its journey at the origin 
4. The knight will continouly move to the square with the lowest value that has not been visited yet

(insert manim graph)

For generating the spiral shape, two decisions have to be made. Firstly, which square from the origin will be visited next: top, bottom, left, or right square? Secondly, which direction will the spiral be generated: clockwise or counter clockwise? Ultimatly, these decisions are abritrary because changing these conditions will only create reflections and rotations in the final graph. The general shape and logic are not dependent on the inital conditions for the spiral. However to match the results found in the video, the first square from the origin will start on the square below and the spiral will continue counter-clockwise. 

# Result

{{< youtube id=ZXOjEcl4wm4 >}}

# A Tour of the Code

## Generating the Spiral

To Generate the spiral, we will keep track of a 2d vector that rotates 90 degrees after it has moved m (the length of the side of the spiral) squares.
The number of squares the vector will move will be m+1 after 2 rotations. 
Technically, the logic to generate the spiral would need to run indefinatly to garentee sufficiency for the simulation. 
But, we will artifically cap the size of the sprial at `spiral_size`. 
If the simulation runs out of squares, the simulation will need to run again with a higher cap size. 

Clearly, artifically capping the spiral size is not ideal. 
What if the simulation has a large number of steps and needs to run for a long time? 
Can't we have an algorithm to determine the number of squares needed? 
This in itself is an interesting statistical problem. 
What area of valued squares are needed to garentee the knight is able to move n times? 
However in practice, the path of the knight tends to tighly wrap around the origin, making this problem unlikely and therefore ignored.

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

We will have a base class called Knight. But why? Why not just implement everything in the knight class? In the future, we may want to create new rules to apply to the knight. So, for each implementation of these rules, the Knight class will be inherited and the abstract method `find_next_square` will be implemented. In this way, each implementation is organized in each class and we will not have to duplicate setup code. 

```python
class Knight:

    def __init__(self, start_pos: tuple = (0,0)) -> None:
        self.curr_pos = start_pos
        self.move_offsets = [(1, 2), (-1, 2), (1, -2), (-1, -2), (2, 1), (-2, 1), (2, -1), (-2, -1)]
        self.pos_history = []
```

Lets implement the rules for finding the next square in a class called `NKnight`. Its called NKnight because n represents the nth lowest value the knight will go to (variations of this simulation will be created ... for future posts). For example, if n=0 and we have the possible square values `[2, 9, 5]`, the knight will go to the square wil value 2. For n=1, the knight will go to the square with value 5. n=3, square with value 9. n=4, since there are only three options, we must take the square with value 9.

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

Another approach would be to create a hashmap of all the visited squares as tuple keys. Therefore lookup will be O(1). 
However, let's save a little extra memory. If a square when visited, the value will be negated. Therefore, for each lookup of the eight squares, we just need to check that it is greater than zero.

For the future variations, it's probably nessisary to keep a hashmap of visited squares. But, this will due for now. 

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
