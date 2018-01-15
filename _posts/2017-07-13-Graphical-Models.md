---
layout: post
tags: [bayesian-networks, machine-learning, probability]

images: 
  url: /images/medimg/markov.png
  alt: markov
  title: markov
---

A method of representing uncertatainty and reasoning. To do this generate a graph, and set probabilities for each node in the graph.

![graphical](/images/medimg/graphical_model.png)

In all cases, nodes are random variables and the graph species a coarse structure of a joint distribution. These graphs have properties of conditional independence from the nodes above their parents.

#### Markov Chains
Conditioned on the present, the past and future are independent.
![markov](/images/medimg/markov.png)

It's important to note that graph seperation implies conditional independence.
#### Naive-Bayes
![nbayes](/images/medimg/nbayes.png)

**inference** takes in a model with known paramaters and estimates the hidden variables

**learning** takes in multiple data instances and estimates the paramaters for a probabilistic model.

#### Sum-Product Belief Propogation
![sp](/images/medimg/sp.png)

A node may only send a message to a neighbor when it has recieved incoming messages from all of its other neighbors. These messages can either be discrete or continuous (belief propogation)

This can be used to detect the relative postion between features.
![stuff](/images/medimg/examples_toy.png)

