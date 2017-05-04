---
layout: post
title: Computer Graphics

images:
    - url: /images/cg/camera_model.png
      alt: camera
      title: camera
---
## Points, Lines, Vectors, Planes
A **linear combination** of $$m$$ vectors is given by 
$$ \textbf{w} = a_1\textbf{v}_1 + \ldots + a_m\textbf{v}_1 $$

An **affine combination** is a linear combination such that
$$ \sum_{i=1}^{m}{a_i} = 1$$

A **convex combination** is a linear combination where
$$a_i \geq 0 \text{ for } i=1 \ldots m$$

#### Homogenous Representation
Vectors and points are represented as $$ 4 \times 1$$ column matrices

$$\textbf{v} = \begin{bmatrix} v_1 \\ v_2 \\ v_3 \\ 0 \end{bmatrix}$$
$$P = \begin{bmatrix} P_1 \\ P_2 \\ P_3 \\ 1 \end{bmatrix}$$

The difference of two points is a vector:

$$P = \begin{bmatrix} P_1 \\ P_2 \\ P_3 \\ 1 \end{bmatrix}$$
$$Q = \begin{bmatrix} Q_1 \\ Q_2 \\ Q_3 \\ 1 \end{bmatrix}$$
$$P - Q = \textbf{v} = \begin{bmatrix} P_1 - Q_1 \\ P_2-Q_2 \\ P_3-Q_3 \\ 0 \end{bmatrix}$$

#### Lines
There are three main forms for lines:

|Explicit | Implicit | Parametric |
|---------|----------|------------|
|$$y = mx + B$$ | $$f(x,y) = (x-x_o)dy - (y-y_0)dx$$ | $$ x(t) = x_0 + t(x_1 - x_0)\\ x(t) = y_0 + t(y_1 - y_0)$$|

Explicit representation has trouble representing vertical lines, while implicit representation to check if a point is on ($$f(x,y) = 0$$ ), above ( $$f(x,y) > 0$$ ), or below ($$f(x,y) < 0 $$ ) the line.

Parametric makes it easy to draw the line with regards to another paramater, $$t$$

## Coordinate Systems
With the homogenous representaiton, we can define coordinate systems.
A coordinate system is determined by three linearly independent vectors, 
$$\textbf{a, b, c} $$ and an origin point, $$O$$.

We can see the reason behind the 0 and 1 in the homogenous representation to represent vectors and points.

$$ \textbf{v} = v_1 \textbf{a} + v_2 \textbf{b} + v_3 \textbf{c} = 
\begin{bmatrix} \textbf{a } \textbf{b } \textbf{c } O \end{bmatrix}
\begin{bmatrix} v_1 \\ v_2 \\ v_3 \\ 0 \end{bmatrix} $$

$$ P = O + v_1 \textbf{a} + v_2 \textbf{b} + v_3 \textbf{c} = 
\begin{bmatrix} \textbf{a } \textbf{b } \textbf{c } O \end{bmatrix}
\begin{bmatrix} v_1 \\ v_2 \\ v_3 \\ 1 \end{bmatrix} $$

## Affine Transformations
The homogenous representation also makes it easy to represent affine transformations.
There are four basic affine transformations, and every other affine transformation is a combination of these four transformations.

#### Translation

## Projection Transformations
