---
layout: post
tags: [machine-learning, theory]
published: false

---

An explanation of how some popular ensemble methods work in ml. 

Let's say we have some simple classifier. This classifier will either make bias or variance related errors - talk a little bit about bias/variance tradeoff here. 

Ensemble methods offer us an easy way to correct for these errors

## Bias related errors

This is an underfitting problem

boosting trains many weak classifiers for a strong classifier

Adaptive boost (Adaboost)
  - sequential
  - train on the residual, increase weights on misclassified stuff
  - sum of a lot of weak classifierS
  -
  - $$F_T(x) = \sum_{t=1}^{T}{f_t(x)}$$ where $$f_t(x)$$ is a weak classifier.
  -
  - adaboost learns the relative contribution of each weak learner according to how well it does on the test set
  - there is one set of weights that adaboost changes over time, by fitting more weak classifiers
  -
  - upweighting observations that were misclassified before
  - special case of gradient boosting in terms of the loss function

Gradient Boosting
  Take a regression problem, feed simple classifier, and then learn the residual or the error on each step. 
  Residualts are the gradient of the RMS loss, hence the gradient 

  cidual on loss of classification problems
  correspods to running gradient descent on the loss function

  can run from a warm start
  - doesn't modify the sample distribution, weak learner trains on the residuals of the strong learner. 
  - the contribution of the weak learner is given by minimizing the overall error of the strong learner.

## Variance related errors
When we overfit the data

bagging, or bootstrap aggregrating resamples the data with replacement

each of the models overfit in a different way, which when you take the average yields really good results

Random Forests
  - also samples the features as well
  -


rf are invariant to linear monotonic change
resilient to correlated features
resilient to randomness as well

watchlists, learning curves
