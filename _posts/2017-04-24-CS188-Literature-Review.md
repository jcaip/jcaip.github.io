---
layout: post
title: CS188 Literature Review
tags: [machine-learning, nlp]

images:
    - url: /images/cs188/rcnn_structure.png
      alt: structure
      title: structure
---

## Goal of the project
	
In the last few years, machine learning models have become increasingly powerful, and accessible. Although practically science fiction a decade ago, the frontier of computer vision is capable of labeling arbitrary images with resounding accuracy. Despite this prodigious progress, the medical field has been slow to embrace its potential for diagnosis of medical images. In our project, we hope to analyze MRI scans and their accompanying doctors notes to train a machine learning model to associate images with notes. Using a medical dictionary, our model will associate image features with specific ailments. This model would be able to accurately generate notes and diagnoses through the images. 
<!--more-->

A paper from 2015 seeks use a deep learning system to mine the semantic interactions of radiology images and their respective reports. To ease the necessary data analysis, they focus on representative ~216K 2D images with a doctor’s observations.  They use unsupervised document topic-modeling [algorithm (LDA)](https://en.wikipedia.org/wiki/Latent_Dirichlet_allocation) to analyze the doctor reports for the potential topics to eventually categorize the image. A certified radiologist reviews the keywords and divides the topics into different levels based on how broadly they apply. Then, a CNN is trained to categorize the image into a topic for each of the different levels.

In general, this approach seems to work well, although the reports and captions used are much smaller than the data we currently have. We want to use [this](http://proceedings.mlr.press/v32/le14.pdf) approach create paragraph vectors, which would try to capture the tone/information of the report in a shorter, more succinct representation. Furthermore, we may be able to use the Topical Phrase Model described in Yulan He’s [paper](https://arxiv.org/pdf/1603.08486.pd). If we were able to shorten our reports, we may be able to use Tensorflow’s off the shelf Show and Tell model to be able to generate our condensed captions from images.

We also want to try to use the [recurrent neural cascade model](https://arxiv.org/pdf/1603.08486.pdf), which uses the weights of the already trained pair of CNN/RNN on the domain-specific image/text dataset, to infer the joint image/text contexts for composite image labeling and led to a significant performance increase

## Relevant Papers
+ [Paragraph Vectors](http://proceedings.mlr.press/v32/le14.pdf)
    + Could be useful as part of a classification pipeline
+ [Interleaved Text/Image Deep Mining](www.cs.jhu.edu/~lelu/publication/cvpr15_0371.pdf)
+ [Extracting Topical Phrases](http://dl.acm.org/citation.cfm?id=3016316)
+ [Learning to read Chest X-Rays](https://arxiv.org/pdf/1603.08486.pdf)
    + Code found [here](https://github.com/khcs/learning-to-read) 

## Perplexity
+ probability of the test set normalized by the number of words in Shannon's game
+ Extrensic evaluation of N-gram models
+ bad approximation unless test data looks alot like training data
+ generally useful in pilot experiments

**The Shannon Game**: How well can we predict the next word?

A better model of text is one which assigns a higher probability to the word that actually occurs

The best language model is one that best predicts an unseen test set. 

Perplexity is related to the average branching factor
+ on average, what can occur.
+ Weighted equivalent branching factor
