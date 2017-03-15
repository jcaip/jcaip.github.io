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

## Quicksort


### Analysis
Let $$E[T(A)]$$ be the number of comparisons. We want to bound $$E[T(A)]$$, or the expected number of comparisons that Quicksort will call. We also define $$W(n)$$ to be the max expected value of any $$A$$ such that the size of $$A$$ is $$n$$.

From the recurrence relation we see that $$T(A) = T(A^+) + T(A^-) + n-1$$

Taking the expected value of each side yields 

$$E[T(A)] = E[T(A^+) + T(A^-) + n-1]$$

We can use linearity of expectations here to break this equation down to 

$$E[T(A)] = E[T(A^+)] + E[T(A^-)] + n-1$$

Assume that all elements are distinct, then $$Pr[\left\vert{A^-}\right\vert = i] = \frac{1}{n}$$
This is because the pivot must be the $$i+1^{th}$$ element, which has probability $$\frac{1}{n}$$


Now we can express the expected value of $$A^-$$ as

$$E[T(A^-)] = \sum_{i=0}^{n-1}{Pr[\left\vert{A^-}\right\vert = i] \times W(i)}$$

$$E[T(A^-)] = \sum_{i=0}^{n-1}{\frac{W(i)}{n}}$$

Similarly, $$Pr[\left\vert{A^+}\right\vert = i] = \frac{1}{n}$$

$$E[T(A^+)] = \sum_{i=0}^{n-1}{Pr[\left\vert{A^+}\right\vert = i] \times W(i) = \sum_{i=0}^{n-1}{\frac{W(i)}{n}}$$

We can plug this into our previous equation to get

$$E[T(A)] = \sum_{i=0}^{n-1}{\frac{W(i)}{n}} +  \sum_{i=0}^{n-1}{\frac{W(i)}{n}} + n-1$$

Remember that $$W(n) = \max{E[T(A)]}$$

$$W(n) \leq \sum_{i=0}^{n-1}{\frac{W(i)}{n}} +  \sum_{i=0}^{n-1}{\frac{W(i)}{n}} + n-1 \leq \sum_{i=0}^{n-1}{W(i)} \times \frac{2}{n}  + n-1$$

Now we will prove that $$W(n) \leq 2n \log(n)$$ by induction

Base Case: $$W(2) = 1 \leq 2 \log(2)$$

Inductive Step:
$$W(n) \leq \sum_{i=0}^{n-1}{W(i)} \times \frac{2}{n}  + n-1$$

Plugging in the induction hypothesis yields 

$$W(n) \leq \sum_{i=0}^{n-1}{2i\log(i)} \times \frac{2}{n}  + n-1$$

$$W(n) \leq \sum_{i=0}^{n-1}{i\log(i)} \times \frac{4}{n}  + n-1$$

Note that
$$\sum_{i=0}^{n-1}{i\log(i)} \leq \int_1^n{i \log(i)}$$

Thus 
$$\sum_{i=0}^{n-1}{i\log(i)} \leq \frac{n^2 log(n)}{2} + \frac{n^2}{4}$$

$$W(n) \leq (\frac{n^2 log(x)}{2} + \frac{n^2}{4}) \times \frac{4}{n}  + n-1$$

$$W(n) \leq 2n \log(n)$$

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
