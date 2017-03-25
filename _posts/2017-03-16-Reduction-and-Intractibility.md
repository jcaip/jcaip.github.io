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

You can think of reduction as calling a library as part of your solution. For example. 

In order for reductions to work, we have to have some set of problems that are known to be $$NPC$$. A common starting point is $$SAT$$, or boolean satisfiability. 
Karp has a famous paper containing 21 $$NPC$$ problems you can view [here](https://en.wikipedia.org/wiki/Karp's_21_NP-complete_problems).

### 3SAT
For $$3SAT$$, we are given a boolean formula $$\Phi$$ in CNF form ([conjunctive normal form](https://en.wikipedia.org/wiki/Conjunctive_normal_form)) such that for each clause $$C_r$$, $$C_r = l_1 \lor l_2 \lor l_3$$. We return true if this boolean formula is satisfiable.

### Clique
We say that a graph $$G$$ has a clique of size $$k$$ if there is a set of vertices $$I$$ such that $$\left\vert{I}\right\vert = k$$ and $$I$$ is fully connected.

We will prove that $$CLIQUE \leq_p 3SAT$$.

We can define our function $$A$$ as follows:

For each $$C_r  = ( l_1 \lor l_2 \lor l_3)$$ for $$r =1,\ldots,k$$ , create corresponding vertices $$v_1, v_2, v_3$$ in $$V$$. Now add an edge between $$v_i^r$$ and $$v_j^s$$ if $$r \neq s$$ and the corresponding literals are consistent( You can't connect $$\neg l_1$$ to $$l_1$$).

### Vertex Cover
We will prove that $$VERTEX\ COVER \leq_p CLIQUE$$.

### Dominating Set
We will prove that $$DOMINATING\ SET \leq_p VERTEX\ COVER $$.

## Complexity Classes
### P
Loosly, $$P$$$ is defined as the set of all problems such that there is a polynomial time solution. This means that the solution to a problem $$a \in P$$ runs in $$O(n^k)$$ time where $$k$$ is some constant.

### NP
Loosly, $$NP$$$ is defined as the set of all problems such that there is a polynomial time verification. 

### NP-Complete
### Co-NP

