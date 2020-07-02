---
title: Visual Diffe-Hellman Key Exchange
tags: [math, theory]
published: True
layout: post
---

<!--more-->

Recently I've been learning some abstract algebra, most of this has been about cyclic groups, and I wanted to understand how this math can be used in the real world. 

One perfect example of this is Diffe-Hellman key exchange.

Diffe Hellman key exchange allows two individuals to agree upon a secret shared key without allowing a eavesdropper to figure out that key. 



Say Alice and Bob want to communicate, but they a third party can see anything they send between each other. 
They each have a box and Alice wants to send a gold ring to Bob. They each have their own respective padlocks and keys. 

How can they safely get the ring to Bob without it getting stolen?

So Alice can put the ring in her box, lock it and then send it to Bob. 
But Bob can't open it, as he doesn't have Alice's key. 

Alice needs to somehow send her key to Bob, but if she just sends the key in the box without locking it, the postman can make a copy and then when Alice sends the locked box with the ring, Charlie can steal the ring. 



What Bob can do is first put his padlock in his box and send it to Alice. 

Alice can then take his padlock and use it to lock her box with her key in it and send it to Bob

Bob then unlocks his own padlock takes the key out, and sends the box back to alice

Who puts the ring in the box and locks it, then sends it. 


The idea here is to use **one-way** functions. 

These are functions that are easy to compute but hard to invert. 


However, Diffie-Hellman key exchange can be generalized to finite cyclic groups - since 

1. Alice and Bob agree on a finite cyclic group $G$ of order $n$ and a generating element $g \in G$. (This is usually done long before the rest of the protocol; $g$ is assumed to be known by all attackers.)
2. Alice picks a random natural number $a$, where $1 < a < n$, and sends $g^a$ to Bob.
3. Bob picks a random natural number $b$, which is also $1 < b < n$, and sends $g^b$ to Alice.
4. Alice computes $(g^b)^a$.
5. Bob computes $(g^a)^b$.

So we can consider Diffie-Hellman key exchange on the group $G$ = the nth roots of unity.

And something very nice about the nth roots of unity are that 
1. They are easy to visualize
2. multiplication on them is easy to visualize. 

Let's consider $n=4$. 

Then the four elements are $G = \\{ 1, i, -1,  -i \\}$ and the group table looks something like this:


|    | 1  | i  | -1 | -i |
|----|----|----|----|----|
| 1  | 1  | i  | -1 | -i |
| i  | i  | -1 | -i | 1  |
| -1 | -1 | -i | 1  | i  |
| -i | -i | -1 | i  | 1  |

If you notice there's something patternly about this table - multiplication by the nth root of unity is the same thing as rotation by n steps. 

This is much easier to visualize if instead of writing out the roots of unity in complex form, we just call them $\mathbb{1}, \mathbb{2} \ldots \mathbb{n}$.


In fact **every** finite cyclic group of order n is isomorphic to the $\mathbb{Z} \\ n \mathbb{Z}$ or multiplication modulo n, which is what DH conventionally acts on. 

So we'll define a mapping from multiplication on the naturals mod n to the roots of unity, where multiplication is replaced by rotation. 

In fact rotation is really just addition in the phase space, which makes sense because there's a logarithm in the root of unity equation. 

It's a bit easier to see this visually. 

Say we want to multiply 3 and 5. 

From what we know before, 3 and 5 map to $\mathbb{3}$ and $\mathbb{5}$.


Imagine you and a friend are on different rocks in a circle. IT is pitch black and there is a crocodile that wants to eat you, 

You two want to arrive on the same spot but if you say which spot then the crocodile will come and eat you. 

Is there a way for you to say a number and your friend to say a number so that the crocodile doesn't know where to go to eat you, but you and your friend do?


