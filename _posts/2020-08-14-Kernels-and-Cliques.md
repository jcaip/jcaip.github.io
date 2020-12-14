---
published: False
layout: post
tags: [machine-learning, nlp, research]
title: Kernels and Cliques 

images:
    - url: https://upload.wikimedia.org/wikipedia/commons/thumb/a/a1/Illustration_of_overlapping_communities.svg/1920px-Illustration_of_overlapping_communities.svg.png
    - alt: Clique
    - title: Clique
---

This is a post about some research I never got around to finishing. 

The basic idea here is to combine transformer based sentence embeddinsg with existing community detection techniques. 

<!--more-->

## Introduction

## Encoders, Feature Maps and Kernels 

Let's say we have a sentence encoder $f$ that takes in a natural language sentence and turns it into a vector in $\mathbb{R}^d$. 

We can think of $f$ as a feature map, which defines a kernel

$$k(x, x') = \langle f(x), f(x') \rangle$$ 

So what's a good choice for $f$?

Taking the mean BERT embedding as $f$ seems like a straightforward approach - but this is actually a pretty bad kernel!

If you compare $f$ with something like the mean word2vec vector, you'll find that the latter performs much better on STS tasks.

**Check out this table from SentenceBERT[[^1]].**
![comparison on STS tasks](/images/research/sentence_bert_table.png){: .center}

I think that since BERT's training objective is masked language modeling, there's no explicit need to make $f(x)^Tf(x)$ meaningful.

BERT basically only cares about being able to predict the masked token given the context - so it can act like a hashmap, essentially mapping different contexts to a different direction in the vectorspace. 

However this means BERT can then map the context for dog and the context for cat in two random directions, and so long as it can tell them apart. There's no need for it to map the context for dog and cat to be close. 

But if you take the dot product of any two random directions, you'll probably end up with 0, especially in higher directions. And this is why BERT is a "bad kernel". 

To get around this, we can use SentenceBERT, which is BERT but tuned to be a good general purpose sentence encoder.

## Finding topics a naive approach: Kernels and Cliques

So given we have our encoder, SentenceBERT, and a large corpus of short, single-sentence documents (think chats, news headlines, etc. ) how can we find "clusters" or topics in them?

We can do something like k-means on the embedding and see what we get. 

Given our kernel $k(x, x')$, we can compute the pairwise similarity matrix by encoding $X$ and dotting it with itself. 

$$f(X)^Tf(X) = S \in \mathbb{R}^{n \times n}$$

This pairwise similarity matrix can also be viewed as a adjacency matrix of a fully connected weighted graph. If we can parse this graph and find some structure then we might be able to identify topics this way. 

However, it's way too hard to work on this fully connected weighted graph, so we first do some thresholding to turn this fully connected weighted graph to a sparse unweighted graph, such that the only edges between semantically similar pairs exists. 

Emperically, I saw that 

- Thresholding globally does not seem to work that well - taking a 98% threshold
- Thresholding per node does seem to work well.  (take top k)

I mainly think this is due to the fact that a high similarity score does not imply relatedness but vice versa is true. So using ranking instead of a global comparison is beneficial. 

You can see this in the distribution of edge densities. 

Let's think about what a maximal clique is in this thresholded graph - it's a set of sentences that are all similar to each other -> it's a topic in a high-level sense, which is exactly what we want.

We can use a clique-percolation community detection technique that finds all maximal cliques in the graph and then "percolates" them upward until they are as large as possible. 
Two cliques are percolated (merged) if they share all but 1 elements. 

Or more generally, we can take this graph and feed it to any number of community detection algorithm. 


#### Results

#### Sample Clusters

### Scalability

Okay but assume we have a large corpus - will this still work?

Nope, since computing a pairwise similarity score is $O(n^2)$ and is not feasible for large datasets.. 

Also clique detection / percolation is exponential time. 

As an aside, this is part of the reason why people gave up on kernel methods back in the day - SVMs used to be hot, now their not, and one reason is because for most kernel methods you do need to compute a pairwise similarity matrix - so it works great for small-medium datasets, but once people started getting into big data these approaches are hard to scale. 

So in order to get a reasonably scalable algoritim we need to address these two issues.

#### Computing the graph

I'll make the claim for most real world data - the similarity matrix is actually quite sparse. 

Let's "order" the corpus - what this means is that you use a rough heurestic to rule out most comparisons:

For most cases time is a really good heurestic:

I also have used a jank heurestic called sorting the embedding - this means nothing if your embedding means nothing, but assuming you have a good feature extraction, then you are golden. 

So instead of computing all pairs of points, you just go along the diagonal

This is good because it turns $O(n^2)$ to $O(n)$, 

#### Clique percolation

But what about the clique percolation algorithm?

If we use the top-k ranking then 

- O(d/3) but then d/3 becomes fixed so this is also linear time

## Experimental scalability results + clusters

| $\vert{X}\vert$ | Time (s)    | # Clustered Documents |
|-----------------|-------------|-----------------------|
| 5000            | 38.22       | 1845                  |
| 10000           | 73.44       | 3736                  |
| 50000           | 378.25      | 17986                 |
| 100000          | 750.79      | 35187                 |

![scalability](/images/research/scalability.png)

### Other considerations
Yay we did it - but now we may have the problem where:

same topic, but at different times - should this be considered the same topic? 

for some people yes but for some people no. 

## Putting it all together

So you run a heirarchical clustering algorithm on the clusters given by the clique community detection.

Basically what you do can be thought of like this

1. Compute pairwise similarity score fat diagonal
2. Run clique percolation algorithm to get clusters/seed

Why is this better than k-means on feature mappings: 
- No picking k! (Actually this is not true, there are still hyperparameters to tune)
- much more stable - 
- some control over min cluster size!
- qualitatively, the topics are much cleaner. 
- You can subsitute your own kernel method when comparing similarity scores ( include domain logic )
-
#### 1/13/2020
I just watched this lecture by Andrew W. Lo titled [Artificial Stupidity](https://www.youtube.com/watch?v=zqw1nmJ7XZM&t=3957s)

Basically the idea is that we need AI to understand human behavior, 

Economists assume human behavior is purely rationaly (expected utility maximization) but this isn't true. 
Some other guy proposed a different model of behavior - essential we build a heurestic and procedurally refine it (Simons)

New AI (deep learning) is more similar to this approach than old AI (expert systems). 

Lo wrote a paper trying to use machine learning to predict when/if a person will panic sell, and he want's to use this to create personalized finance. 

I'm a little money grubbing shit at heart, so I was thinking -

Instead of predicting which stocks will go up, why not try to predict when people will panic. 

Because if you predict a panic successfully, you can buy the dip. 


#### 1/23/2020
Equitable Valuation of Data:

Shapley Value
- nullness
- symmetry
- linearity

Better than leave one out because doesn't depend of relative similarity of examples. 

MC approximation - sample random permutation of training samples. 

Then train - at each gradient update you can get marginal improvement PERF(a, b, c) - perf(a, b) by subtracting previousl performance.

applications:
small mumber of samples - can be used to clean a dataset

data shapley can be computed efficiently for KNN. 

use dnn for feature then use knn

#### 1/28/2020
SBM (n, p, W)
n - number of vertices
p - probability 

# References
[^1]: Sentence BERT: https://arxiv.org/pdf/1908.10084.pdf
[^2]: BERT: https://arxiv.org/abs/1810.04805
[^3]: ULM-Fit: https://arxiv.org/abs/1801.06146
[^4]: QuickThoughts: https://arxiv.org/pdf/1902.00267.pdf
[^5]: Word2Vec: https://arxiv.org/pdf/1902.00267.pdf
[^6]: Scikit-learn: https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html
[^7]: Sampling Matters in Deep Metric Learning: https://arxiv.org/pdf/1706.07567.pdf
[^9]: CURL: http://www.offconvex.org/2019/03/19/CURL/
