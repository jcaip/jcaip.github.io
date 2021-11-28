---
layout: page
permalink: /about/
---

Hi there!

I'm Jesse Cai, and welcome to my blog. 

There's not really a central theme here, I just write about stuff that I find interesting.  
Most of the posts  here are about machine learning / nlp. 

Some of the more interesting things I've written about:
<!--- Combining sentence embeddings and clique percolation to [cluster text](/Kernels-and-Cliques)-->
- Experimental and theoretical analysis of [sentence representations](/Quickthoughts)
- [Distributed gradient descent](/Distbelief) in PyTorch
- Constructing the [rational numbers](/Building-Q) ($\mathbb{Q}$) visually
- Using RNNs to [predict user activity](/Predicting-User-Submission)

<hr>

#### About me:

I currently work as a Senior Research Engineer at [Cultivate](https://cultivate.com/), a startup leveraging AI to managers better.  
Before I joined Cultivate I spent some time working at [Blend](https://blend.com/) and [JPL](https://www.jpl.nasa.gov/).  
I graduated from UCLA, where I researched representation learning for NLP with Professor [Kai-Wei Chang](http://web.cs.ucla.edu/~kwchang/). 

Please feel free to email me if you want to chat or have any questions!  
Or alternatively, if you have answers to any of the following questions definitely shoot me a line. 

- **Is it ever rational to be irrational?**

Suppose two players, $A$ and $B$ are playing a game that goes as follows: 
- Players $A$ and $B$ alternate turns and $A$ starts with 10 points and $B$ with 15.
- Each turn the player loses a point.
- Each turn the player can also bet X points for a 25% chance of winning $2X$ points and a 75% chance of losing the bet. So $E[X] = 0.75 X$.
- The game ends when one player loses by running out of points.

Obviously the "rational" strategy is not to bet - but then player $A$ is guaranteed to lose since $B$ can outlast them. So $A$ must make an irrational bet for a chance of winning. I wonder what the optimal strategy is for $A$ and how to model this mathematically.

- **Is relational knowledge distillation viable?**

See [here](https://arxiv.org/abs/1904.05068). Basically instead of doing distillation as a KL between $P(y \mid x, \theta_{teacher})$ and $P(y \mid x, \theta_{student})$ you can take a KL between the distribution of distances in a minibatch. I unsuccessfully tried to do this to distill BERT, so really interested in any new work / thoughts on this approach. 

- **What makes a manager *good* ?**

Happy to hear generals or specifics here. 
