---
published: False
layout: post
tags: [machine-learning, nlp, research]
title: Cooccurrence Matrices
---

This is maybe a good idea.

<!--more-->

This approach is kind of similar to GloVe/LSA

### SentEval Binary Classification Accuracy 

|                              | MR    | CR    | MPQA  | SUBJ  | 
|------------------------------|-------|-------|-------|-------|
|Next sentence prediction      | 65.74 | 72.34 | 83.11 | 85.23 |
|Co-occurrence counts (Temp=10)| 62.41 | 70.15 | 74.86 | 81.85 | 
|TFIDF matrix (Temp=10)        | 63.16 | 71.47 | 77.62 | 82.73 | 
|BERT-NSP last 3 layers        | 68.66 | 73.09 | 86.47 | 86.63 | 


### SentEval STS Spearman

|                              | STS12 | STS13 | STS14 | STS15 | STS16 | SICKr | STSb | 
|------------------------------|-------|-------|-------|-------|-------|-------|------|
|Next sentence prediction      | 0.374 | 0.352 | 0.359 | 0.393 | 0.389 | 0.751 | 0.649|
|Co-occurrence counts (Temp=10)| 0.394 | 0.301 | 0.437 | 0.524 | 0.516 | 0.728 | 0.602| 
|TFIDF matrix (Temp=10)        | 0.395 | 0.307 | 0.396 | 0.461 | 0.405 | 0.730 | 0.674| 
|BERT-NSP last 3 layers        | 0.416 | 0.508 | 0.434 | 0.488 | 0.430 | 0.705 | 0.589| 


