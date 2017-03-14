---
layout: post
published: True

images:

    - url: /images/algs/dp/APShortestPath.png
      alt: Loss Function
      title: Loss Function
---
{% assign image = page.images[0] %}		
{% include image.html image=image %}


### Linearity of Expectations
Let \\(x\\) and \\(y\\) be two random variables. Linearity of Expectations states that

$$ E[x+y] = E[x] + E[y] $$

That is, the expected value of the sum is the sum of the expectations.

### Randomness in Algorithms
When you have many good choices, just pick one at random.
Randomness can be used to speed up algorithms and ensure good expected runtime.

### Quicksort


### Analysis


### Quickselect

### Analysis
Let \\(T(A, k)\\) be the total number of comparisons that the algorithm makes. We want to bound \\(E[T(A,k)]\\), which is the expected number of comparisons.
Let \\(W(n)\\) be the max possible \\(E[T(A,k)] \forall A, k \\).


$$ E[T(A,k)] \leq n + E[W(\left\vert{B}\right\vert)] $$
$$ \left\vert{B}\right\vert = \begin{cases}
    i-1 \text{ if } i > k \\
    n-i \text{ if } i < k \\
\end{cases}
$$



### Dictionaries and Hashing

### Analysis
