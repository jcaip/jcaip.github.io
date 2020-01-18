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

Ideally the meaning of a sentence should determine where in $\mathbb{R}$ it lies. 

One way to do this is via **contrastive learning**, which tries to map "similar" datapoints close to each other while forcing "dissimilar" datapoints to be far apart.

A good explanation of contrastive learning can be found [here](https://gombru.github.io/2019/04/03/ranking_loss/). Or you can check out my previous posts about [Quickthoughts](/Quickthoughts) or [my research](/Summer-Research) for some more background. 

Contrastive learning is actually a quite general approach:

DeepMind released a [paper](https://arxiv.org/abs/1807.03748) in 2018 that uses contrastive learning to learn representations for a bunch of different domains - audio, text, images.

However, I think the rise of BERT and masked language modeling has pretty much replaced contrastive loss based text representations, at least from what I can tell. Although I have seen it used recently as a [task in multi-task learning](https://arxiv.org/pdf/1901.11504.pdf) in conjunction with MLM loss.

## Next Sentence Prediction

*How can we get "similar" samples and "dissimilar" samples for contrastive learning?*

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

$$
scores_{unnormalized} = \begin{bmatrix} 
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

We then zero out the identity diagonal and row-wise softmax our to turn our scores matrix into a probability distribution

$$
scores = \begin{bmatrix} 
\frac{1}{Z_{s_1}}  & \frac{e^{f(s_1)^Tg(s_2)}}{Z_{s_1}} & \dots & \frac{e^{f(s_1)^Tg(s_n)}}{Z_{s_1}} \\
\frac{e^{f(s_1)^Tg(s_2)}}{Z_{s_2}} &  \frac{1}{Z_{s_2}}\\
\vdots & & \ddots & \\
\frac{e^{f(s_1)^Tg(s_n)}}{Z_{s_n}} & & & \frac{1}{Z_{s_n}}\\
\end{bmatrix}
$$

$$ scores_{i+1, i} = \hat{P}(s_{i+1} \mid s_i, D )  \propto f(s_{i+1})^T g(s_{i}) $$

To calculate our loss, we take the KL div between the normalized scores matrix and the targets matrix. 
Each row represents a probability distribution, so we take the KL loss of each row and then average across the corpus to get the final loss.

$$
loss = KL 
\left(
\begin{bmatrix} 
0  & 1 &  \dots & 0 \\
0  & \ddots & \ddots &  0 \\
\vdots  & & & 1  \\
0  &  &  &  0 \\
\end{bmatrix} 
,

\begin{bmatrix} 
\frac{1}{Z_{s_1}}  & \frac{e^{f(s_1)^Tg(s_2)}}{Z_{s_1}} & \dots & \frac{e^{f(s_1)^Tg(s_n)}}{Z_{s_1}} \\
\frac{e^{f(s_1)^Tg(s_2)}}{Z_{s_2}} &  \frac{1}{Z_{s_2}}\\
\vdots & & \ddots & \\
\frac{e^{f(s_1)^Tg(s_n)}}{Z_{s_n}} & & & \frac{1}{Z_{s_n}}\\
\end{bmatrix}

\right)
$$

Just like in any other machine learning algorithm, we minimize this loss with SGD. 

Quick note - we actually don't compute this matrix pairwise for every sentence in the corpus, because that's computationally infeasible. Bookcorpus is ~68 million sentences, so if we did this across the whole corpus we'd need to model approximately  $n^2 = 4.525 \times 10^{15}$ probabilities.

Instead we only compare pairwise across all sentences in a minibatch. 

This is okay because points that are far away will contribute very little to the KL loss, so we can effective ignore them.

I find this to be very similar to the approach taken in Barnes-Hut t-SNE, which only considers the $n$ nearest neighbors to turn t-SNE from a $O(n^2)$ runtime to a $O(n \log n)$ runtime..

You can read more about the Barnes-Hut approximation [here](https://www.microsoft.com/en-us/research/blog/optimizing-barnes-hut-t-sne/) and [here](http://lvdmaaten.github.io/publications/papers/JMLR_2014.pdf), which is also used in particle physics. It might help provide some intuition. 


*What's the core idea behind this approach?*

**Sentences next to each other tend to be related to each other, so they should be closer to each other in the embedding space.**

This is great, but taking sentences that are next to each other as similar is just a heuristic, so I thought there might be different heuristics that we can use to mine samples.
This corresponds to changing our targets matrix. 

I started by trying to use the document-document word cooccurence matrix, the intuition being that if a sentence shares words with another sentence it's likely to be similar in meaning.

## Document-Document Word Cooccurence Matrix

This is a matrix $V \in \mathbb{Z}^{b \times b}$ where $b$ is the batch size and $V_{i,j} = $ the number words shared between $s_i$ and $s_j$.

There's a pretty quick and easy way to genenerate this matrix, by taking a binary BOW encoding and dotting it with itself. If you're unfamiliar with bag-of-words encoding you can read more about it [here](https://en.wikipedia.org/wiki/Bag-of-words_model).

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

and after taking the BOW encoding we have the matrix. Note that this matrix is "binarized" so instead of being the counts, we just have an indicator variable that signifies if the word is present in the sentence or not.

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

We can do what we did before and row-wise softmax after zeroing the diagonals to turn this targets into a probability distribution and then take a KL loss between this targets matrix and our scores matrix, which is computed the same as in next sentence prediction.

$$
loss = KL 
\left(

\sigma \left(
\begin{bmatrix}
    0 & 1 & 2 \\
    1 & 0 & 1 \\
    2 & 1 & 0 \\
\end{bmatrix}
\right)
, 
\begin{bmatrix}
f(s_1)^Tg_(s_1)  & \ldots & f(s_1)^Tg(s_n)\\
\vdots  & \ddots & \\
f(s_n)^Tg_(s_1)  & & f(s_n)^Tg(s_n) \\
\end{bmatrix}
\right)
$$

*What's the core idea of this approach?*

**Sentences that share words with each other tend to be related to each other, so they should be closer to each other in the embedding space.**

This actually leads to pretty bad performance out of the box. 

I think it's because the new targets distribution is quite smooth, which makes the model output the same vector for all kinds of different things (more on this later).

To do this I used softmax [temperature scaling](https://geoffpleiss.com/nn_calibration) to make the targets distribution more peaky, which seemed to help.

#### $L^2$ Loss

As a side note, I also tried just using the L2 loss between the two unnormalized matrices. This is very similar to the approach described in [GloVe](https://nlp.stanford.edu/pubs/glove.pdf) from what I can tell, except instead of trying to learn word vectors, we try to use sentence vectors. There's also no weighting factor involved here, because unlike the set of all words, which we can consider to be finite, the set of all sentences is infinite. 

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

The loss is zero when the scores and targets matrix are equivalent.

But recall how we derive the targets matrix - by taking the BOW encoding and dotting it with itself. 
The scores matrix is computed from the outer product of $f(D)$ and $g(D)$ where $D$ is a minibatch of sentences. 

So what we're essentially doing is trying to find a dense representation of the BOW matrix. 

## Using TF-IDF Matrices


[TF-IDF](https://en.wikipedia.org/wiki/Tf%E2%80%93idf) stands for Term-Frequency Inverse-Document-Frequency. It's basically a way to account for how "important" a word is. 

It might help to consider the following example to understand why TF-IDF weights help. 

Let's assume we have a corpus as follows:

1. The tree is green.
2. The dogs barked.
3. Dogs like bones.
4. The man with the hat ate the beans.


Obviously sentences 2 & 3 are more similar to each other than to 1, but when we compute our scores matrix we find that $scores_{1,2} = scores_{2,3}$, which implies that they are equally similar. 

This is because sentences 1 and 2 share the word `the`, which is a very common word. So we'd like to account for the relative frequency of words - this is what TF-IDF does. 

We can do this very easily by switching out `CountVectorizer` with `TfIdfVectorizer`. 

```python
tokenizer = BertTokenizer.from_pretrained('bert-base-uncased')
vectorizer = TfIdfVectorizer(tokenizer=tokenizer.tokenize, vocabulary=tokenizer.vocab)

def generate_cooccurence_counts(data, vectorizer):
    bow = vectorizer.fit_transform(data)
    targets = bow.dot(bow.T)
    return targets
```

Let's compare the targets matrix we get from `TfIdfVectorizer` and `CountVectorizer`:

$$
BOW = 
\begin{bmatrix}
    4 & 1 & 0 & 3 \\
    1 & 3 & 1 & 3 \\
    0 & 1 & 3 & 0 \\
    3 & 3 & 0 & 14 \\
\end{bmatrix}
, 
\text{TF-IDF} =
\begin{bmatrix}
1 & 0.155 & 0 & 0.225 \\
0.155 & 1 & 0.270 & 0.291 \\
0 & 0.270 & 1 & 0 \\
0.225 & 0.291 & 0 & 1 \\
\end{bmatrix}
$$

As you can see, $ TF-IDF_{2, 3}  >  TF-IDF_{1, 2} $, which means that these two sentences are considered more similar. 

To compute the loss, I again normalize the given targets matrix and take a row-wise KL divergence. 

$$
loss = KL 
\left(

\sigma \left(
\begin{bmatrix}
0 & 0.155 & 0 & 0.225 \\
0.155 & 0 & 0.270 & 0.291 \\
0 & 0.270 & 0 & 0 \\
0.225 & 0.291 & 0 & 0 \\
\end{bmatrix}
\right)
,
\begin{bmatrix}
f(s_1)^Tg_(s_1)  & \ldots & f(s_1)^Tg(s_n)\\
\vdots  & \ddots & \\
f(s_n)^Tg_(s_1)  & & f(s_n)^Tg(s_n) \\
\end{bmatrix}
\right)
$$

*What's the core idea of this approach?*

**Not all words are created equal. Sentences that share rare words are more likely to be similar to each other than sentences that share common words, and thus should be closer in the embedding space.**

Again this approach doesn't really work out of the box, largely because of the smoothness of the targets distribution. 

I also used softmax temperature scaling to make the distribution more peaky. 

### Binarized TFIDF Matrices 

So why is smoothness in the targets distribution bad?

Let's consider a case when the targets distribution is perfectly smooth - imagine a uniform distribution. 

Then there's a trivial solution to our embedding problem. If our mapping maps every sentence to the same vector, then we'll get the same value at every entry in our unnormalized scores matrix, which corresponds to the uniform distribution. 

But obviously mapping every sentence to the same vector is a bad sentence embedding. 

I tried using softmax temperature scaling to deal with this, but I didn't like the fact that this essentially introduces another hyperparameter to tune. 

Also having this sort of "smooth" targets distribution kind of goes against the idea of contrastive learning. 

Remember in contrastive learning we have similar pairs of points $x, x^+$ and dissimilar pairs of points $x, x^-$. With a smooth distribution this notion becomes kind of muddled. Instead we have a sort of soft similarity score. 

To counteract this, I stopped minimizing the KL-divergence between the scaled TF-IDF matrix and scores matrix, and instead used the TF-IDF matrix as a heuristic to mine samples. 

```python
tfidf_mat= torch.Tensor(generate_tfidf_counts(raw_data, vectorizer)).cuda()
max_indicies = torch.argmax(tfidf_mat, dim=0)
targets = F.one_hot(indicies, num_classes=args.batch_size).float()
```

This creates a one-hot vector where the index is 1 where the max TF-IDF score is, and 0 everywhere else.

For the example described before, that matrix would look something like this:

$$
\text{TF-IDF}_{bin} =
\begin{bmatrix}
0 & 0 & 0 & 1 \\
0 & 0 & 1 & 0 \\
0 & 1 & 0 & 0 \\
1 & 0 & 0 & 0 \\
\end{bmatrix}
$$

Basically I'm making the distribution as peaky as possible manually. This would be the same thing as if we did softmax temperature scaling with $Temp = \infty$

To me this is more inline with the original Quickthoughts/contastive learning approach, but now instead of mining similar samples from their relative locations in the corpus, I'm using TF-IDF similarity scores to mine similar samples. 

*What's the core idea of this approach?*

**Smoothness leads to sameness. Instead of scaling the targets distribution to be sharper, we can create a sharp distribution manually**.


## Results

To evaluate these different approaches I trained a copule of models, all with GRU encoders. I used a batch size of 1000, a learning rate of 0.0001 and a embedding size/hidden size of 300/1000 respectively. 
The only thing I changed was the different loss functions I described above. 

I used Facebook's [senteval](https://github.com/facebookresearch/SentEval) package in order to evaluate the different approaches. 


|                          | MR    | SUBJ  | CR    | MPQA  | STS12 | STS13 | STS14 | STS15 | STS16 | STSb  | SICKr |
|--------------------------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|
| Next Sentence Prediction | ***68.18*** | ***88.45*** | 74.76 | ***79.95*** | 49.49 | 35.85 | 49.80 | 55.05 | 55.22 | ***72.11*** | ***81.32*** |
| Cooccurence Matrix       | 64.02 | 84.42 | 70.44 | 76.16 | 26.89 | 8.63  | 17.28 | 28.64 | 19.97 | 64.61 | 75.48 |
| TF-IDF Matrix            | 66.93 | 85.89 | 72.56 | 75.52 | 48.85 | 41.50 | 50.57 | 59.95 | 51.09 | 68.53 | 78.77 |
| Binarized TF-IDF Matrix  | 67.88 | 86.85 | ***75.76*** | 78.91 | ***53.95*** | ***49.40*** | ***59.37*** | ***67.16*** | ***60.62*** | 65.08 | 80.49 |


MR, SUBJ, CR, and MPQA are binary classification tasks, while STS12-16 are unsupervised semantic textual similarity tasks. 
STSb and SICKr are supervised textual similarity tasks.

For all semantic textual similarity tasks I report the pearson coefficient, while for binary classification tasks I report accuracy. 

As you can see the binarized TF-IDF does very well on STS tasks - this is to be expected because BOW models have done well on these tasks. At the same time, this approach provides comparable performance on binary classification tasks. 

The accuracies for the binary classification tasks are actually quite low (about ~10%) compared to the results described in the paper. I'm not too sure why this happened, considering I used this exact same code to achieve comparable results seen [here](/Quickthoughts). 

## Conclusion


Trying different encoders could be interesting. I've tried a lot to try and use contrastive loss with Transformer models with limited success. I feel like there's a missing link here but I"m not to sure what it is. 

I wonder if taking the KL-div of two softmaxed vectors has a mathematical interpretation that I'm missing. 
There may be a trick here to calculate this more efficiently, but I'm not sure. I think the normalization constants make this hard to calculate, but perhaps there's a clever way to do this, 


Also I want to see if you can use this approach for "relational" KD like that described in the paper [here](http://openaccess.thecvf.com/content_CVPR_2019/papers/Park_Relational_Knowledge_Distillation_CVPR_2019_paper.pdf). The idea is that you can replace the BOW-TFIDF encoder with a large BERT encoder and use this approach to "distill" the model. 


You would essentially need an ordered corpus here - basically some sort of transfer corpus, and then do the minibatch trick used in Quickthoughts. But if you have a good teacher embedding, like Sentence-BERT you might be able to order the corpus. 

Ordering the corpus can be thought of a *metric* travelling salesman problem. This is NPC, but it may be possible to use some heuristics to "approximately" order the corpus. 





