---
published: True
layout: post
title: Building $\mathbb{R}$ visually
tags: [theory, mathematics]
images:
    - url: https://upload.wikimedia.org/wikipedia/commons/thumb/1/17/Number-systems.svg/1024px-Number-systems.svg.png
---

I'm going to go over how to construct $\mathbb{R}$, or the real numbers. 
We're going to by constructing the natural numbers, and from there we'll construct the integers, then the rationals and then the reals. 
<!--more-->

## Stuff you should know
Sets, relations, first-order logic, functions, inductive proofs, notion of supremum/infimun.
TODO: update this section

This is an equivalence relation, as it is 

+ transitive
+ symmetric
+ reflexive. 

**Note:** All the axes you see should have arrows, but I couldn't figure out how to do it in matplotlib. 

# The Naturals: $\mathbb{N}$

Of course, we have to start from somewhere - in this case, we'll start with the natural numbers, $\mathbb{N}$.
Unfortunately this part is much less visual, but it gets better later on.


The naturals are defined by a 3-tuple $(\mathbb{N}, 0, S)$ that satisfies the 5 **Peanno Axioms**. 
1. $\mathbb{N}$ is a set and $0 \in \mathbb{N}$
2. $S : \mathbb{N} \to \mathbb{N}$ is a function with $Dom(S) = \mathbb{N}$
3. $\forall n \in \mathbb{N}: S(n) \neq 0$ 
4. $\forall n, m \in \mathbb{N}: S(n) = S(m) \implies n = m $ 
5. $\forall A \subset \mathbb{N}: 0 \in A \land S(A) \subset A \implies A = \mathbb{N}$ 

One such construction we'll look at under [ZFC set theory]() is as follows:

We'll call $0 := \emptyset, 1 := S(0), 2 := S(S(0))$, and so on. This gives us the natural numbers, $\mathbb{N}$.

### Arithmetic on the naturals
With this, we can define some of the operations we are familiar with, starting with addition.

1. $\forall m \in \mathbb{N} : m+0 = m $
2. $\forall m, n \in \mathbb{N} : m + S(n) = S(m+n)$

We can also define multiplication in the naturals in the same fashion.

1. $\forall m \in \mathbb{N} : 0 \cdot m = 0 $
2. $\forall m, n \in \mathbb{N} :  S(n) \cdot m  = n \cdot m + n$

Note if we denote $1 = S(0)$ we can also get $\forall m \in \mathbb{N}: S(m) = m+1$.

These operations uphold the properties that we expect of them - they are distributive, commutative, associative, etc. A full proof of these can be seen in the notes.

One property to take note of is that addition is injective (one-to-one), so there is at most 1 solution to the equation $ y = a + b$ given $y, b$.

#### Defining a well-ordering
We can also define the $\leq$ relation as follows:

$$\forall m, n \in \mathbb{N}: m \leq n \iff \exists r \in \mathbb{N} : n = m + r$$

This gives us the basics for a subtraction operation, by denoting $r$ the difference of $n-m$

$$\forall m, n \in \mathbb{N}: (\exists r \in \mathbb{N}: m+r = n ) \implies r = m-n$$

But here we run into a problem, as this operation is only defined if $m \leq n$. To get around this, we'll expand $\mathbb{N}$ to $\mathbb{Z}$.

The main takeaway here is that we have $\mathbb{N}$, defined from 0 to infinity, and we have several operators (addition, inequality, and multiplication) defined for these numbers.
We'd really like a subtraction operation, to solve any equation $y = a + b$ given $y, b$ and there seems to be a clear definition for one, but it's not defined for $y \leq b$, so we'd like to expand the naturals to the integers, such that this subtraction relation is defined $\forall y, b \in \mathbb{Z}$.

# The Integers $\mathbb{Z}$

To construct $\mathbb{Z}$, we will consider all pairs of naturals numbers, or $\mathbb{N} \times \mathbb{N}$. 

We'll define an equivalence relation $R$ over all pairs of pairs, $(\mathbb{N} \times \mathbb{N}) \times (\mathbb{N} \times \mathbb{N})$ such that

$$\forall (p_1, q_1), (p_2, q_2) \in \mathbb{N} \times \mathbb{N}: (p_1, q_1)\  R \  (p_2, q_2) \iff p_1 + q_2 = p_2 + q_1$$

Then the set of all equivalence classes gives us a way to partition $\mathbb{N} \times \mathbb{N}$ and is in fact the integers, $\mathbb{Z} = \\{ [(p, q)]:  \forall p, q \in \mathbb{N} \\}$

We can visualize this by plotting all pairs $(p, q) \in \mathbb{N} \times \mathbb{N}$ and coloring them a different color based on their equivalence class.

![all pairs](/images/R/NxN.png){: .center}

Here each equivalence class (integer) corresponds to a different color. You can see that each class corresponds to a line with a different y-intercept. In fact the y-intercept of the line is the integer value of that line. 

Below we see how to draw an axis that defines the $\mathbb{Z}$ number line for the interval $[-5, 5]$.

![integer axis](/images/R/Zaxis.png){: .center}


### Arithmetic on the integers
We can define the negation operator as well as addition, subtraction, and multiplication as follows

1. $-[(p,q)] = [(q, p)]$
2. $[(p_1,q_1)] + [(p_2, q_2)] = [(p_1 + p_2 , q_1 + q_2)]$
3. $[(p_1,q_1)] - [(p_2, q_2)] = [(p_1 + q_2 , q_1 + p_2)]$
4. $[(p_1,q_1)] \cdot [(p_2, q_2)] = [(p_1 \cdot p_2 + q_1 \cdot q_2, p_q \cdot q_2 + p_2 \cdot q_1)]$

To get a better sense of arithmetic in $\mathbb{Z}$, let's try to visualize how $1+ -2 = -1 = 2 + -3$.

Going back to our construction, each integer is an equivalence class so it is a subset of $\mathbb{N} \times \mathbb{N}$.

We can pick some element from this subset to represent the entire equivalence class. 
So we pick some pairs to represent $1, -2, 2, -3$ respectively. 

But visually, each pair defines a vector from the origin to the coordinates given by the pair. So to recap, vectors define a pair of coordinates in $\mathbb{N}$ which corresponds to a particular equivalence class of $\mathbb{N} \times \mathbb{N}$ or a specific integer in $\mathbb{Z}$. 

If we add these two vectors, we get a new vector whose equivalence class is the same as the integer result ($-1$). 

![addition](/images/R/addition.png){: .center}

So adding the vector representations of integers yields a vector representation of the sum. This vector representation holds for all of the operations we defined previously.

So now we've solved the problem described at the start of this since $\forall m, n \in \mathbb{Z}. \exists r \in \mathbb{Z} : r = m -n $. Take care to note that we've only used properties that we've previously defined (addition and multiplication on $\mathbb{N}$).

But there's still another problem, as we'd like to be able to undo this multiplication operation. To do that we will use the same technique as before to expand $\mathbb{Z}$ to  $\mathbb{Q}$.

# The Rationals $\mathbb{Q}$

We will define a relation $R$ on $\mathbb{Z} \times \mathbb{Z} \setminus \\{0\\}$ such that

$$\forall (p_1, q_1), (p_2, q_2) \in \mathbb{Z} \times \mathbb{Z} \setminus \{0\} : (p_1, q_1)\  R \  (p_2, q_2) \iff p_1 \cdot q_2 = p_2 \cdot q_1$$

We can use the same trick to visualize the equivalence classes, 

![z cross z](/images/R/ZxZ.png){: .center}

The equivalence classes are a bit harder to see here, but again they correspond to lines, just now with different slopes instead of intercepts.

![rational axis](/images/R/Qaxis.png){: .center}

We can imagine a curved axis that if flattened would give the $\mathbb{Q}$ number line. 
Notice that when $\theta= \frac{\pi}{2}$ the slope of the line is $+\infty$ and at $\theta = -\frac{\pi}{2}$ the slope of the line is $-\infty$.

Furthermore the vertical line corresponding to the y-axis is not defined, as this corresponds to division by 0.

### Arithmetic on the rationals

Now let's consider arithmetic over $\mathbb{Q}$ by defining the operation of addition, subtraction multiplication and division as follows:

1. $[(p_1,q_1)] + [(p_2, q_2)] = [(p_1 \cdot q_2 + p_2 \cdot q_1, q_1 \cdot q_2)]$
2. $[(p_1,q_1)] - [(p_2, q_2)] = [(p_1 \cdot q_2 - p_2 \cdot q_1, q_1 \cdot q_2)]$
3. $[(p_1,q_1)] \cdot [(p_2, q_2)] = [(p_1 \cdot p_2 , q_1 \cdot q_2)]$
4. $[(p_1,q_1)] \div [(p_2, q_2)] = [(p_1 \cdot q_2 , q_1 \cdot p_2)]$

Again we'll plot the vectors to see that they are the right equivalence class.

![rational axis](/images/R/Qaxis.png){: .center}

# The Reals $\mathbb{R}$
Unfortunately we can't use the same approach to construct $\mathbb{R}$. While the cardinality of the $\mathbb{N}, \mathbb{Z}, \mathbb{Q}$ are all equal, $\mathbb{R}$ is far more dense.

Instead we'll consider a way to partition the $\mathbb{Q}$ number line called a Dedekinde cut.

More precisely, a Dedekinde cut if it satisfys these 4 conditions: 

1. $r \neq \emptyset$
2. $r \neq \mathbb{Q}$
3. closed downward
4. contains no greatest element

Then $\mathbb{R}$ is the set of all cuts of $\mathbb{Q}$.

### Arithmetic on the reals


And we're done, from $\mathbb{N} \to \mathbb{R}$.


## Conclusion

The construction of the reals from Dedekinde cuts is well-known.
The construction I showed as well as most of the arithmetic properties were learned from MATH131, while the visualizations were created by me. You can find the code for them [here]().

