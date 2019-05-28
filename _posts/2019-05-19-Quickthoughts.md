---
published: False
layout: post
tags: [machine-learning, nlp]
title: "Learning Better Sentence Representations"
---

Over the past couple years, representation learning has really taken off in NLP. 

Doc2Vec, Word2Vec, GloVe, etc. 

Talk a bit about learning good sentence representations. 

## What is Contrastive Learning?

Representation learning can be thought of a form of density estimation or dimensionality reduction. 

The main goal of representation learning is to find a vector $z \in \mathbb{R}^d$ such that for two related sentences, $s_1$ and $s_2$, 

One very common way to learn representations is to use an autoencoder, in a normal autoregressive manner.

Then one can take the intermediate hidden layer as the representation. 

The model is inclined to be able to reproduce the candidate sentence. 

The was the approach taken by Skip-Thoughts. 

Contrastive learning is the idea of considering similar and disimilar example points

## QuickThoughts

The main idea of quickthoughts is actually quite simple: Similar sentences are next to each other. 

Instead of training a complicated decoder to recover the original sentence, we instead train a discriminator to select the encoding amongst a bunch of other samples. 

What this means is that instead of parameterizing an encoder and a decoder, $P_\theta(z \mid x )$ and $Q_\phi(x \mid z)$ and minimizing the reconstruction loss

Within each batch, we attempt to correctly predict the next sentence given the encodings of all the sentences. 

More concretely, we model $P_\theta(z \mid x)$ and a discriminatior,  which models $ P(s_{ctx} \mid s, S_{cand})$, where $s$ is $S_{cand}$ is the candidate window size (the sentences that we choose from) and $s$ is the dataset. 

More concretely, we can define this probability as a softmax distribution, with some scoring function $\Phi$.

$$ P(s_{cand} \mid s, S_{cand}) = \frac{e^{\Phi(f(s), g(s_{cand}))}}{\sum_{s' \in S_{cand}} e^{ \Phi(f(s'), g(s_{cand})) }}$$

We can then define the scoring function to be the dot product of the two encoders, so as to avoid training a trivial encoder and a powerful discriminator, as we discard the discriminator at the end of training.

When we consider sentences as atomic words, this is the same loss function used in Mikolov et al's seminal Word2Vec paper. 

### Deriving our Loss Function

We can think of this cost function a different way, by considering the loss function as the KL between our data distribution and our model output.

More concretely, we want to take the KL of

P(x) and Q(x)

$$ P_{model}(s_{cand} \mid s, S_{cand})  = $$

Recall that the KL divergence between two probability distributions is given by 

$$D_{KL}(P \mid\mid Q) = \sum_{x \in X}  P(x) \log \left( \frac{P(x)}{Q(x)}\right)$$

We can rewrite this as an expectation:

$$D_{KL}(P \mid\mid Q) = \sum_{x \in X}  P(x) \log \left( \frac{P(x)}{Q(x)}\right) = \mathbb{E}_{x \in X} \left [\log \frac{P(x)}{Q(x)} \right ]$$


We have two distributions here, the target label distribution, given by a one-hot vector, and our model output distribution, given by $P_\theta ( s_{ctx} \mid x, S_{cand})$.

The likelihood of our data $X$ is given by:

$$P(X) =  \prod_{x \in X} \prod_{s_{ctxt} \in S_{ctx}} P(s_{ctxt} \mid x, S_{cand})$$

We can optimize the log likelihood instead, to avoid numerical issues, getting a final cost funciton of the form:

$$J(X; \theta) = \log P(X) = \sum_{s \in X} \sum_{s_{ctxt} \in S_{ctx }} \log P_\theta(s_{ctxt} \mid x, S_{cand})$$

But then we can subsitute in (1) to get:

$$J(X; \theta) =  \sum_{s \in X} \sum_{s_{ctxt} \in S_{ctx }} \log  \frac{e^{\Phi(f(s), g(s_{cand}))}}{\sum_{s' \in S_{cand}} e^{ \Phi(f(s'), g(s_{cand})) }}$$

Using log properties we can rewrite this as.

$$J(X; \theta) =  \sum_{s \in X} \sum_{s_{ctxt} \in S_{ctx }} \left[ \log  e^{\Phi(f(s), g(s_{cand}))} - \log {\sum_{s' \in S_{cand}} e^{ \Phi(s'), g(s_{cand})) }} \right]$$


Our loss is in fact taking a KL Divergence between target distribution and softmax of dot-product scores. 

### QuickThoughts: Implementation details

CURL provides a theoretical framework of some of the bounds here. 

CURL also tells us about negative samples. 

## Building a better sentence representation 

One thing that stood out to me was the theoretical analysis done by Aurora et al about what they call contrastive unsupervised representation learning. 

### Block sampling method

Going back to the roots with regards to word vectors, one similar approach that was shown to be functionally similar was to simply generate a giant cooccurence matrix, and then factor that matrix to get good representations.

So let's go back to our target distribution. Here instead of simply denoting the next sentence, 

Our intution: similar sentences share have similar words. So we'll use the word cooccurence counts as a measure of the relatedness of two sentences.

To do this we tokenized the bookcorpus dataset into paragraphs, and considered every sentence within a paragraph to be related, and all other paragraphs to be unrelated. 

This essentially gives us a variable length block size. 

### Dealing with poor negative sampling

In the case of word vectors, we can just pick two random words and use this as a negative sample. 

For QuickThoughts, for each training example, we have on average 398 negative examples and 2 positive examples.

But the problem with our negative sampling is that we hold the batch size of to be the set of all negative samples, aside from the 2 sentences that are adjacent. 

But related sentences show up all around our candidate sentence in actuality. They may show up one sentence later, or two, or four. 

But here in lies the problem with our sampling strategy. We're going to mark a bunch of positive samples as negative samples, and this in theory (according to the analysis done in CURL) should hamper the effectiveness of our learned representation. 

There were a couple of ways that I tried to get around this. 

I considered some softmax smoothing of the target distribution, or to simply draw the batch from 10 different points inside the corpus. 

But really, every problem I faced was tied to the assumptions that we made regarding our target distribution, so we had to change that. 

## Using more powerful encoders and embeddings

GRUs and LSTMs are dead, pay attention to the future. Attention based models, such as the Transformer have taken off, in large part due to their abbility to handle longer term relationships better than recurrent models. 

We sought to implement a more efficient (though this is hardly necessary) implementation of QuickThoguths by replacing the GRU encoder with a Transformer/Attention based model. 

Furthermore, we also tried using pretrained contextual word embeddings a la Elmo in order to get a better feel for our results.
