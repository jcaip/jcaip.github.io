---
layout: post
title: Artificial Intelligence 

images:
    - url: https://www.deepcoredata.com/wp-content/uploads/2016/06/small_1420.png
      alt: Artificial Intelligence
      title: ida*
---

## Determining Intelligence

#### Turing Test
The Turing Test is a test devised by Alan Turning. The test calls for a computer and a human to be put in two seprate rooms, and another human to converse with both the human and the computer. If the human observer thinks he/she is talking to another human, the computer is said to have passed the Turing test. 

#### Winograd Schema
Winograd Schema are another series of problems that are used to test for understanding.
The idea is to have a sentence, with two possible meanings and to let the computer pick two choices that make sense. 

E.g. The city councilmen refused the demonstrators a permit because they [feared/advocated] violence. Who [feared/advocated] violence?


## Search
Many problems, such as 8-puzzles, n-queens, or solving a rubic's cube can be formulated as search problems.

#### Formulating Search Problems
All search problems should have these parts:
+ **initial state** - This is the initial state that is given to the search problem
+ **goal state** - There may be more than one goal state. It is the state at which the search problem is solved.
+ **transition model/successor function** - Describes how to move from one state to another given an action. 
+ **action** - Each action changes the current state. Each action may also have an associated cost with it. 

The goal is to find a sequence of actions to move from the initial state to the goal state and minimize the cost of your actions

#### Solving Search Problems
The idea behind solving all search problems is the same.
```
frontier = {I}
loop:
    if frontier is empty:
        return FALSE
    node = some node in the frontier
    if goal == node:
        return current state
    generate the node's children
    add the node's children to the frontier and "explored set"
```
Heurestic/Informed Search

BFS
DFS
Iterative Deepening Search
Depth Limited Serach
Tree Search
Uniform Cost Search
Best First Search
A\* Search

Seperation Property
#### Determining a Heurestic
Consistency
Admissability

How to generate heurestics

## Local Search Strategies
+ Gradient Descent
+ [Simulated Annealing](https://jcaip.github.io/Simulated-Annealing/)
+ [Genetic Algorithms](https://jcaip.github.io/Genetic-Algorithm/)

## Constraint Satisfaction

Constraint Graph
#### Types of Constraints
+ Unary Constraint
+ Binary Constraint
+ Global Constraint

#### Solving Contraint Satisfaction Problems

## Games
