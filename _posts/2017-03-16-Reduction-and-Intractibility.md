---
layout: post
published: True

images:

    - url: /images/algs/reduction/reduction_visualized.png
      alt: Reduction 
      title: Reduction
---

## Reductions

Reductions allow us to to determine the difficulty of a problem.

We say that for two problems $$Y$$ and $$X$$, $$Y \leq_p X$$ if instances of $$Y$$ can be solved by a polynomial amount of computation plus a polynomial number of calls to a "black-box" that solves $$X$$.

![reduction](/images/algs/reduction/reduction_visualized.png){: .center-image}

In order for reductions to work, we have to have some set of problems that are known to be $$NPC$$. A common starting point is $$SAT$$, or boolean satisfiability. 
Karp has a famous paper containing 21 $$NPC$$ problems you can view [here](https://en.wikipedia.org/wiki/Karp's_21_NP-complete_problems).

### 3SAT
For $$3SAT$$, we are given a boolean formula $$\Phi$$ in CNF form ([conjunctive normal form](https://en.wikipedia.org/wiki/Conjunctive_normal_form)) such that for each clause $$C_i$$, $$C_I = l_1 \lor l_2 \lor l_3$$. We return true if this boolean formula is satisfiable.

Given $$3SAT$$ formula $$PHO


### Clique
We say that a graph $$G$$ has a clique of size $$k$$ if there is a set of vertices $$I$$ such that $$\left\vert{I}\right\vert = k$$ and $$I$$ is fully connected.

We will prove that $$CLIQUE \leq_p 3SAT$$.

We can define our function $$A$$ as follows:

For each $$C_r 


### Vertex Cover
We will prove that $$VERTEX\ COVER \leq_p CLIQUE$$.

### Dominating Set
We will prove that $$DOMINATING\ SET \leq_p VERTEX\ COVER $$.

## Complexity Classes
### P
### NP
### NP-Complete
### Co-NP

