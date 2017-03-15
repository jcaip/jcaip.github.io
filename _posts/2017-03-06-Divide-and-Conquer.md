---
published: True

images:

    - url: /images/algs/Greedy/InterviewScheduling.png
      alt: Interval Scheduling
      title: Interval Scheduling
---

## Divide and Conquer

### Master Theorem
The master theorem provides a solution for recurrence relations from divide and conquer algorithms.

Let some recurrence $$T(n) = \alpha T(\frac{n}{b}) + f(n)$$
Then consider the Recursion Tree of \\(T(n)\\)

Let $$k = \log_b{n}$$ be the depth of the recursion tree 

Let $$\alpha^i  = \text{the number of subproblems at level } i$$

Let $$\frac{n}{b^i} = \text{the size of subproblems at level } i$$

$$ T(n) = 
\begin{cases}
O(n^k)  \text{ if } f(n) = O(n^{k-\epsilon}) \text{ for some } \epsilon > 0 \\ 
O(n^k \log^{p+1}{n})  \text{ if } f(n) = O(n^k \log^{p}{n}) \\
O(f(n))  \text{ if } f(n) = \Omega(n^{k+\epsilon}) \text{ for some } \epsilon > 0 \\ 
\end{cases} $$

### Mergesort

### Fast Exponentiation

### Dropping Glass Jars

### Median of Medians
