---
layout: post
tags: [probabilistic-models]
---
We handled uncertainty before via **belief states**. However, when interperting partial sensor information, belief states create a huge number of states. This leads to the **qualification** problem. 

In general, **laziness**, **theoretical ignorance** and **practical ignorance** keep us from deducing with pure logic. To deal with **degrees of belief** we will use **probability theory**. 
<!--more-->

We use a combination of probability theory and utility theory to form deciscions, seeking to maximize the **max expected utility**. 

### Probability
The set of all possible worlds is called the **sample space**. $$P(w)$$ specifies the probability of each possible worlds. 

We use probabilities to create **propositions**. For any proposition $$\phi, P(\phi) = \sum_{\omega \in \phi}P(\omega)$$

Probabilities such as P(same number) are **unconditional** or **prior** probabilities. Usually however, we have some **evidence** and want to find the **conditional** or **posterior** probability of rolling doubles given the first die is a 5.

$$P(A\land B) =  P(A | B) P(B)$$

Variables in probability theory are refered to as **random variables** with a set domain. We use a **probability density function** to express the belief of continuous variables. 

For distributions on multiple variables, we use a **joint probability distribution**.  A full joint probability distribution is a probability model that is completely dependent on all of the random variables. 

**inclusion-exclusion principle** - $$P(a \lor b) = P(a) + P(b) - P(a \land B)$$

**de Finetti** - If an agent expresses a set of beliefs that violate probability theory, there is a combination of bets that gaurentees that that agent will lose money every time.

Probability has different schools of thought - frequentist, objectivist, subjectivist, subjective Bayesian view.

The **marginal** probability can be found by summing out, or **marginalization**. $$P(Y) = \sum_{z \in Z} P(Y,z)$$. If we subsitute in conditional probabilities using the product rule, this is called conditioning.

We can avoid a costly division by simply writing $$ P(X \vert e) = \alpha P(X, e) $$

**marginal independence** occurs when $$P(X \vert Y) = P(X)$$. If the complete set of variables can be divided into independent subsets, then the full joint distribution can be factored into seprate joint distributions.

**Bayes rule** - $$P(b \vert a) = \frac{P(a \vert b) P(b)}{P(a)}$$ We can use this when we see evidence of the effect of some unknown cause. Diagnostic knowledge is often more fragile than causal knowledge. 

If we assume **conditional independence**, then we can rerite $$P(A \land B \vert C) =  P(A \vert C) P(B \vert C)$$. This grows in size $$O(n)$$ instead of $$O(n^2)$$. It also introduces the concept of **seperation**.

We can then write the full joint probability distribution as

$$P(Cause, Effect_1 \ldots Effect_n) = P(Cause) \prod_i{P(Effect_i | Cause)}$$

This is the **naive-Bayes** model, which can be used as a **Bayesian classifier**.

## Bayesian Networks
We can use a **Bayesian network** to represent dependencies among variables. Each node corresponds to a random variable, which may be discrete or continuous. Each node $$X_i$$ has the probability distribution $$P(X_i | Parents(X_i))$$

Each node has a **conditional probability table**, with each row containing a **conditioning case**, which is just a possible combination of parent variables. 

We see that for each node, $$P(x_1 \ldots x_n) = \prod_{i=1}^n P(x_i \vert parents(X_i))$$

When we rewrite $$P(x_1 \ldots x_n)$$ into conditional probabilities, we see a **chain rule**. This means that our bayesian network is the correc representation if each node is conditionally independent from its predecessors given the parents.

Bayesian networks are a form of **locally-structured**, or **sparse** systems, where each subcomponent only interacts with a limited number of other nodes. If we choose the node ordering well, we are able to maintain a compact Bayesian network. if we try to build diagnostic models with links from symptoms to causes, we often have to specify additional dependencies vetween otherwise independent causes. 

Each variable is conditionally independent of its non-descendants given its parents. A node is conditional independent of all other nodes in the network given its children, parents, and children's parents, or its **Markov blanket**.

### Inference in Bayesian Networks
The basic task of probabilistic inference is to compute the posterior probaiblity for a set of **query variables**. That is we want $$P(X|e)$$ where $$e$$ is some particular observed event, and $$X = Hidden \cup  Evidence \cup Query$$.

We can conduct inference by enumeration by computing sums of products of conditional probabilities from the network.
    $$P(b|j,m) = \alpha \sum_e \sum_a P(b)P(e)P(a|b, e)P(j|a)P(m|a)$$

However, this can be set up via **variable elimination**. This works by evaluating expressions in right-to-left order and storing intermediate results. We split eahc part of the expression into corresponding **factors**.

This process will create new factors that eventually lead to our solution.

$$f(X_1 \ldots X_j, Y_1 \ldots Y_k, Z_1 \ldots Z_l) = f_1(X_1 \ldots X_j, Y_1 \ldots Y_k) f_5(Z_1 \ldots Z_l)$$

We can sum out factors $$f(B,C) = \sum_a f_3(A,B,C) = f_3(a, B,C) + f_3(\neg a, B,C)$$. Any factor that does not depend on the variable to be summed out can be moved outside the summation.

We need to be able to establish an ordering for the variables. Intractable to find optimal factor, but one good heurestic is to eliminate whichever variable minimizes the size of the next factor to be considered. 

It's important to note that every variable that is not an ancestor of a query variable or evidence variable is irrelevant to the query.

