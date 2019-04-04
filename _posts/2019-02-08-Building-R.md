---
published: False
layout: post
title: Constructing $\mathbb{R}$ Visually
tags: [theory, mathematics]
---

<!--more-->

I'm going to go over how to construct $\mathbb{R}$, or the real numbers. 
We're going to by constructing the natural numbers, and from there we'll construct the integers, then the rationals and then the reals. 

I'm hoping to give a visual construction, so you can "see" what exactly each axis corresponds to as by it's mathematical construction.
All the axes you see should have arrows, but I couldn't figure out how to do it in matplotlib. 

## Sets, Relations, and Functions

First things first let's go over some of the machinery we'll need. We'll need ZFC set theory for the naturals. 

## The Naturals $\mathbb{N}$
Of course, we have to start from somewhere - in this case, we'll start with the natural numbers, $\mathbb{N}$ defined by the **Peanno Axioms** under ZFC. 

The naturals are defined by a 3-tuple $(\mathbb{N}, 0, S)$ that satisfies these 5 axioms. 
1. $\mathbb{N}$ is a set and $0 \in \mathbb{N}$
2. $S : \mathbb{N} \to \mathbb{N}$ is a function with $Dom(S) = \mathbb{N}$
3. $\forall n \in \mathbb{N}: S(n) \neq 0$ 
4. $\forall n, m \in \mathbb{N}: S(n) = S(m) \implies n = m $ 
5. $\forall A \subset \mathbb{N}: 0 \in A \land S(A) \subset A \implies A = \mathbb{N}$ 

Basically we can think about

We'll have 0 be the empty set, and then S(0) as "1", S(S(0)) as "2" and so on. This gives us the natural numbers.

## The Integers $\mathbb{Z}$
To construct $\mathbb{Z}$, we will define a relation $R$ such that

$$\forall (p_1, q_1), (p_2, q_2) \in \mathbb{N} \times \mathbb{N}: (p_1, q_1)\  R \  (p_2, q_2) \iff p_1 -q_1 = p_2 - q_2$$.

This is in fact an equivalence relation, as it is transitive, symmetric, and reflexive.
So we can look at the equivalence classes of this relation, which will give us a way to partition $\mathbb{N} \times \mathbb{N}$.
In fact the set of all equivalence classes is just $\mathbb{Z}$.

We can visualize this by plotting all pairs in $\mathbb{N} \times \mathbb{N}$ and coloring them a different color based on $p-q$.
![all pairs](/images/R/NxN.png){: .center}

So you can see here that each equivalence class corresponds to a different line. Once can create the $\mathbb{Z}$ number line by taking a line across. Below  we see a line that defines the $\mathbb{Z}$ number line for the interval $[-5, 5]$.
![zaxis](/images/R/Zaxis.png){: .center}

To get a better sense of our visualization, let's add two integers together.

![addition](/images/R/addition.png){: .center}

## The Rationals $\mathbb{Q}$
Now we have $Z$.  We'll do something similar to construct Q

We take Z and consider all pairs in Z. We define a relation R such that $(p_1, q_1) \ R \ (p_2, q_2) \iff p_1 \times q_2 = p_2 \times q_1$
![zaxis](/images/R/ZxZ.png){: .center}

![zaxis](/images/R/Qaxis.png){: .center}

## The Reals $\mathbb{R}$
So now we have $Q$. But to go from Q to R takes a bit more effort. 

Algebraic and analytic needs for the reals

every cauchy sequence should have a limit -> but under Q this is not true. 
can't solve (x^2 -x) = 1

Define a dedekinde cut, every rational number is a dedekinde cut. 

addition, is anding the sets together

