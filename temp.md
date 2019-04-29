---
layout: page
permalink: /temp/
---

Stuff I wanna remember

---
# DistBelief Part 2

1. First, we tried to use openmpi to run our code - good starting point. 
2. still need to profile code

I should be able to use starcluster to spin up an OpenMPI cluster. 

I just need to change the model to use model averaging instead of this param-request approach. 
also need to figure out their bipartite graph shit

- shouldn't use a gradient norm update.

something about ill conditioning

---
# Wasserstein Distance

https://www.youtube.com/watch?v=aEOuu75z694

## Notes about optimal transport

kl divergence measures overlap, but if they don't overlap at all we gain no more information. 

EMD/ Wassertien distance 

can we reformat this as a continuous problem? 

divergence grows linearly with how far the distribution is from each other. 

### Monge-Kantorovich Problem 

finding distance between mu and nu

X could be a mainfold/euclidean space 

Wasserstein barycenters provides good platonic form of MNIST data.

---
# Building The Reals $\mathbb{R}$

Unfortunately we can't use the same approach to construct $\mathbb{R}$. While the cardinality of the $\mathbb{N}, \mathbb{Z}, \mathbb{Q}$ are all equal, $\mathbb{R}$ is far more dense.

Instead we'll consider a way to partition the $\mathbb{Q}$ number line called a Dedekinde cut. 

More precisely, a subset $r$ of $\mathbb{Q}$ is a cut if it satisfys these conditions: 

1. $r \neq \emptyset \land r \neq \mathbb{Q}$
3. $\forall x \in r .\forall y \in \mathbb{Q}: x \leq y \implies x \in r$
4. $\forall x \in \mathbb{Q} \setminus A. \exists y \in \mathbb{Q}: x < y \land y \not \in A$

$\mathbb{R}$ is then defined as the set of all cuts of $\mathbb{Q}$.

$$\mathbb{R} = \{ r \subset \mathbb{Q}: r \text{ is a cut }\}$$

With this in mind let's look at the cut that represents the rational number $1$ in the reals. Again please note the axes should extend to $\pm \infty$.

![r1](/images/R/R1.png){: .center}

And also the irrational number $\sqrt 2$.

![r2](/images/R/Rroot2.png){: .center}

### Arithmetic on the reals
We can define a well-ordering $\leq$ on the reals such that $\forall x,y \in \mathbb{R}: x \leq y  \iff x \subset y$. 

Visually, we can see that the cut representing $1$ is indeed a subset of $\sqrt 2$, as $1 \leq \sqrt2$

We can define addition simply as

$$ A + B  = \{a + b: a \in A \land b \in B \}$$

So adding together $ 1 + \sqrt 2$ looks something like this:

![raadd](/images/R/Raddition.png){: .center}

---
# Research Log: Implementing QuickThoughts

Basically converted all the code to python3 for quick thoughts, but now I need to get the eval scripts set up.

and then I fucked up by chown -R jcaip / .. RIP, probably need to reinstall my system now


