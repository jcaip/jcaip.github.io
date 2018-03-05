---
layout: post
tags: [machine-learning, theory]
published: True

images:
  - url: https://infinitescript.com/wordpress/wp-content/uploads/2016/09/AdaBoost.jpg
  - alt: adaboox
  - title adaboost
---

An explanation of how some popular ensemble methods work for machine learning. 

Let's say we have some simple classifier. This classifier will either make bias or variance related errors. 

![bias_var](https://www.kdnuggets.com/wp-content/uploads/bias-and-variance.jpg)

Ensemble methods offer us an easy way to correct for these errors.

## Bias related errors

This is an underfitting problem

Boosting trains many weak classifiers to form a single strong classifer. 

Adaptive boost (Adaboost)
  - $$F_T(x) = \sum_{t=1}^{T}{f_t(x)}$$ where $$f_t(x)$$ is a weak classifier.
  - Adaboost learns the relative contribution of each weak learner according to how well it does on the test set
  - there is one set of weights that adaboost changes over time, by fitting more weak classifiers
  - upweighting observations that were misclassified before
  - special case of gradient boosting in terms of the loss function

Gradient Boosting
  Take a regression problem, feed simple classifier, and then learn the residual or the error on each step. 
  Residuals ($$y(x) - f(x)$$) are the gradient of the RMS loss, hence the gradient

  Instead of training on residuals, you can train on ciduals, of the loss for  classification problems. This corresponds to running gradient descent on a loss function. 

  We can run from a warm start
  - doesn't modify the sample distribution, weak learner trains on the residuals of the strong learner. 
  - the contribution of the weak learner is given by minimizing the overall error of the strong learner.

## Variance related errors
Variance related errors occur when we overfit our training data. In this case, our model is able to achieve good performance on the training set but fails to generalize when fed the test set. We can solve this problem by **bagging**.

Bagging, or bootstrap aggregrating resamples the data with replacement. Each of the models overfit in a different way, which when you take the average yields a good solution

Random Forests
  - randomly samples not only the examples but also the features. 
  - random forests are invariant to linear monotonic change
  - resilient to correlated features
  - resilient to randomness as well

Watchlist - you can overfitting at any round of the boosting process.
Easy cut-off at any round when classifying - to recover from overfitting
