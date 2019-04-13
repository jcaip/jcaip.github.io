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
Our goal here is too add two real numbers together, but building everything we need to do so along the way.
<!--more-->

## Stuff you should know
Sets, relations, first-order logic, supremum/infimun, equivalence relations and classes.

**Note:** All the axes you see should have arrows, but I couldn't figure out how to do it in matplotlib. 

# The Naturals: $\mathbb{N}$

Unfortunately this part is much less visual, but it gets better later on.

The naturals are defined by a 3-tuple $(\mathbb{N}, 0, S)$ that satisfies the 5 **Peanno Axioms**. 
1. $\mathbb{N}$ is a set and $0 \in \mathbb{N}$
2. $S : \mathbb{N} \to \mathbb{N}$ is a function with $Dom(S) = \mathbb{N}$
3. $\forall n \in \mathbb{N}: S(n) \neq 0$ 
4. $\forall n, m \in \mathbb{N}: S(n) = S(m) \implies n = m $ 
5. $\forall A \subset \mathbb{N}: 0 \in A \land S(A) \subset A \implies A = \mathbb{N}$ 

Our construction under [ZFC set theory]() is as follows:

We'll let $0 = \emptyset$ and $\forall n: S(n) = n \cup \\{n\\}$

$$0 = \emptyset$$

$$1 = S(0) = \{ \emptyset \}$$

$$2 = S(S(0)) = S(1) = \{ \emptyset, \{ \emptyset\} \} $$

$$3 = S(S(S(0))) = S(2)=  \{ \emptyset, \{ \emptyset \}, \{ \emptyset, \{ \emptyset\} \} \}$$

This gives us the natural numbers, $\mathbb{N}$.

### Arithmetic on the naturals
With this, we can define some of the operations we are familiar with, starting with addition.

1. $\forall m \in \mathbb{N} : m+0 = m $
2. $\forall m, n \in \mathbb{N} : m + S(n) = S(m+n)$

Futhermore we denote $1 = S(0)$ so that $\forall m \in \mathbb{N}: S(m) = m+1$.

Let's add two numbers to get a better sense of our construction.

$$2 + 1 = 3$$

Using the fact that $1 = S(0)$ we get $2+1 = 2 + S(0) = 3$

Applying our definition of addition (2) yields $2+1 =  S(2+0) = 3$

Now we apply case (1) to get $2 + 0 = 2$ so $2+1 =  S(2) = 3$

And this meets our natural expectation that $2+1=3$.

We can also define multiplication in the naturals similarly.

1. $\forall m \in \mathbb{N} : 0 \cdot m = 0 $
2. $\forall m, n \in \mathbb{N} :  S(n) \cdot m  = n \cdot m + n$

These operations uphold the properties that we expect of them - they are distributive, commutative, associative, etc.

One property to take note of is that addition is injective (one-to-one), so there is at most 1 solution to the equation $ y = a + b$ given $y, b$.

#### Defining a well-ordering
We can also define the $\leq$ relation as follows:

$$\forall m, n \in \mathbb{N}: m \leq n \iff \exists r \in \mathbb{N} : n = m + r$$

This gives us the basics for a subtraction operation, by denoting $r$ the difference of $n-m$

$$\forall m, n \in \mathbb{N}: (\exists r \in \mathbb{N}: m+r = n ) \implies r = m-n$$

But here we run into a problem, as this operation is only defined if $m \leq n$. To get around this, we'll expand $\mathbb{N}$ to $\mathbb{Z}$.

The main takeaway here is that we have $\mathbb{N}$, defined from 0 to infinity, and we have several operators (addition, inequality, and multiplication) defined for these numbers.
We'd really like a subtraction operation, to solve any equation $y = a + b$ given $y, b$ and there seems to be a clear definition for one, but it's not defined for $y \leq b$, so we'd like to expand the naturals.

# The Integers $\mathbb{Z}$

To construct $\mathbb{Z}$, we will consider all pairs of naturals numbers, or $\mathbb{N} \times \mathbb{N}$. 

We'll define an equivalence relation $R$ over all pairs of pairs, $(\mathbb{N} \times \mathbb{N}) \times (\mathbb{N} \times \mathbb{N})$ such that

$$\forall (p_1, q_1), (p_2, q_2) \in \mathbb{N} \times \mathbb{N}: (p_1, q_1)\  R \  (p_2, q_2) \iff p_1 + q_2 = p_2 + q_1$$

Then the set of all equivalence classes gives us a way to partition $\mathbb{N} \times \mathbb{N}$ and is in fact the integers, $\mathbb{Z} = \\{ [(p, q)]:  \forall p, q \in \mathbb{N} \\}$

We can visualize this by plotting all pairs $(p, q) \in \mathbb{N} \times \mathbb{N}$ and coloring them a different color based on their equivalence class.

![all pairs](/images/R/NxN.png){: .center}

Here each equivalence class (integer) corresponds to a different color. You can see that each class corresponds to a line with a different y-intercept. In fact the y-intercept of the line is the integer value of that line. 

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
Unfortunately I couldn't quite figure out how to get the colors to match up perfectly, some some of the classes are miscolored.

![rational axis](/images/R/Qaxis.png){: .center}

Notice that when $\theta= \frac{\pi}{2}$ the slope of the line is $+\infty$ and at $\theta = -\frac{\pi}{2}$ the slope of the line is $-\infty$.
Furthermore the vertical line corresponding to the y-axis is not defined, as this corresponds to division by 0.

### Arithmetic on the rationals

Now let's consider arithmetic over $\mathbb{Q}$ by defining the operation of addition, subtraction multiplication and division as follows:

1. $[(p_1,q_1)] + [(p_2, q_2)] = [(p_1 \cdot q_2 + p_2 \cdot q_1, q_1 \cdot q_2)]$
2. $[(p_1,q_1)] - [(p_2, q_2)] = [(p_1 \cdot q_2 - p_2 \cdot q_1, q_1 \cdot q_2)]$
3. $[(p_1,q_1)] \cdot [(p_2, q_2)] = [(p_1 \cdot p_2 , q_1 \cdot q_2)]$
4. $[(p_1,q_1)] \div [(p_2, q_2)] = [(p_1 \cdot q_2 , q_1 \cdot p_2)]$

Let's try and see that $1 \div 2 = -2 \div -4$ in $\mathbb{Q}$.

So again, we have to pick pairs to represent $1, 2, -2, -4$.

We'll pick the pairs $(1, 1), (2, 1), (-2, 1), (-4, 1)$ respectively.

Then $1 \div 2 $ should be given by $(1 \cdot 2, 1 \cdot 1) = (2, 1) $, and $-2 \div -4$ will be given by $(1 \cdot -4, -2 \cdot 1) = (-4, -2)$.

![arithemtic on q](/images/R/Qarithmetic.png){: .center}

We can see that the vectors $(2, 1)$ and $(-4, -2)$ have the same slope, and therefore the same equivalence class. This is the equivalence class for $\frac{1}{2}$. 

# The Reals $\mathbb{R}$
Unfortunately we can't use the same approach to construct $\mathbb{R}$. While the cardinality of the $\mathbb{N}, \mathbb{Z}, \mathbb{Q}$ are all equal, $\mathbb{R}$ is far more dense.

Instead we'll consider a way to partition the $\mathbb{Q}$ number line called a Dedekinde cut.

More precisely, a subset $r$ of $\mathbb{Q}$ is a cut if it satisfys these conditions: 

1. $r \neq \emptyset \land r \neq \mathbb{Q}$
3. $\forall x \in r \forall y \in \mathbb{Q} x \leq y \implies x \in r$
4. $r$ has no maximum $\neg \exists x \in \mathbb{Q} \forall y \in r : y \leq x  $
$\forall x \in \mathbb{Q} \setminus A \exists y \in \mathbb{Q}: x < y \land y \not \in A$

$\mathbb{R}$ is then defined as the set of all cuts of $\mathbb{Q}$. A real number can be thought of as the set of all rational numbers less than itself.

$$\mathbb{R} = \{ r \subset \mathbb{Q}: r \text{ is a cut }\}$$

Let's look at the cut that represents the rational number $1$ in the reals.

![r1](/images/R/R1.png){: .center}

And also the irrational number $\sqrt 2$.

![r2](/images/R/Rroot2.png){: .center}

### Arithmetic on the reals

We can define a well-ordering $\leq$ on the reals such that $\forall x,y \in \mathbb{R}: x \leq y  \iff x \subset y$. 

And this is supported visually, as $1 \leq \sqrt2$ since the set is smaller.

Addition in the reals is defined as 

$$ A + B  = \{a + b: a \in A \land b \in B \}$$

So adding together $ 1 + \sqrt 2$ looks something like this:

![raadd](/images/R/Raddition.png){: .center}

And we're finished! We've built from the basics of set theory by defining progressively more complicated number systems, working our way from $\mathbb{N} \to \mathbb{Z} \to \mathbb{Q} \to \mathbb{R}$, and we can add real numbers together.
