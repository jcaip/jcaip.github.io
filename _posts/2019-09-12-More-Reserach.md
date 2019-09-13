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
|Next sentence prediction      | 64.52 | 72.34 | 81.63 | 85.89 |
|Co-occurrence counts (Temp=10)| 62.41 | 70.15 | 74.86 | 81.51 | 

### SentEval STS Spearman

|                              | STS12 | STS13 | STS14 | STS15 | STS16 | SICKr | STSb | 
|------------------------------|-------|-------|-------|-------|-------|-------|------|
|Next sentence prediction      | 0.369 | 0.347 | 0.352 | 0.384 | 0.382 | 0.755 | 0.657|
|Co-occurrence counts (Temp=10)| 0.394 | 0.301 | 0.437 | 0.524 | 0.508 | 0.728 | 0.607| 


