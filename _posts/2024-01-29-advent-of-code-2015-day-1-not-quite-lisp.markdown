---
title: "Solving Advent of Code 2015, Day 1: Not Quite Lisp"
categories: [Advent of Code, Python]
tags: [Problem Solving, Python, Algorithms, Data Structures]
date: 2024-02-11 15:00:00 -0500
math: false
mermaid: false
pin: false
img_path: /assets/img/posts/advent-of-code/2015/
image: day-1-not-quite-lisp.webp
---

Since I discovered [Advent of Code](https://adventofcode.com/2023/events), I've been determined to solve all its puzzles for each year. In this blog series, I'll not only provide the solutions but also brief explanations with it's code. Kick-starting with the 2015 Advent of Code event, let's start with the [Day 1: Not Quite Lisp](https://adventofcode.com/2015/day/1).

## Handling the input

To simplify the implementation for both parts, let's create a function called `read_input` that reads the input file and converts it into a convenient data structure. Since the input solution for this day consists only of a single line of characters, the `read_input` function will look as follows:

```python
def read_input(file):
    s = []
    with open(file) as f:
        for c in f.read():
            s.append(c)
    return s
```

## Part one

Our puzzle input will consist of a string of parentheses. An opening parenthesis, `(`, means Santa should move up one floor, while a closing parenthesis, `)`, indicates he should move down one floor. To determine the floor where the instructions lead Santa, follow these simple steps: iterate over the string of parentheses and add `1` to a variable if the current character is `(`. And, subtract `1` when encountering a `)`. I've created a dictionary mapping `(`, `)` to `1` and `-1` respectively. Thus, the part's one solution is:

```python
def solve(input):
    instruction = {'(': 1, ')': -1}
    sum = 0
    pos = 0
    for i, c in enumerate(input):
        sum += instruction[c]
    return sum
```

## Part two

Given the same instructions, we are now tasked with finding the character position that causes Santa to enter the basement for the first time. To accomplish this, we'll create a new variable named `pos` (for position). Keep in mind that when the floor position becomes `-1`, we should add the subsequent index `i` to `pos`. By doing so, it's simple to combine the solution for both parts. The part two solution is:

```python
def solve(input):
    instruction = {'(': 1, ')': -1}
    sum = 0
    pos = 0
    for i, c in enumerate(input):
        sum += instruction[c]
        if sum == -1 and pos == 0:
            pos = i + 1
    return sum, pos
```

Finally you can find the complete source code [here](https://github.com/crixodia/aoc/blob/main/2015/01_not_quite_lisp/main.py). Enjoy it and fell free to comment below your suggestions and thoughts.
