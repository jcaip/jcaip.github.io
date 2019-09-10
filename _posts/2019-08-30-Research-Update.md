---
published: False
---

Anyways I think I want to start blogging more just in general, so I want to start writing monthly research updates:

I've been doing research for the past 6 months, and it's been quite the mixed bag. 
I cannot lie, I'd like to say that I've made some meaningful progress, but I haven't gotten any interesting results yet. On the other hand, I have learned quite a bit and have tried some interesting ideas. 

Basically I've been working on BERT and different ways to fine-tune it/ use it for sentence representations. 

<!--more-->

Here's a short list of the different things I've tried:
- Contrastive fine-tuning
- using contextual word vectors as input to quick-thoughts
- Hierarchical fine-tuning (only seeing a subset of labels

## Contrastive Fine-Tuning of BERT

The central idea behind a contrastive loss is that given two samples, $x^+$, $x^-$, we'd like for $x^+$ to be close to x and for $x^-$ to be far away from x. 

The key idea of this approach is how to mine these semantically similar examples. 

In the case of BERT this is my fine tuning approach:

I encode the entire batch, and then specify that all samples that occur within in the same class are semantically similar, while all the examples that are from different classes are distinct. 

Of course, we also need to then calculate a targets matrix to compute a KL divergence and get a loss between these two matrices. 

To do that we can use a clever trick - If we one hot encode our labels, $y$, into $Y$, a one hot matrix, we can compute this targets distribution by taking the outer product of $Y$ and $Y$. 

Why does this work? Consider $T_{i, j}$. If these two samples are from the same class, then, the one hot labels dotted together (as a result of the matrix multiplication) will be 1, however, if these labels are different then the one hot labels dotted together will be 0. 

However, this contastive fine tuning doesn't seem to improve downstream classification performance. Although I'm not 100% sure, I have a couple of ideas:

1. First off, there's an optimization problem here. Since both encodings come from the same encoder, when we backpropagate, the gradients for the model can be quite large, which leads to poor performance. 

2. I think in general it's best to be as explicit as possible, so using a softmax loss is probably the best choice here. 

2. Secondly, a contrastive loss here may not be the best idea vs. something  more explicit, like a softmax classifier loss. Another problem with the contrastive loss is that it forces 

The hope is to find a fine-tuning solution that works for only a bit of data, since this is usually when you can't use a supervised classification loss. 

I also experienced a corner case where if I fine tune on a limited subset of data then I lose the pre-trained LM apparently: This is documented in the BERT paper, so it is what it is. 

In retrospect this may or may not be a problem, since it pretty much only happens when the training set is small, and that means that you can afford to do a bunch of runs. 

## Bert-QT

So given that contrastive fine-tuning of raw BERT was not performing well, I wanted to try a BERT-QT model. 

That is, take BERT embeddings as the input into a model trained using contrastive loss. This is very similar as before, but instead of using this contrastive loss for fine-tuning, we're training BERT for a long period of time in order to get good sentence representations. 

You'll note that average/CLS BERT embeddings on STS tasks seems to perform poorly. In fact they perform worse than average GloVe word vectors.

I need to check performance of this on MNLI and STS, which will be a good measure, as well as SentEval, which is already set up.

Compare against [Sentence Siamese BERT](https://arxiv.org/pdf/1908.10084.pdf).

Also try intelligent masking for this. Note that if this works, this is important, because this is an entirely unsupervised approach - no labels at all, we mine the related samples.

I also tried to get better performance by doing some masking of the scores matrix

## Hierarchical fine-tuning

The idea here is come from some experiments that my research partner Di ran. 
On 20 news groups he noted a increase in training accuracy/test accuracy when training on the just top level labels.

But the one thing that peeved me about this is that the label encoding is that they share virtually no information with each other. 

That is the top level category `misc` is encoded to 4 in the first scheme but is encoded to 6 in the second scheme (these numbers are arbitariy, they're just here to show the problem).
So this doesn't really make sense so I thought it would be more natural to use a hierarchical encoding scheme. 

That it we should encode a category `computers.graphics` as the vector $[1, 1]$  and `computers.asdf.mac` as $[1, 2]$ instead so that there's some notion of similarity between the labels. 

This was mildly successful, but I saw no gain over flat softmax unfortunately. 

# When and why does BERT fail?

BERT has exhibited some volatility when fine-tuning on small datasets... But why does this happen?

As an aside, this probably insn't too much of a usability problem, since if it's a small dataset you can affort to run lots of runs and just take a run that fits well. 

This might have to do with how the weights are initialized? Some covariate norm thing probaby.

We're claiming that BERT gives a good sentence representation.

Also there's a noted catastrophic forgetting problem that everyone mentions but nobody defines. I need to devise a test to show this problem - probably successive training two classifiers and noting a marked decrease in performance. 


