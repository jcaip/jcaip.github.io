---
published: True
layout: post
tags: [machine-learning, nlp, research]
title: Learning Embeddings from Cooccurence Matrices
---

This post is about a couple of different ways to learn sentence representations, inspired by contrastive learning.

<!--more-->

## Background

The goal when learning sentence representation is to learn a function $f$ that converts a sentence to a vector $v \in \mathbb{R}^d$. 

One way to do this is via **contrastive learning**, which tries to map "similar" datapoints close to each other while forcing "dissimilar" datapoints to be far apart.

A good explanation of contrastive learning can be found [here](https://gombru.github.io/2019/04/03/ranking_loss/). Or you can check out my previous posts about [Quickthoughts](/Quickthoughts) or [my research](/Summer-Research) for some more background. 

Contrastive learning is actually a quite general approach.

DeepMind released a paper in 2018 that uses contrastive learning to learn representations for a bunch of different domains - audio, text, images.

However, I think the rise of BERT and masked language modeling has pretty much replaced contrastive loss based text representations, at least from what I can tell. Although I have seen it used recently as a task in multi-task learning, [] [] []. 

I spent a lot of time trying to combine contrastive loss and BERT with mixed results. 

## Next Sentence Prediction

*So how can we get "similar" samples and "dissimilar" samples for contrastive learning?*

In Quickthoughts, Logeswaram and Lee try to learn good representations by trying to predict the next sentence over a corpus of books.

In other words, they assume that sentences next to each other tend to be related to each other, and thus are "similar" should be closer in the representation space. 

Sentences that are far away from each other in the corpus are considered "dissimilar".
Note that this relies upon an ordered, structured corpus - so no IID assumption. 

More precisely, for a given corpus $D$, and sentences $s_i \in D$, we try to model the following probability distribution:

$$\prod_{s_i \in D}  P(s_{i+1} \mid s_i, D)$$

Our true probability distribution then looks something like this:

$$
target = \begin{bmatrix} 
0  & 1 &  \dots & 0 \\
0  & \ddots & \ddots &  0 \\
\vdots  & & & 1  \\
0  &  &  &  0 \\
\end{bmatrix} \in \mathbb{R}^{n \times n}
$$

QT attempts to model this probability distribution with two encoder networks, in this case, BiLSTM RNNs. We use these RNNS, $f, g$ to encode our sentences, and take an outer product between these two vectors in order to get our scores matrix. 

We then row-wise softmax our to turn our scores matrix into a probability distribution

$$
scores = \begin{bmatrix} 
f(s_1) \\
\vdots \\
f(s_n) \\
\end{bmatrix}
\begin{bmatrix} 
g(s_1) \dots g(s_n) \\
\end{bmatrix} =
\begin{bmatrix}
f(s_1)^Tg_(s_1)  & \ldots & f(s_1)^Tg(s_n)\\
\vdots  & \ddots & \\
f(s_n)^Tg_(s_1)  & & f(s_n)^Tg(s_n) \\
\end{bmatrix} \in \mathbb{R}^{n \times n}
$$

$$ \hat{P}(s_{cand} \mid s, S_{cand}) = \frac{\exp (f(s)^T g(s_{cand}))}{\sum_{s' \in S_{cand}} \exp ( f(s)^Tg(s') )}$$

To calculate our loss, we take the KL div between the normalized scores matrix and the targets matrix. 
Each row $i$ represents the probability distribution $P(s_i \mid s_i, D)$, so we take the KL loss of each row and then average across the corpus to get the final loss.

$$
loss = KL 
\left(
\begin{bmatrix}
f(s_1)^Tg_(s_1)  & \ldots & f(s_1)^Tg(s_n)\\
\vdots  & \ddots & \\
f(s_n)^Tg_(s_1)  & & f(s_n)^Tg(s_n) \\
\end{bmatrix}
, \begin{bmatrix} 
0  & 1 &  \dots & 0 \\
0  & \ddots & \ddots &  0 \\
\vdots  & & & 1  \\
0  &  &  &  0 \\
\end{bmatrix} 
\right)
$$

Quick note - we actually don't do this pairwise for every sentence in the corpus, because that's computationally infeasible. Bookcorpus is ~68 million sentences, so if we did this across the whole corpus we'd need to model approximately  $n^2 = 4.525 \times 10^{15}$ probabilities. 

Instead we only compare pairwise across all sentences in a minibatch. 

This okay though, because points that are far away, will contribute very little to the KL loss, so we can effective ignore them.

I find this to be very similar to the approach taken in Barnes-Hut t-SNE, which only considers the $n$ nearest neighbors to turn t-SNE from a $O(n^2)$ runtime to a $O(n \log n)$ runtime..

There's a couple great explanations of the Barnes-Hut approximation, which is also used in particle physics [here]() and [here]().


*What's the core idea behind this approach?*

**Sentences next to each other tend to be related to each other, so they should be closer to each other in the embedding space.**

This is great, but taking sentences that are next to each other as similar is just a heuristic, so I thought there might be different heuristics that we can use to mine samples, which corresponds to changing our targets matrix. 

I started by trying to use the document-document word cooccurence matrix, the intuition being that if a sentence shares words with another sentence it's likely to be similar in meaning.

## Document-Document Word Cooccurence Matrix

This is a matrix $V \in \mathbb{Z}^{b \times b}$ where $b$ is the batch size and $V_{i,j} = $ the number words shared between $s_i$ and $s_j$.

There's a pretty quick and easy way to genenerate this matrix, by taking a binary BOW encoding and dotting it with itself. 

The binary BOW encoding maps each sentence to a vector $s \in \mathbb{R}^{\lvert V \rvert}$ where $V$ is the vocabulary. This is a binary vector where $v_j = 1$  if $v_j$ is in the sentence, and 0 otherwise. 
Scikit-learn provides a simple way to get this matrix, by using [CountVectorizer](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.CountVectorizer.html).


```python
tokenizer = BertTokenizer.from_pretrained('bert-base-uncased')
vectorizer = CountVectorizer(tokenizer=tokenizer.tokenize, vocabulary=tokenizer.vocab, binary=True)

def generate_cooccurence_counts(data, vectorizer):
    bow = vectorizer.transform(data)
    targets = bow.dot(bow.T)
    return targets
```

It might be helpful to consider a toy example. Consider a corpus of the following three sentences:

1. I like dogs.
2. The dogs barked.
3. Dogs like bones.


For this example the vocabulary is then 

$$ 
Vocabulary = [\text{I}, \text{like}, \text{dogs}, \text{The}, \text{barked}, \text{bones}]
$$

and after taking the BOW encoding we have the matrix 

$$
BOW = \begin{bmatrix}
    1 & 1 & 1 & 0 & 0 & 0 \\
    0 & 0 & 1 & 1 & 1 & 0 \\
    0 & 1 & 1 & 0 & 0 & 1 \\
\end{bmatrix}
$$

After we dot this matrix with it's transpose we get our unnormalized targets matrix.

$$
targets = BOW(BOW^T) = \begin{bmatrix}
    3 & 1 & 2 \\
    1 & 3 & 1 \\
    2 & 1 & 3 \\
\end{bmatrix}
$$

We can do what we did before and row-wise softmax to turn this targets into a probability distribution and then take a KL loss between this targets matrix and our scores matrix, which is the same as above.

$$
loss = KL 
\left(
\begin{bmatrix}
f(s_1)^Tg_(s_1)  & \ldots & f(s_1)^Tg(s_n)\\
\vdots  & \ddots & \\
f(s_n)^Tg_(s_1)  & & f(s_n)^Tg(s_n) \\
\end{bmatrix}
,  \begin{bmatrix}
    3 & 1 & 2 \\
    1 & 3 & 1 \\
    2 & 1 & 3 \\
\end{bmatrix}

\right)
$$

*What's the core idea of this approach?*

**Sentences that share words with each other tend to be related to each other, so they should be closer to each other in the embedding space.**

Just using this for the targets matrix actually doesn't leave to great performance. 

I think it's because the new targets distribution is quite smooth, which makes the model output the same vector for all kinds of different things (more on this later).

To do this I did some softmax [temperature scaling]() so make the distribution more peaky, which seemed to help.

#### $L^2$ Loss

As a side note, I also tried just using the L2 loss between the two unnormalized matrices. This is very similar to the approach described in [GloVe]() from what I can tell, except instead of trying to learn word vectors, we try to use sentence vectors. There's also no weighting factor involved here, because unlike the set of all words, which we can consider to be finite, the set of all sentences is infinite. 

$$
loss =  
\left(
\begin{bmatrix}
f(s_1)^Tg_(s_1)  & \ldots & f(s_1)^Tg(s_n)\\
\vdots  & \ddots & \\
f(s_n)^Tg_(s_1)  & & f(s_n)^Tg(s_n) \\
\end{bmatrix}
-  \begin{bmatrix}
    3 & 1 & 2 \\
    1 & 3 & 1 \\
    2 & 1 & 3 \\
\end{bmatrix}

\right)^2
$$

I like this L2 loss formulation though, because it kind of gives us some intuition as to why we learn good sentence embeddings with this approach. 
In the L2 loss fomulation we're kind of finding a dense representation of the BOW vector of the sentence.


## Using TF-IDF Matrices

Obviously, raw word counts are also not perfect, so I tried to extended this by using TF-IDF weighted similarity scores, by switching out `CountVectorizer` with `TfIdfVectorizer`. 

```python
tokenizer = BertTokenizer.from_pretrained('bert-base-uncased')
vectorizer = TfIdfVectorizer(tokenizer=tokenizer.tokenize, vocabulary=tokenizer.vocab)

def generate_cooccurence_counts(data, vectorizer):
    bow = vectorizer.fit_transform(data)
    targets = bow.dot(bow.T)
    return targets
```
What this essentially does is weight the words by their frequency. So if a document shares a uncommon word, they tfidf dot will be larger. 

Again, I normalize the given targets matrix and take a KL loss. 

$$
loss = KL 
\left(
\begin{bmatrix}
f(s_1)^Tg_(s_1)  & \ldots & f(s_1)^Tg(s_n)\\
\vdots  & \ddots & \\
f(s_n)^Tg_(s_1)  & & f(s_n)^Tg(s_n) \\
\end{bmatrix}
,  \begin{bmatrix}
    3 & 1 & 2 \\
    1 & 3 & 1 \\
    2 & 1 & 3 \\
\end{bmatrix}
\right)
$$

*What's the core idea of this approach?*

**Not all words are created equal. Sentences that share rare words are more likely to be similar to each other than sentences that share common words, and thus should be closer in the embedding space.**

Again I was running into a problem with the smoothness of the target distribution. I did do the softmax temerature scaling, but I was wondering if there was a better approach to deal with this problem. 

### Binarized TFIDF Matrices 

TF-IDF makes the distribution too smooth. Smoothness of the target distribution is actually very bad for us. 

It will make all the sentence map to the same vector.

To counteract this, I stopped minimizing the KL-divergence between the scaled TF-IDF matrix and scores matrix, and instead used the TF-IDF matrix as a heuristic to mine samples. 

```python
tfidf_mat= torch.Tensor(generate_tfidf_counts(raw_data, vectorizer)).cuda()
max_indicies = torch.argmax(tfidf_mat, dim=0)
targets = F.one_hot(indicies, num_classes=args.batch_size).float()
```

This creates a one-hot vector where the index is 1 where the max TF-IDF score is, and 0 everywhere else. Basically I'm making the distribution as peaky as possible manually. This is actually extremely similar to the orginal QT approahc, so I thought it might help a bit. 

*What's the core idea of this approach?*

**Smoothness leads to sameness. Instead of scaling the targets distribution to be sharper, we can create a sharp distribution manually**.


## Results

To evaluate performance I trained a couple of  models, all with a bidirectional GRU encoder and tried to see the results.

#### SentEval STS Spearman

|                              | STS12 | STS13 | STS14 | STS15 | STS16 | SICKr | STSb | 
|------------------------------|-------|-------|-------|-------|-------|-------|------|
|Next sentence prediction      | 0.374 | 0.352 | 0.359 | 0.393 | 0.389 | 0.751 | 0.649|
|Co-occurrence counts (Temp=10)| 0.394 | 0.301 | 0.437 | 0.524 | 0.516 | 0.728 | 0.602| 
|TFIDF matrix (Temp=10)        | 0.395 | 0.307 | 0.396 | 0.461 | 0.405 | 0.730 | 0.674| 
|binarized TFIDF               | 0.464 | 0.451 | 0.496 | 0.547 | 0.550 | N/A   | N/A  | 

If you have comments, questions, or corrections please LMK. Or if you have some random idea that seems tangentially related. I feel like this is close to being an effective approach, but I think I'm missing something.



