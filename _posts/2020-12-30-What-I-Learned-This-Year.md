---
published: False
layout: post
---

1/13/2020
I just watched this lecture by Andrew W. Lo titled [Artificial Stupidity](https://www.youtube.com/watch?v=zqw1nmJ7XZM&t=3957s)

Basically the idea is that we need AI to understand human behavior, 

Economists assume human behavior is purely rationaly (expected utility maximization) but this isn't true. 
Some other guy proposed a different model of behavior - essential we build a heurestic and procedurally refine it (Simons)

New AI (deep learning) is more similar to this approach than old AI (expert systems). 

Lo wrote a paper trying to use machine learning to predict when/if a person will panic sell. 

Want's to use this to create personalized finance. 

I'm a little wall street shit at heart, so I was thinking -

Instead of predicting which stocks will go up, why not try to predict when people will panic. 

Because if you predict a panic successfully, you can buy the dip. 


1/23/2020
Equitable Valuation of Data:

Shapley Value
- nullness
- symmetry
- linearity

Better than leave one out because doesn't depend of relative similarity of examples. 

MC approximation - sample random permutation of training samples. 

Then train - at each gradient update you can get marginal improvement PERF(a, b, c) - perf(a, b) by subtracting previousl performance.

applications:
small mumber of samples - can be used to clean a dataset

data shapley can be computed efficiently for KNN. 

use dnn for feature then use knn

1/28/2020
SBM (n, p, W)
n - number of vertices
p - probability 

