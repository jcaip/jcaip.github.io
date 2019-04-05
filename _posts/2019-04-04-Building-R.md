---
published: False
layout: post
title: Constructing $\mathbb{R}$ Visually
tags: [theory, mathematics]
---

I'm going to go over how to construct $\mathbb{R}$, or the real numbers. 
We're going to by constructing the natural numbers, and from there we'll construct the integers, then the rationals and then the reals. 

<!--more-->

## The Naturals $\mathbb{N}$

Of course, we have to start from somewhere - in this case, we'll start with the natural numbers, $\mathbb{N}$ defined by the **Peanno Axioms** under ZFC. Unfortunately this part is much less visual, but bear with me, it gets better later on.

The naturals are defined by a 3-tuple $(\mathbb{N}, 0, S)$ that satisfies these 5 axioms. 
1. $\mathbb{N}$ is a set and $0 \in \mathbb{N}$
2. $S : \mathbb{N} \to \mathbb{N}$ is a function with $Dom(S) = \mathbb{N}$
3. $\forall n \in \mathbb{N}: S(n) \neq 0$ 
4. $\forall n, m \in \mathbb{N}: S(n) = S(m) \implies n = m $ 
5. $\forall A \subset \mathbb{N}: 0 \in A \land S(A) \subset A \implies A = \mathbb{N}$ 

Basically we can think about

We'll call $0 := \emptyset, 1 := S(0), 2 := S(S(0))$, and so on. This gives us the natural numbers, $\mathbb{N}$.

### Arithmetic and ordering the naturals

Addition is defined as 
1. $\forall m \in \mathbb{N} : m+0 = m $
2. $\forall m, n \in \mathbb{N} : m + S(n) = S(m+n)$

We can define the well-ordering relation, $\leq$ 

$$\forall m, n \in \mathbb{N}: m \leq n \iff \exists r \in \mathbb{N} : n = m + r$$.

We would like to define a subtraction relation, but it would only be defined only if $m \leq n$. We'd like to expand $\N$ to 4$\mathbb{z}$.

## The Integers $\mathbb{Z}$

To construct $\mathbb{Z}$, we will define a relation $R$ on $\mathbb{N} \times \mathbb{N}$ such that

$$\forall (p_1, q_1), (p_2, q_2) \in \mathbb{N} \times \mathbb{N}: (p_1, q_1)\  R \  (p_2, q_2) \iff p_1 -q_1 = p_2 - q_2$$

This is an equivalence relation, as it is transitive, symmetric, and reflexive.

Let's look at the equivalence classes of this relation, which will give us a way to partition $\mathbb{N} \times \mathbb{N}$.

In fact the set of all equivalence classes is $\mathbb{Z}$.

We can visualize this by plotting all pairs $(p, q) \in \mathbb{N} \times \mathbb{N}$ and coloring them a different color based on $p-q$.

**Note:** All the axes you see should have arrows, but I couldn't figure out how to do it in matplotlib. 

![all pairs](/images/R/NxN.png){: .center}

Here each equivalence class corresponds to a different color. You can see that each class corresponds to a different line.

Below we see how to draw an axis that defines the $\mathbb{Z}$ number line for the interval $[-5, 5]$.

![integer axis](/images/R/Zaxis.png){: .center}

To get a better sense of our visualization, let's show $1+ -2 = -1 = 2 + -3$.

Going back to our construction, each integer is an equivalence class so it is a subset of $\mathbb{N} \times \mathbb{N}$.
If we pick some element from this subset it will represent the entire equivalence class. 

So we pick some pairs to represent $1, -2, 2, -3$. Each pair defines a vector. 

If we add these two vectors, we get a new vector whose equivalence class is the same as the integer result ($-1$). 

![addition](/images/R/addition.png){: .center}

So adding the vector representations of integers yields a vector representation of the sum.

## The Rationals $\mathbb{Q}$

We'll use a similar idea to construct $\mathbb{Q}$.

We will define a relation $R$ on $\mathbb{Z} \times \mathbb{Z} \setminus \{0\} $ such that

$$\forall (p_1, q_1), (p_2, q_2) \in \mathbb{Z} \times \mathbb{Z} \setminus \{0\} : (p_1, q_1)\  R \  (p_2, q_2) \iff p_1 * q_2 = p_2 * q_1$$

![z cross z](/images/R/ZxZ.png){: .center}

The equivalence classes are a bit harder to see here, but again the correspond to lines, just this time lines with different slope instead of intercept.

We can imagine a curved axis that defines the $\mathbb{Q}$ number line. 

![rational axis](/images/R/Qaxis.png){: .center}

If we flattened this out, we would get the $\mathbb{Q}$ number line.

## The Reals $\mathbb{R}$
Unfortunately we can't use the same approach to construct $\mathbb{R}$.

Instead we'll consider a way to partition the $\mathbb{Q}$ number line called a Dedekinde cut.

More precisely, a Dedekinde cut is a 

Every real number corresponds to a Dedekinde cut.
