---
published: True
layout: post
tags: [machine-learning, nlp, research]
title: Kernels, Cliques, and Agglomerative Clustering
---

This is about some interesting approach.

<!--more-->

I've been working on a way to identify topics/trends in the news, but at a pretty granular level. 

Basically, given a large corpus of short, single-sentence documents (think chats, news headlines, ) how can we find "clusters" or topics in them?

The way to do it now is BERT and then something like k-means, but this doesn't always work that well. It also precludes 

## Kernels and Similarity Scores

BERT has bad similarity scores, so we can use SentenceBERT - which is basically BERT but with better geometry. 

We define our kernel as just $x^Tx$ and this seems to work well, if we use SentenceBERT. We could also use BERT literally as our kernel by feeding in the two sentences, and seeing the NSP result, but this is way too slow. 

## Finding topics a naive approach:

Compute similarity matrix, for all pairs in the dataset - this takes quadratic time. 

then do some thresholding on the edge weights to get a graph and feed this into a run of the mill clique percolation algorithm

- Thresholding globally does not seem to work that well, 
- Thresholding per node does seem to work well.  ()

what we get out = very good clusters (emperically)

Okay but assume we have a large corpus - will this still work?

No because you won't be able to compute this big graph

### Scalability

But now computing a pairwise similarity score is $O(n^2)$ and not very scalable at all. 

So how do we get around this?

I'll make the claim for most real world data - the similarity matrix is actually quite sparse. 

Let's "order" the corpus - what this means is that you use a rough heurestic to rule out most comparisons:

For most cases time is a really good heurestic:

I also have used a jank heurestic called sorting the embedding - this means nothing if your embedding means nothing, but assuming you have a good feature extraction, then you are golden. 

So instead of computing all pairs of points, you just go along the diagonal

This is good because it turns O(n^2) to O(n) 

But what about the clique percolation algorithm?

- O(d/3) but then d/3 becomes fixed so this is also linear time

Yay we did it - but now we may have the problem where:

same topic, but at different times - should this be considered the same topic? 

for some people yes but for some people no. 

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

