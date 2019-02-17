---
published: False
layout: post
title: Building R
tags: [theory, mathematics]
---

This is a bit of a new kind of post, it's about mathematics instead of computer science. 

I'm going to go over how to construct $\mathbb{R}$, or the real numbers. 

Of course, we have to start from somewhere - in this case, we'll start with the natural numbers, $\mathbb{N}$ defined by the **Peanno Axioms**. 

It's kind of weird to think about, but R is really a mathematical construction. 

So I'll try to describe how R is constructed, starting from the naturals. 


First well derive the integers, and then from the integers, we will construct the rationals, and from the rationals we will create the reals. 

So first we define an equivalence relation
A way to partition elements in a set so that any element from the set represents the entire set. 

So how should we define Z?

We take N and consider all pairs in N. We define a relation R such that $(p_1, q_1) ~ (p_2, q_2) $ 
if $p_1 -q_1 = p_2 - q_2$

$Z$ is the set of all equivalence classes in this construction. 

Now we have $Z$.  We'll do something similar to construct Q

We take Z and consider all pairs in Z. We define a relation R such that $(p_1, q_1) ~ (p_2, q_2) $ if $p_1 (q_2) = p_2 ( q_1)$


