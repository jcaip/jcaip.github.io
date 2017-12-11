---
layout: post
title: Artificial Intelligence 

tags: [artificial-intelligence]
---

## Determining Intelligence

#### Turing Test
The Turing Test is a test devised by Alan Turning. The test calls for a computer and a human to be put in two seprate rooms, and another human to converse with both the human and the computer. If the human observer thinks he/she is talking to another human, the computer is said to have passed the Turing test. 

#### Winograd Schema
Winograd Schema are another series of problems that are used to test for understanding.
The idea is to have a sentence, with two possible meanings and to let the computer pick two choices that make sense. 

E.g. The city councilmen refused the demonstrators a permit because they [feared/advocated] violence. Who [feared/advocated] violence?


## Machine Learning
An agent is **learning** if it improves its performance on future tasks after making observations about the world.

+ What component should be improved?
+ What prior knowledge do we have?
+ What representation is used for the data and component?
+ What feedback is available to learn from?

Data will be provided in a **factored representation** - a vector of attribute values and outputs that can either be numerical or discrete.

**Feedback** can be classified into unsupervised, reinforcement, and supervised learning. There are also approachs that blur the line, for example semi-supervised learning or inverse reinforcement learning.

### Supervised Learning
In supervised learning, giving a **training set**, we try to find a **hypothesis** function $$h$$ that will give a proper matching. We evaluate $$h$$ on our **test set** of distinc examples.

When we want discrete values, this is **classification**, wheras when we want continuous values this is **regression**.

In general, we select $$h$$ from some **hypothesis space**. **Ockham's razor** suggests that we should pick the simplest hypothesis consistent with the data. In general, there is a tradeoff between complex hypotheses that fit the training data well and simpler hypotheses that may generalize better.

We say that a learning problem is **realizable** if the hypothesis space contains the true function. There is a tradeoff between the expressiveness of a hypothesis space and the complexity of finding a good hypothesis within that space.

### Learning Decision Trees
A decision tree represents a function that takes in a vector of attribute values and returns a single output value. The aim is to learn a definition for the **goal predicate**. A Boolean eciscion tree is good for some but not all applications. For example, a simple majority funciton is very hard to map. 

```python
DecisionTreeLearn(examples, attributes, parent_examples)
    if examples is empty then return Plurality-Value(parent_examples)
    else if all examples have the same classification then return classification
    else
        A = argmax(Importance(a, examples))
        tree = new decision tree with root test A
        for each value v of A
            exs = {e such that s.A = v}
            subreee = DecisionTreeLearn(exs, attributes -A, examples)
            add a branch to tree with label (A=v) and subtree subtree

        return tree
```

To pick the corrrect attribute, we introduce the notion of **entropy**. 

$$H(V) =  - \sum_kP(k) lg(P_k)$$ 

The information gain is the expected reduction in entropy.

Decision trees may overfit the data, so we may have to prune the tree. We can determine irrelevant nodes via a significance test. We can also use **early-stopping** to find the optimal stopping point.
