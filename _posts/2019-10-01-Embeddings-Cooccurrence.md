---
published: True
layout: post
tags: [machine-learning, nlp, research]
title: Learning Embeddings from Cooccurence Matrices
---

I wanted to share a different way to learn sentence representations. 

<!--more-->

To learn sentence representations, we learn a function to convert a sentence to a vector. 

This is useful because then you can use the vectors to do semantic search and stuff. 

One way to learn this is to try and predict the next sentence.

$$\prod_{s_i \in D}  P_\theta(s_{i+1} \mid s_i, D)$$

In Quickthoughts, they find semantically similar and dissimilar examples by looking at the location of the sentences in the minibatch. 


As a brief recap, this loss function is calculated by taking a row-wise KL between two matrices, the scores matrix and a targets matrix, 

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

$$
target = \begin{bmatrix} 
0  & 1 &  \dots & 0 \\
0  & \ddots & \ddots &  0 \\
\vdots  & & & 1  \\
0  &  &  &  0 \\
\end{bmatrix} \in \mathbb{R}^{n \times n}
$$

The scores matrix gets normalized to become a probability distribution.

The general thought behind this can be summed up as follows: Sentences next to each other tend to be related to each other. 

But this next sentence prediction that is used is really just a heurestic, so I thought I would take a look at other heurestics and see if I could find one that would work better. 

I started by trying to use the document-document word cooccurence matrix, the intuition being that sentences that share similar words are more likely to be similar to each other in meaning.


## Document-Document cooccurence matrix

This is a matrix $V \in \mathbb{Z}^{b \times b}$ where $b$ is the batch size and $V_{i,j} = $ the number words shared between $s_i$ and $s_j$.

There's a pretty quick and easy way to get the targets matrix in this case, by taking a binarized BOW encoding and dotting it with itself. 

We can get the targets matrix by taking the BOW encoding of every sentence. 

This maps each sentence to a vector $s \in \mathbb{R}^{\lvert V \rvert}$ where $V$ is the vocabulary. This is a binary vector where $v_j = 1$  is in the sentence, and 0 otherwise. 

Doing this for each sentence in the minibatch yields a $M \in \mathbb{R}^{b \times \lvert V \rvert}$ where $b$ is the batch size. 

It's probably helpful to see an example. Consider the sentences

1. I like dogs.
2. The dogs barked.
3. Dogs like bones.



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

and taking the subsequent transpose and inner product we can get our targets matrix.

$$
targets = BOW(BOW^T) = \begin{bmatrix}
    3 & 1 & 2 \\
    1 & 3 & 1 \\
    2 & 1 & 3 \\
\end{bmatrix}
$$

```python
tokenizer = BertTokenizer.from_pretrained('bert-base-uncased')
vectorizer = CountVectorizer(tokenizer=tokenizer.tokenize, vocabulary=tokenizer.vocab)

def generate_cooccurence_counts(data, vectorizer):
    bow = vectorizer.transform(data)
    targets = bow.dot(bow.T)
    return targets
```

This also kind of gives us some intuition as to why this should give us a good document embeddings. 

We're kind of finding a dense representation of the BOW vector of the term, but not quite. If we were really trying to find a dense representation we could take the L2 loss instead of the row-wise softmax. 

This approach is similar to GloVe,  but with a couple of key difference (as far as I can tell)

1. GloVe works on the word-word "document" cooccurence matrix, this is really a window cooccurence matrix 
1. Instead of directly minimizing the L2 loss between the targets and embeddings matrix, as GloVe seeks to do, I instead minimize the KL divergence between the two normalized probability distributions. 
That is, with GloVE I'm trying to find a dense representation explicitly that captures the cooccurence information, but here I'm using it to mine samples for contrastive learning. 

Obviously the whole KL-Div of the two softmax distributions is not the same as the L2 loss, but you get the general idea. 

Just using this for the targets matrix actually doesn't leave to great performance. 

I think it's because the distribution becomes quite smooth, which I think makes the model kind of just output the same vector for all kinds of different things.

To do this I did some softmax temperature scaling so make the distribution more peaky.


## Using TF-IDF Matrices

Of course this can be extended to TF-IDF weighted similarity scores, by switching out `CountVectorizer` with `TfIdfVectorizer`. 

```python
tokenizer = BertTokenizer.from_pretrained('bert-base-uncased')
vectorizer = TfIdfVectorizer(tokenizer=tokenizer.tokenize, vocabulary=tokenizer.vocab)

def generate_cooccurence_counts(data, vectorizer):
    bow = vectorizer.fit_transform(data)
    targets = bow.dot(bow.T)
    return targets
```

What this essentially does is take into acount the term frequency over the inverse document frequency of each term. 

One problem that I was running into here again when training was the smoothness of the target distribution.

To counteract this, I stopped minimizing the KL-divergence between the scaled TF-IDF matrix and scores matrix, and instead used the TF-IDF matrix as a heurestic to mine samples. 

```python
tfidf_mat= torch.Tensor(generate_tfidf_counts(raw_data, vectorizer)).cuda()
max_indicies = torch.argmax(tfidf_mat, dim=0)
targets = F.one_hot(indicies, num_classes=args.batch_size).float()
```

This creates a one-hot vector where the index is 1 where the max TF-IDF score is. 



To evaluate performance I trained a couple of QT models, all with a bidirectional GRU encoder and tried to see the results.

### SentEval STS Spearman

|                              | STS12 | STS13 | STS14 | STS15 | STS16 | SICKr | STSb | 
|------------------------------|-------|-------|-------|-------|-------|-------|------|
|Next sentence prediction      | 0.374 | 0.352 | 0.359 | 0.393 | 0.389 | 0.751 | 0.649|
|Co-occurrence counts (Temp=10)| 0.394 | 0.301 | 0.437 | 0.524 | 0.516 | 0.728 | 0.602| 
|TFIDF matrix (Temp=10)        | 0.395 | 0.307 | 0.396 | 0.461 | 0.405 | 0.730 | 0.674| 
|binarized TFIDF               | 0.464 | 0.451 | 0.496 | 0.547 | 0.550 | N/A   | N/A  | 

### SentEval Binary Classification Accuracy  

|                              | MR    | CR    | MPQA  | SUBJ  | 
|------------------------------|-------|-------|-------|-------|
|Next sentence prediction      | 65.74 | 72.34 | 83.11 | 85.23 |
|Co-occurrence counts (Temp=10)| 62.41 | 70.15 | 74.86 | 81.85 | 
|TFIDF matrix (Temp=10)        | 63.16 | 71.47 | 77.62 | 82.73 | 
|binarized TFIDF matrix        | 62.67 | 71.84 | 76.84 | 81.66 | 

The binarized TFIDF performs much better on the STS evaluation. 


I've talked about this quite a bit before but I was trying to use this to essentially fine-tune BERT and try and get a better document representation. 
This didn't work at all, probably something to revisit. 
