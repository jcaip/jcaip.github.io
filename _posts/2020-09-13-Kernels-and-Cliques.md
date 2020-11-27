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

This post is about some old research I was working on at UCLA-NLP with Taboola about a year ago. 

<!--more-->

## Intro: Encoders, Feature Maps and Kernels 

I've written a lot about sentence encoders in the past, but the general idea is that we have a neural network $f$ that takes in a natural language sentence and turns it into a vector in $\mathbb{R}^d$.

A good encoder should map sentences with similar semantic meanings to similar directions in the vectorspace. For example: If we had a encoder that mapped to $\mathbb{R}^2$, we would expect something like this:

We can also think of $f$ as a feature map, which in turn defines a kernel

$$k(x, x') = \langle f(x), f(x') \rangle$$ 

Most of the time, these embeddings are just used for some downstream text classification task, but they are an incredibly versatile tool. 

For example: You can easily implement semantic search by encoding everything and then dotting with a encoded query vector to get similarity scores.

In particular I want to talk about a couple of techniques I've used to:
1. Find relationships within a corpus. 
2. Bootstrapping text classifiers



#### So what's a good choice for $f$?

Taking the mean BERT embedding as $f$ seems like a straightforward approach - **but this is actually a pretty bad kernel**.

I think that since BERT's training objective is masked language modeling, there's no explicit need to make $f(x)^Tf(x)$ meaningful.

BERT only cares about being able to predict the masked token given the context - so it can act like a hashmap, essentially mapping different contexts to a different direction in the vector space. 

However this means BERT can then map the context for dog and the context for cat in two random dimensions, so long as it can tell them apart. There's no need for it to map the context for dog and cat to be close. 

But if you take the dot product of any two random directions, you'll probably end up with 0, especially in higher directions - making BERT a bad kernel. 

Luckily there's a very easy way around this - we can use SentenceBERT, which is BERT but fine-tuned to be a good general purpose sentence encoder. Check out this table from SentenceBERT[[^1]].
![comparison on STS tasks](/images/research/sentence_bert_table.png){: .center}


# Finding relationships in a corpus

Given our kernel $k(x, x')$, we can compute the pairwise similarity matrix by encoding $X$ and dotting it with itself. 

$$f(X)^Tf(X) = S \in \mathbb{R}^{n \times n}$$


This matrix can actually tell you a lot about your dataset. For example, I sorted the 20 newsgroups dataset by label and then visualized the pairwise similarity matrix.

![Pairwise similarity matrix](/images/research/sim_matrix.png){: .center}

You can clearly see block structure along the diagonal! In fact, if I mark the limits of each category the block structure becomes even more evident

![Masked similarity matrix](/images/research/masked_matrix.png){: .center}

This leads to something that I'll touch a bit more on in my bootstrapping text classifiers post - but you can actually use this to get a pretty good sense of intra-class and inter-class variance.

For example, in the above example, 

```
['alt.atheism',
 'comp.graphics',
 'comp.os.ms-windows.misc',
 'comp.sys.ibm.pc.hardware',
 'comp.sys.mac.hardware',
 'comp.windows.x',
 'misc.forsale',
 'rec.autos',
 'rec.motorcycles',
 'rec.sport.baseball',
 'rec.sport.hockey',
 'sci.crypt',
 'sci.electronics',
 'sci.med',
 'sci.space',
 'soc.religion.christian',
 'talk.politics.guns',
 'talk.politics.mideast',
 'talk.politics.misc',
 'talk.religion.misc']
```

You can see a large `comp` section, which is related to `sci.crypt/sci.electronics. `

You can also see `alt.atheism` has a high correlation with `soc.religion.christian`, which you would expect. 

This pairwise similarity matrix can also be viewed as a adjacency matrix of a fully connected weighted graph. If we can parse this graph and find some structure then we might be able to identify topics this way. 

However, in practice working with a similarity matrix, like this is kind of infeasible, so instead we threshold it to turn this fully connected weighted graph to a sparse unweighted graph, such that the only edges between semantically similar pairs exists. 

Let's think about what a maximal clique is in this thresholded graph - it's a set of sentences that are all similar to each other -> it's a topic in a high-level sense, which is exactly what we want.

# Dealing with scale: Pt.I

There are two different rules that I tried:
- Thresholding globally does not seem to work that well - taking a 98% threshold
- Thresholding per node does seem to work well.  (take top k)

I mainly think this is due to the fact that a high similarity score does not imply relatedness but vice versa is true. So using ranking instead of a global comparison is beneficial. 

You can see this in the distribution of edge densities. 


We can use a clique-percolation community detection technique that finds all maximal cliques in the graph and then "percolates" them upward until they are as large as possible. Two cliques are percolated (merged) if they share all but 1 elements. 

Or more generally, we can take this graph and feed it to any number of community detection algorithm. 


### Sample Clusters

# Dealing with scale: Pt.II

Okay but assume we have a large corpus - will this still work?

Nope, since computing a pairwise similarity score is $O(n^2)$ and is not feasible for large datasets.. 

Also clique detection / percolation is exponential time. 

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

# References
[^1]: Sentence BERT: https://arxiv.org/pdf/1908.10084.pdf
[^2]: BERT: https://arxiv.org/abs/1810.04805
[^3]: ULM-Fit: https://arxiv.org/abs/1801.06146
[^4]: QuickThoughts: https://arxiv.org/pdf/1902.00267.pdf
[^5]: Word2Vec: https://arxiv.org/pdf/1902.00267.pdf
[^6]: Scikit-learn: https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html
[^7]: Sampling Matters in Deep Metric Learning: https://arxiv.org/pdf/1706.07567.pdf
[^9]: CURL: http://www.offconvex.org/2019/03/19/CURL/
