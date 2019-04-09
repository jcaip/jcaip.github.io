---
published: True
layout: post
title: Constructing $\mathbb{R}$ visually from $\mathbb{N}$
tags: [theory, mathematics]
images:
    - url: https://upload.wikimedia.org/wikipedia/commons/thumb/1/17/Number-systems.svg/1024px-Number-systems.svg.png
---

I'm going to go over how to construct $\mathbb{R}$, or the real numbers. 
We're going to by constructing the natural numbers, and from there we'll construct the integers, then the rationals and then the reals. 

Along each step we'll be restrictive in what we're allowed to use - only the relations and numbers that we've defined before.

<!--more-->

## Stuff you should know

Sets, relations, fol, functions, inductive proofs, notion of supremum/infimun . TODO: update this section

**Note:** All the axes you see should have arrows, but I couldn't figure out how to do it in matplotlib. 


## The Naturals $\mathbb{N}$

Of course, we have to start from somewhere - in this case, we'll start with the natural numbers, $\mathbb{N}$.

Unfortunately this part is much less visual, but it gets better later on.

The naturals are defined by a 3-tuple $(\mathbb{N}, 0, S)$ that satisfies these 5 axioms, called the **Peanno Axioms**. 
1. $\mathbb{N}$ is a set and $0 \in \mathbb{N}$
2. $S : \mathbb{N} \to \mathbb{N}$ is a function with $Dom(S) = \mathbb{N}$
3. $\forall n \in \mathbb{N}: S(n) \neq 0$ 
4. $\forall n, m \in \mathbb{N}: S(n) = S(m) \implies n = m $ 
5. $\forall A \subset \mathbb{N}: 0 \in A \land S(A) \subset A \implies A = \mathbb{N}$ 

We'll call $0 := \emptyset, 1 := S(0), 2 := S(S(0))$, and so on. This gives us the natural numbers, $\mathbb{N}$.

### Arithmetic and well-ordering the naturals

We can define addition of the naturals recursively

1. $\forall m \in \mathbb{N} : m+0 = m $
2. $\forall m, n \in \mathbb{N} : m + S(n) = S(m+n)$

We can also define multiplication in the naturals.

We can define a relation, $\leq$ that makes $\mathbb{N}$ well-ordered.

$$\forall m, n \in \mathbb{N}: m \leq n \iff \exists r \in \mathbb{N} : n = m + r$$

This is an anti-symmetric relation

This gives us the basics for a subtraction operation, by denoting $r$ the difference of $n-m$

$$\forall m, n \in \mathbb{N}: (\exists r \in \mathbb{N}: m+r = n ) \implies r = m-n$$

But here we run into a problem, as this operation is only defined if $m \leq n$. To get around this, we'll expand $\mathbb{N}$ to $\mathbb{z}$.

If this is a bit unclear to you don't worry, it's the most "technical" bit of this post. The main takeaway here is that we have $\mathbb{N}$, from 0 to infinity, and we have several operators (addition, inequality, and multiplication) defined for these numbers.

We'd really like a subtraction operation, and there seems to be a clear definition for one, but it's undefined for certain pairs, so we'd like to expand the naturals to the integers.

## The Integers $\mathbb{Z}$

To construct $\mathbb{Z}$, we will consider all pairs of naturals numbers, or $\mathbb{N} \times \mathbb{N}$. 

We'll define a relation $R$ over all pairs of pairs, $(\mathbb{N} \times \mathbb{N}) \times (\mathbb{N} \times \mathbb{N})$ such that

$$\forall (p_1, q_1), (p_2, q_2) \in \mathbb{N} \times \mathbb{N}: (p_1, q_1)\  R \  (p_2, q_2) \iff p_1 + q_2 = p_2 + q_1$$

This is an equivalence relation, as it is transitive, symmetric, and reflexive. 

TODO: proof

Then the set of all equivalence classes gives us a way to partition $\mathbb{N} \times \mathbb{N}$. 

This is in fact the integers, $\mathbb{Z} = \\{ [(p, q)]:  \forall p, q \in \mathbb{N} \\}$

We can visualize this by plotting all pairs $(p, q) \in \mathbb{N} \times \mathbb{N}$ and coloring them a different color based on their equivalence class.

![all pairs](/images/R/NxN.png){: .center}

Here each equivalence class (integer) corresponds to a different color. You can see that each class corresponds to a different line.

![integer axis](/images/R/Zaxis.png){: .center}

Below we see how to draw an axis that defines the $\mathbb{Z}$ number line for the interval $[-5, 5]$.

### Arithmetic and well-ordering of the integers
We can define the negation operator as well as addition, subtraction, and multiplication as follows

1. $-[(p,q)] = [(q, p)]$
2. $[(p_1,q_1)] + [(p_2, q_2)] = [(p_1 + p_2 , q_1 + q_2)]$
3. $[(p_1,q_1)] - [(p_2, q_2)] = [(p_1 + q_2 , q_1 + p_2)]$
4. $[(p_1,q_1)] \dot [(p_2, q_2)] = [(p_1 \dot p_2 + q_1 \dot q_2, p_q \dot q_2 + p_2 \dot q_1)]$

To get a better sense of arithmetic in $\mathbb{Z}$, let's show $1+ -2 = -1 = 2 + -3$.

Going back to our construction, each integer is an equivalence class so it is a subset of $\mathbb{N} \times \mathbb{N}$.

We can pick some element from this subset to represent the entire equivalence class. 
So we pick some pairs to represent $1, -2, 2, -3$ respectively. 

But visually, each pair defines a vector from the origin to the coordinates given by the pair. So to recap, vectors define a pair of coordinates in $\mathbb{N}$ which corresponds to a particular equivalence class of $\mathbb{N} \times \mathbb{N}$ or a specific integer in $\mathbb{Z}$. 

If we add these two vectors, we get a new vector whose equivalence class is the same as the integer result ($-1$). 

![addition](/images/R/addition.png){: .center}

So adding the vector representations of integers yields a vector representation of the sum. This vector representation holds for all of the operations we defined previously.

So now we've solved the problem described at the start of this since $\forall m, n \in \mathbb{Z} \exist r \in \mathbb{Z} : r = m -n $. Take care to note that we've only used properties that we've previously defined (addition and multiplication).
But there's still another problem, as we'd like to be able to undo this multiplication problem. To do that we will use the same technique as before to expand $\mathbb{Z}$ to  $\mathbb{Q}$.

## The Rationals $\mathbb{Q}$

We will define a relation $R$ on $\mathbb{Z} \times (\mathbb{Z} \setminus \{0\})$ such that

$$\forall (p_1, q_1), (p_2, q_2) \in \mathbb{Z} \times \mathbb{Z} \setminus \{0\} : (p_1, q_1)\  R \  (p_2, q_2) \iff p_1 * q_2 = p_2 * q_1$$

We can use the same trick to visualize the equivalence classes.

![z cross z](/images/R/ZxZ.png){: .center}

The equivalence classes are a bit harder to see here, but again the correspond to lines, just this time lines with different slope instead of intercept.

We can imagine a curved axis that defines the $\mathbb{Q}$ number line. 

![rational axis](/images/R/Qaxis.png){: .center}

If we flattened this out, we would get the $\mathbb{Q}$ number line.

### Arithmetic and well-ordering of the rationals
TODO


Again let's visualize these operations by plotting the corresponding vectors

## The Reals $\mathbb{R}$
Unfortunately we can't use the same approach to construct $\mathbb{R}$. While the cardinality of the $\mathbb{N}, \mathbb{Z}, \mathbb{Q}$ are all equal, $\mathbb{R}$ is far more dense.

Instead we'll consider a way to partition the $\mathbb{Q}$ number line called a Dedekinde cut.

More precisely, a Dedekinde cut is a 

Every real number corresponds to a Dedekinde cut.

### Arithmetic and well-ordering of the reals.


And we're done, from $\mathbb{N} \to \mathbb{R}$.

#### Sources
The construction of the reals from Dedekinde cuts is well-known.

The construction I showed as well as most of the arithmetic properties were learned from MATH131, while the visualizations were created by me. You can find the code for them [here]().

