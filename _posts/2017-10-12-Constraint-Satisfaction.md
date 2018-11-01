---
layout: post
tags: [algorithms, artificial-intelligence]

---
Constraint satisfication is similar to search problems. You can check out a solver I wrote [here](https://jcaip.github.io/Constraint-Satisfaction-Solver/)

For a group of variables, $$x_1, x_2, \ldots , x_n$$, with domains $$D_1, D_2, \ldots , D_n$$, we want to find an assignment for each variable such that no constraints are violated.
<!--more-->

From a problem, we can create a **constraint graph**, which is a visualization of the dependencies between variables. Each variables is represented by a vertex and an edge represents a constraint.

#### Types of Constraints
+ Unary Constraint - Constraints involving just one variable ($$A > 0$$).
+ Binary Constraint - Constraints involving two variables ( $$A \neq B$$).
+ Global Constraint - Constraints involving two or more variables ( $$A \neq B \neq C$$)

### Solving Contraint Satisfaction Problems

#### Backtracking Search
Naively, we can try to formulate a search problem, randomly assigning a variable a value from it's domain at each stage in the search tree. However, this means that the total # of states generated is $$n!d^n$$, where $$d$$ is the number of elements in th edomain and $$n$$ is the number of variables. 

We can exploit the fact that variable assignment is communatitive to speedup our serach. At each level, we assign only to a single variable that is determined. Using this, we get a runtime of $$O(d^n)$$, a significant improvement.

#### Heurestics
+ **Least Remaining Values** - when choosing a variable to assign, pick the variable that has the fewest states. 
+ **Degree Heurestic** - when choosing a variable to assign, pick the variable with the highest degree. The idea behind these two heurestics is to detect failure early, to avoid unnecessary expansion.
+ **Least Constraing Value** - when choosing a value to assign to a variable, pick the value that leaves the most remaining choices for the other unassigned variables.

#### Detecting Failure
+ **Local Consistency** - after each assignment, check that no constraints are violated
+ **Forward Checking** - after each assignment, look at all the neighbors and remove all remaining values. 
+ **Arc Consistency** - after each assignment, recursively apply forward checking, propogating information everytime you make an assignment

#### Exploiting Problem Structure
If our constraint graph is a tree, there is a $$P$$ solution to variable assignment. 
You may be able to use **Tree Decomposition** to break down your constraint graph into a tree, and then solve each tree with **message passing**.

## Games
Games can be broken down into different categories.

||**Deterministic**| **Chance**|
|--|--|--|
|**Perfect**|chess|backgammon|
|**Imperfect**  |battleship|poker|

### Formulating Games
In each game, there is said to be a min and max player, who take turns playing. A move by a min/max player is called a **ply**, while a **move** is when every player gets a ply.

Each game also has a **temrinal test** and a set of **operators**, much like search funcitons. 

Furthermore, every ame has a **utility function** that determines the value of the final state. The max player seeks to maxamize the utility function, while the min player seeks to minimize the function. 

If $$Utility(MAX) = -Utility(MIN)$$ the game is said to be a **zero-sum game**.

### Solving Games
#### Minimax
Minimax builds a search tree, with min and max nodes. A **min** node is the mininum of all its children, while a **max** node is the max of all its children. With this game tree, we are able to determine the optimal move by simply running DFS with backtracking to compute the min/max.

However, in practice, this tree is too large to build practically. Instead, we limit the depth of the tree and create a **Evaluation function**, which is an approximation of the current game state. 

We can use iterative deeepening to avoid keeping the whole tree in memory.

#### $$\alpha-\beta$$ Pruning
However, we can avoid looking at certain nodes with pruning. If we keep track of two values, $$\alpha$$, and $$\beta$$, we can prune roughly 1/2 of the nodes and avoid looking at them.
