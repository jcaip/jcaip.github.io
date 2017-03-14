---
layout: post

images:

    - url: /images/algs/dp/APShortestPath.png
      alt: Loss Function
      title: Loss Function
---

### What is the idea behind dynamic programming?
Dynamic Programming breaks down problems into subproblems, then 
+ solves those subproblems from smallest -> largest
+ uses the solution of those subproblems to solve the larger problem

### All-Pairs Shortest Path
This is a shorted path implementation that works even with negative weights

$$ X = Y $$

$$
    OPT(m,n) = \min
    \begin{cases}
    	\mathbb{1}(x_m \neq y_m) + OPT(m-1, n-1) \\
        1 + OPT(m-1, n) \\
        1 + OPT(m, n-1)
    \end{cases}
$$

$$
  \begin{algorithm}
  \caption{Computing Edit Distance}
  \algorithmicrequire{Two strings $X = x_1 \ldots x_n$ and $Y = y+i \ldots y_m$. } \\
  \algorithmicensure{The minimum number of edits to convert $X$ to $Y$.}
  \begin{algorithmic}[1]
  \Procedure{Compute Edit Distance}{$X, Y$}
  \State Initialize a matrix $OPT$ of size $n \times m$
  \State $OPT[i, 0] \gets i$ $\forall i = 1 \ldots n$
  \State $OPT[0, j] \gets j$ $\forall j = 1 \ldots m$
  \For{$i = 1 \ldots n$}
  \For{$j = 1 \ldots m$}
  \State $	OPT[i,j] = \min
    \begin{cases}
    	\mathbb{1}(x_i \neq y_j) + OPT[i-1, j-1] \\
        1 + OPT[i-1, j] \\
        1 + OPT[i, j-1]
    \end{cases}$
  \EndFor
  \EndFor
  \State \textbf{return} $OPT[n,m]$
  \EndProcedure
  \end{algorithmic}
  \end{algorithm}
$$

![sp](/images/algs/dp/APShortestPath.png)

### Computing Levingstam Distance
This is a measusre of the similarity of two strings.
![ces](/images/algs/dp/ComputingEditDistance.png)

### Knapsack Problem
![ces](/images/algs/dp/KnapsackProblem.png)

### Weighted Interval Scheduling
![wis](/images/algs/dp/WeightedIntervalScheduling.png)

### Matric Chain Multiplication
![ces](/images/algs/dp/MatrixChainMultiplication.png)
