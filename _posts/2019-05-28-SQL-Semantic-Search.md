---
published: False
---
One of the many applications of sentence embeddings is semantic search. 

Given a document $d$, find the $k$ most "related" documents to $d$. 

We can implement this as kNN search, there's also third-party libraries that do exactly this (see SPTAG from Microsoft). 

But I think these approaches are a bit overkill for what I imagine is the most common usecase:

We have somewhere around 1M-10M documents, unlabeled, just free form responses, and we want to look for trends in these documents, each document is about 10 sentences. 

The first thing we need to do is to generate a vector representation of our documents. 
We could train sentence representations, but to make things a bit simpler, we'll use a pretrained embedding that you can find [here]:

With this in mind, we know just need to find the $k$ such that $w_k^Td$ is maximized. 

The problem is that if we're searching over 10M documents, that won't all fit in memory, so we can't just keep a big matrix in memory and do a big matrix multiplication. Assuming our embedding dimension is 300, that means we would have a (100M, 300) matrix of doubles. Each double would be 8 bytes, for a total of 24 Gigabytes, a bit too large to fit comfortably into memory. 

You can store vector embeddings of comments in a table inside a database. In fact, we can do this quickly all inside the database easily by using a bit of SQL. 

I've written a little bit of wrapper code that brings up a PSQL database, and then encodes all the comments and stores all this information inside the database. 

In fact, we can do this quickly all inside the database easily by using a bit of SQL. 

Still it looks like such an approach works. 

Over the past couple of years, there's been huge progress in representation learning. 

In particular learned word vectors like Word2Vec and GloVe are now the industry standard in various NLP tasks. 


Representation learning can be seen as a form of dimensionality reduction. 

The main goal of representation learning is to find a vector $z \in \mathbb{R}^d$  that encodes all the variance of the real world data. 

One very common way to learn representations is to use an autoencoder

Then one can take the intermediate hidden layer as the representation. The model finds a lower dimensional representation that encodes all the variance of the higher dimensions.

The was the approach taken by Skip-Thoughts. 


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

We have two distributions here, the target label distribution, given by a one-hot vector, and our model output distribution, given by $P_\theta ( s_{ctx} \mid x, S_{cand})$.

The likelihood of our data $X$ is given by:

$$P(X) =  \prod_{x \in X} \prod_{s_{ctxt} \in S_{ctx}} P(s_{ctxt} \mid x, S_{cand})$$

We can optimize the log likelihood instead, to avoid numerical issues, getting a final cost funciton of the form:

$$J(X; \theta) = \log P(X) = \sum_{s \in X} \sum_{s_{ctxt} \in S_{ctx }} \log P_\theta(s_{ctxt} \mid x, S_{cand})$$

But then we can subsitute in (1) to get:

$$J(X; \theta) =  \sum_{s \in X} \sum_{s_{ctxt} \in S_{ctx }} \log  \frac{e^{\Phi(f(s), g(s_{cand}))}}{\sum_{s' \in S_{cand}} e^{ \Phi(f(s'), g(s_{cand})) }}$$

Using log properties we can rewrite this as.

$$J(X; \theta) =  \sum_{s \in X} \sum_{s_{ctxt} \in S_{ctx }} \left[ \log  e^{\Phi(f(s), g(s_{cand}))} - \log {\sum_{s' \in S_{cand}} e^{ \Phi(s'), g(s_{cand})) }} \right]$$

## QuickThoughts: Implementation details

Our loss is in fact taking a KL Divergence between target distribution and softmax of dot-product scores. 

## Taking a step back: CURL and other Theoretical Gaurentees
CURL provides a theoretical framework of some of the bounds here. 


#### Block Sampling

this is just adjusting the targets matrix


## Conclusion

### more powerful encoders and embeddings

Attention based models, such as the Transformer have taken off, in large part due to their ability to handle longer term relationships better than recurrent models. 

We sought to implement a more efficient implementation of QuickThoguths by replacing the GRU encoder with a Transformer/Attention based model. 

Furthermore, we also tried using pretrained contextual word embeddings a la Elmo in order to get a better feel for our results.

