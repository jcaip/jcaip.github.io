---
layout: page
permalink: /about/
---

Hi there!

I'm Jesse Cai, and this is my blog, where I write about my research in natural language processing and other interesting ideas in mathematics, computer science, and machine learning.
Most of what I write here is technical, but sometimes I talk about my experiences in industry/academia.

If you're new here I'd recommend reading the following posts:
- Experimental and theoretical analysis of [sentence representations](/Quickthoughts)
- [Distributed gradient descent](/Distbelief) in PyTorch
- Constructing the [rational numbers](/Building-Q) visually
- Using RNNs to [predict user activity](/Predicting-User-Submission)

I'm currently a ML/NLP researcher at UCLA, where I'm part of [UCLA-NLP](http://web.cs.ucla.edu/~kwchang/).

My main research interest is how to leverage unlabeled but structured data to learn better sentence embeddings.

In layman's terms, I'm interested in finding a way to take a sentence and change it into a bunch of numbers ( a vector in $\mathbb{R}^n$), but in a way so that if you plotted sentences with similar meanings, they would be close to each other. In two dimensions it'd look something like this: 

<img src="/images/ex.png" alt="example" width="400" class="center"/>

Why is this important? Machine learning is fundamentally a combination of **representation** and **optimization**, so having representations that make our optimization problems easier can lead to better performance. 

Optimization is essentially rolling a ball down a hill until it reaches a valley - it's easy when you have a spherical ball and a smooth hill, but it becomes a lot harder if your ball is a cube (cubes don't roll very well). 

In actuality, changing the representation changes the hill rather than the ball, but for the sake of analogy it's easier to consider the opposite. 

We want representations generalize well, so that they roll down *any* hill. To do this we need to train over a lot of data, but we're in luck - humans have created a huge amount of natural language data. This data is not labeled, but most of it is structured - into sentences, paragraphs, and books. I hope to exploit this structure as weak supervision in order to learn representations.

I'm also interested in transfer learning, domain-adaptation approaches, and probabilistic models. 

Prior to this I took a year off to join the data team at [Blend](https://blend.com), where I used deep learning to identify app quality issues from NPS comments. Before that I interned at [JPL](https://www.jpl.nasa.gov/).


In my free time I like lifting, exploring nature, and making bad decisions with good friends.

If you want to chat about anything feel free to reach out. 

