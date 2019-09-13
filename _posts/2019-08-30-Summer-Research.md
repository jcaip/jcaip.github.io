---
published: True
layout: post
tags: [machine-learning, nlp, research]
title: BERT fine-tuning and Contrastive Learning
---

So this summer I **officially** started doing research with UCLA-NLP. 

Anyways I want to start blogging more in general (I only have written two posts so far this year!), so I figure I'd write about my research, which has mainly been on transfer/representation learning for NLP. 

<!--more-->


### A comparison of industry of sentence embedding techniques on industry data:

Taboola graciously provided several news datasets, which I used to compare several different embedding techniques. 

The first dataset the provided consisted of 

|        TOP LEVEL TAXONOMY       |Rand Index| Adjusted MI  | train acc |test acc |
|---------------------------------|----------|--------------| ----------|---------|
| Taboola D2V (600)               | 9.85%    | 17.22%       | 49.81%    |  48.13% |
| QT   (1000)                     | 10.37%   | 17.73%       | 56.15%    |  54.11% |
| FT-QT  (Taboola, 10000 lines)   | 15.32%   | 24.00%       | 57.73%    |  54.56% |
| FT-QT  (Taboola, 427419 lines)  | 22.15%   | 30.12%       | 62.42%    |  59.74% |
| FT-QT  (20news, 18846 lines)    | 10.56%   | 17.28%       | 53.49%    |  50.73% |
| BERT   (768) CLS layer          | 8.88%    | 15.70%       | 53.20%    |  51.55% |
| BERT   (768) mean pooling       | 8.31%    | 15.77%       | 54.87%    |  54.17% |
| BERT   (768) CLS fine-tuned     | 26.83%   | 32.99%       | 62.21%    |  53.88% |


| SECTION(14 categories) 30k lines  |Rand Index| Adjusted MI  | train acc | test acc |
|---------------------------------|----------|--------------| ----------| ---------|
| Taboola D2V (600)               | 6.99%    | 8.12%        | 53.37%    |  44.41% |
| QT   (1000)                     | 5.95%    | 9.24%        | 67.84%    |  52.80% |
| FT-QT  (SAME, 10000 lines)      | 12.52%   | 19.84%       | 72.00%    |  57.82% |
| BERT (cls, 768 )  fine          | 38.56%   | 42.55%       | 76.93%    |  68.92% |
| BERT (cls, 768 )                | 2.25%    | 6.01%        | 33.20%    |  30.88% |
| BERT  (pool, 768)               | 1.82%    | 5.04%        | 29.30%    |  26.08% |


# Comparing different fine-tuning approaches:

## Hierarchical fine-tuning

The idea here is come from some experiments that my research partner Di ran. 
On 20 news groups he noted a increase in training accuracy/test accuracy when training on the just top level labels.

But the one thing that peeved me about this is that the label encoding is that they share virtually no information with each other. 

That is the top level category `misc` is encoded to 4 in the first scheme but is encoded to 6 in the second scheme (these numbers are arbitariy, they're just here to show the problem).
So this doesn't really make sense so I thought it would be more natural to use a hierarchical encoding scheme. 

That it we should encode a category `computers.graphics` as the vector $[1, 1]$  and `computers.asdf.mac` as $[1, 2]$ instead so that there's some notion of similarity between the labels. 

This was mildly successful, but I saw no gain over flat softmax unfortunately. 

## Contrastive Fine-Tuning of BERT

The standard approach to fine tune BERT 

The central idea behind a contrastive loss is that given two samples, $x^+$, $x^-$, we'd like for $x^+$ to be close to x and for $x^-$ to be far away from x. 

The key idea of this approach is how to mine these semantically similar examples. 

In the case of BERT this is my fine tuning approach:

I encode the entire batch, and then specify that all samples that occur within in the same class are semantically similar, while all the examples that are from different classes are distinct. 

Of course, we also need to then calculate a targets matrix to compute a KL divergence and get a loss between these two matrices. 

To do that we can use a clever trick - If we one hot encode our labels, $y$, into $Y$, a one hot matrix, we can compute this targets distribution by taking the outer product of $Y$ and $Y$. 

Why does this work? Consider $T_{i, j}$. If these two samples are from the same class, then, the one hot labels dotted together (as a result of the matrix multiplication) will be 1, however, if these labels are different then the one hot labels dotted together will be 0. 

However, this contrastive fine tuning doesn't seem to improve downstream classification performance. Although I'm not 100% sure, I have a couple of ideas:

1. First off, there's an optimization problem here. Since both encodings come from the same encoder, when we backpropagate, the gradients for the model can be quite large, which leads to poor performance. 

2. I think in general it's best to be as explicit as possible, so using a softmax loss is probably the best choice here. 

2. Secondly, a contrastive loss here may not be the best idea vs. something  more explicit, like a softmax classifier loss. Another problem with the contrastive loss is that it forces 

The hope is to find a fine-tuning solution that works for only a bit of data, since this is usually when you can't use a supervised classification loss. 

| fine-tuning experiments (20 news)  | train_acc | test_acc| ARI | AMI|
|------------------------------------|-----------|---------|-----|----|
|BERT                                | 58.56     | 45.36   | 
|BERT + FT                           | 82.61     | 68.48   |
|BERT  +only same category           | 74.12     | 53.29   |

Ultimately, while this contrastive loss was helpful (it led to higher train and test accuracy), one could get better performance by taking the conventional fine tuning approach.

If you think about this, it makes a lot of  sense, since the conventioanl fine tuning approach is by far the most explicit, as well as most similar to the downstream task. 

# Bert-QT

So given that contrastive fine-tuning of raw BERT was not performing well, I wanted to try a BERT-QT model. 

That is, take BERT embeddings as the input into a model trained using contrastive loss. This is very similar as before, but instead of using this contrastive loss for fine-tuning, we're training BERT for a long period of time in order to get good sentence representations. 

There were three different approachs I considered. 



You'll note that average/CLS BERT embeddings on STS tasks seems to perform poorly. In fact they perform worse than average GloVe word vectors.

I need to check performance of this on MNLI and STS, which will be a good measure, as well as SentEval, which is already set up.

## ContraBert

I tried several different models for contrastive learning of BERT.

Firstly, I tried the **feature extraction** approach, by just taking the output of BERT and feeding it into a GRU-RNN in order to get out two different embeddings. 
I then trained the model by minimizing the KL between each row-wise softmax and the targets matrix from QT that represents the next sentence. 

This was mildly successful, but ultimately I saw no improvement over 1


Compare against [Sentence Siamese BERT](https://arxiv.org/pdf/1908.10084.pdf).

A couple interesting ideas here :more intelligent masking
Also try intelligent masking for this. Note that if this works, this is important, because this is an entirely unsupervised approach - no labels at all, we mine the related samples.


|                              | STS12 | STS13 | STS14 | STS15 | STS16 | 
|------------------------------|-------|-------|-------|-------|-------|
| fixed bert                   | 0.369 | 0.347 | 0.352 | 0.384 | 0.382 |
| bert-qt                      | 0.496 | 0.301 | 0.437 | 0.524 | 0.508 | 
| contrabert                   | 0.521 | 0.651 | 0.608 | 0.644 | 0.639 | 


