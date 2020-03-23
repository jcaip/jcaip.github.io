---
published: True
layout: post
tags: [machine-learning, nlp, research]
title: Kernels, Cliques, and Agglomerative Clustering
---




Over the past couple of years, there's been a lot of work on learning better sentence representations. NLP has seen a revolution with the rise of BERT, transformers, and deep language models. 

These approaches have effectively ushured in a new age of transfer learning for NLP, much like it did for computer vision a couple of years prior. 

However, while there's been a bunch of work about **learning** representations, there hasn't been as much work on **using** these representations. 

In this post I describe a couple of algorithms that combine these dense transformer based encoders with exisiting work on community detection. 

<!--more-->

To be clear, by representations, I mean turning a sentence into a vector, but in a "meaningful" way. So that the semantic meaning of the sentence is directly tied to the vector. 

People use them as feature extractors, or add in a linear + softmax layer and then fine-tune the deep language model (BERT). 

Or people run k-means on the algorithms, which can work quite well. 

This is great, but I think you can do more with these deep neural representaitons.


Basically, given a large text corpus of sentences/documents I seek to find topics/trends. 

## Encoders, Feature Maps and Kernels 

Let's say we have a sentence encoder - a model $f$ that takes in a natural language sentence and turns it into a vector in $\mathbb{R}^n$. 

We can actually think of this model $f$ as a feature map, which defines a kernel

We can consider the mean BERT Embedding as our function $f$ and our kernel $k(x, x') = \langle f(x), f(x') \rangle$. 

Actually though, this is not that great of a kernel, if you compare $f$ with something like the mean word2vec vector, you'll find that the latter performs much better. 

You can see this on STS results, and this is kind of expected - while BERT's training objective is MLM, there's no explicit need to make $f(x)^Tf(x)$ meaningful so long as you can predict a word in a sentence. 

BERT basically only cares about being able to predict the masked token given the context - so it can act like a hashmap, essentially mapping different contexts to a different direction in the vectorspace. 

But it can do this randomly - this is the key: It can map the context for dog and the context for cat in two random directions, and so long as it can tell them apart that's good. There's no need for it to map the context for dog and cat to be close. 

But if you take the dot product of any two random directions, you'll probably end up with 0, especially in higher directions. And this is why BERT is a "bad kernel". 

To get around this, we can use SentenceBERT, a BERT model fine tuned on NLI dataset. This basically gives us a good single-sentence representations . There's a lot of possible options here but for the sake of argument let's just say we have a very good function $f$ that gives a good feature map and kernel. 

So given our very good encoder, and a large corpus of short, single-sentence documents (think chats, news headlines, ) how can we find "clusters" or topics in them?

The way to do it now is BERT and then something like k-means, but this doesn't always work that well. First off, you have to pick a $k$. 

## Finding topics a naive approach: Kernels and Cliques

So given this kernel, we can compute the pairwise similarity for every single example - this takes quadratic time. 

This pairwise similarity matrix actually also describes a weighted adjacency matrix. If we can parse the graph and find some structure then we might be able to identify topics this way. 

But it's way too hard to work on this full weighted graph, so to make things easier, we do some thresholding to turn this fully connected weighted graph to a sparse unweighted graph, such that the only edges that exist are those between two sentences (datapoints) that are similar. 

Let's think about what a maximal clique is in this graph- it's a set of datapoints (sentences) that are all similar to each other -> it's a topic in a high-level sense. This is exactly what we want.

In another sense, we can take this graph and feed it to a clique percolation algorithm for community detection. This basically finds all maximal cliques in the graph and then "percolates" them upword until they are as large as possible. 

Two cliques are percolated (merged) if they share all but 1 elements. 

#### So how to threshold?

I observe that the 

- Thresholding globally does not seem to work that well - taking a 98% threshold
- Thresholding per node does seem to work well.  (take top k)

I mainly think this is due to the fact that a high similarity score does not imply relatedness but vice versa is true. So using ranking instead of a global comparison is beneficial. 

So there ends up being nodes that are related to all of the other datapoints for some reason ???

#### Results

what we get out = very good clusters (emperically)

## But is it web-scale?

Okay but assume we have a large corpus - will this still work?

No because you won't be able to compute this big graph

### Scalability

But now computing a pairwise similarity score is $O(n^2)$ and not very scalable at all. 

also clique is exponential time, clique percolation is also exponential time. 


As an aside, this is why people gave up on kernel methods back in the day - SVMs used to be hot, now their not, and the main reason is because for most kernel methods you do need to compute this pairwise similarity matrix - so it works great for small-medium datasets, but once people started getting into big data (teenage sex) it didn't work so well. 


So in order to get a reasonably scalable algoritm we need to adress these two issues

#### Computing the graph

I'll make the claim for most real world data - the similarity matrix is actually quite sparse. 

Let's "order" the corpus - what this means is that you use a rough heurestic to rule out most comparisons:

For most cases time is a really good heurestic:

I also have used a jank heurestic called sorting the embedding - this means nothing if your embedding means nothing, but assuming you have a good feature extraction, then you are golden. 

So instead of computing all pairs of points, you just go along the diagonal

This is good because it turns O(n^2) to O(n), 

#### Clique percolation

But what about the clique percolation algorithm?

If we use the topk ranking then 

- O(d/3) but then d/3 becomes fixed so this is also linear time

## Experimental scalability results + clusters

### Other considerations
Yay we did it - but now we may have the problem where:

same topic, but at different times - should this be considered the same topic? 

for some people yes but for some people no. 

## Putting it all together

So you run a heirarchical clustering algorithm on the clusters given by the clique community detection.

Basically what you do can be thought of like this

1. Compute pairwise similarity score fat diagonal
2. Run clique percolation algorithm to get clusters/seed
3. Run algomerative clustering to merge in similar clusters (if you want to).


Why is this better than k-means on feature mappings: 
- No picking k! (Actually this is not true, there are still hyperparameters to tune)
- much more stable - 
- some control over min cluster size!
- qualitatively, the topics are much cleaner. 
- You can subsitute your own kernel method when comparing similarity scores ( include domain logic )

I haven't done a literature review on any of this stuff, so if people have thoughts/comments/ideas/complaints about credit please don't be shy :) 

