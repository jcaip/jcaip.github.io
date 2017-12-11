---
published: True

images:

    - url: /images/algs/divide/recursion_tree.gif
      alt: Divide and Conquer
      title: Divide and Conquer
---

There are usually three steps to a divide and conquer algorithm

+ Divide the problem into subproblems
+ Recursively solve each subproblem
+ Merge the results together

### Master Theorem
The master theorem provides a solution for recurrence relations from divide and conquer algorithms. You can read more about the master theorem [here](https://en.wikipedia.org/wiki/Master_theorem).

Let some recurrence $$T(n) = \alpha T(\frac{n}{b}) + f(n)$$
Then consider the Recursion Tree of \\(T(n)\\)

Let $$k = \log_b{n}$$ be the depth of the recursion tree 

Let $$\alpha^i  = \text{the number of subproblems at level } i$$

Let $$\frac{n}{b^i} = \text{the size of subproblems at level } i$$

$$ T(n) = 
\begin{cases}
O(n^k)  \text{ if } f(n) = O(n^{k-\epsilon}) \text{ for some } \epsilon > 0 \\ 
O(n^k \log^{p+1}{n})  \text{ if } f(n) = O(n^k \log^{p}{n}) \\
O(f(n))  \text{ if } f(n) = O(n^{k+\epsilon}) \text{ for some } \epsilon > 0 \\ 
\end{cases} $$

### Mergesort
![mergesort](/images/algs/divide/mergesort.png)


### Fast Exponentiation

![fast_exp](/images/algs/divide/fast_exp.png)

