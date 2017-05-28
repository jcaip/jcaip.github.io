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
\begin{bmatrix} P_1 \\ P_2 \\ P_3 \\ 1 \end{bmatrix} $$

## Affine Transformations
The homogenous representation also makes it easy to represent affine transformations as matrix multiplication $$Q = \textbf{M}P.
There are four basic affine transformations, and every other affine transformation is a combination of these four transformations.

#### Translation
$$ \begin{bmatrix} Q_x \\ Q_y \\ Q_z \\ 1 \end{bmatrix}  =
\begin{bmatrix} 1 & 0 & 0 & T_x \\ 0 & 1 & 0 & T_y \\
0 & 0 & 1 & T_z \\ 0 & 0 & 0 & 1 \end{bmatrix}
\begin{bmatrix} P_x \\ P_y \\ P_z \\ 1 \end{bmatrix} $$

To apply an inverse
$$ \textbf{M}^{-1} =
\begin{bmatrix} 1 & 0 & 0 & -T_x \\ 0 & 1 & 0 & -T_y \\
0 & 0 & 1 & -T_z \\ 0 & 0 & 0 & 1 \end{bmatrix} $$

#### Rotation
$$ \begin{bmatrix} Q_x \\ Q_y \\ Q_z \\ 1 \end{bmatrix}  =
\begin{bmatrix} cos \theta & -sin \theta  & 0 & 0 \\ sin \theta & cos \theta & 0 & 0 \\
0 & 0 & 1 & 0 \\ 0 & 0 & 0 & 1 \end{bmatrix}
\begin{bmatrix} P_x \\ P_y \\ P_z \\ 1 \end{bmatrix} $$

To apply an inverse
$$ \textbf{M}^{-1} = \textbf{M}^T $$

This is because, $$\textbf{M}$$ is an orthonormal matrix.

Note that with 3D rotation, it is possible to run into gimbal lock, where we lose one degree of freedom.

#### Scaling
$$ \begin{bmatrix} Q_x \\ Q_y \\ Q_z \\ 1 \end{bmatrix}  =
\begin{bmatrix} s_x & 0 & 0 & 0 \\ 0 & s_y & 0 & 0 \\
0 & 0 & s_z & 0 \\ 0 & 0 & 0 & 1 \end{bmatrix}
\begin{bmatrix} P_x \\ P_y \\ P_z \\ 1 \end{bmatrix} $$

To apply an inverse
$$ \textbf{M}^{-1} =
\begin{bmatrix} \frac{1}{s_x} & 0 & 0 & 0 \\ 0 & \frac{1}{s_y} & 0 & 0 \\
0 & 0 & \frac{1}{s_z} & 0 \\ 0 & 0 & 0 & 1 \end{bmatrix} $$

#### Shear
$$ \begin{bmatrix} Q_x \\ Q_y \\ Q_z \\ 1 \end{bmatrix}  =
\begin{bmatrix} 1 & a & 0 & 0 \\ 0 & 1 & 0 & 0 \\
0 & 0 & 1 & 0 \\ 0 & 0 & 0 & 1 \end{bmatrix}
\begin{bmatrix} P_x \\ P_y \\ P_z \\ 1 \end{bmatrix} $$

To apply an inverse
$$ \textbf{M}^{-1} =
\begin{bmatrix} 1 & -a & 0 & 0 \\ 0 & 1 & 0 & 0 \\
0 & 0 & 1 & 0 \\ 0 & 0 & 0 & 1 \end{bmatrix} $$

## Projection Transformations
A projection is basically a mapping from $$ R^n \rightarrow R^m$$ where $$ n > m$$

There are many different types or projections
![projections](\images\cg\taxonomy_projections.png)

#### Basic Orthographic Projection
![projections](\images\cg\basic_orthographic.png)

$$ \begin{bmatrix} Q_x \\ Q_y \\ Q_z \\ 1 \end{bmatrix}  =
\begin{bmatrix} 1 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 \\
0 & 0 & 0 & -N \\ 0 & 0 & 0 & 1 \end{bmatrix}
\begin{bmatrix} P_x \\ P_y \\ P_z \\ 1 \end{bmatrix} $$

#### Perspective Projection
![projections](\images\cg\perspective.png)

$$ \begin{bmatrix} Q_x \\ Q_y \\ Q_z \\ 1 \end{bmatrix}  =
\begin{bmatrix} 1 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 \\
0 & 0 & 1 & 0 \\ 0 & 0 & -\frac{1}{N} & 0 \end{bmatrix}
\begin{bmatrix} P_x \\ P_y \\ P_z \\ 1 \end{bmatrix}  \div \frac{-P_z}{N} $$

## View Volumes
Viewports undo the distortion of the projection transformation and map pixel coordinates on screen. 

### Wireframe Displays
``` 
Compute $$M_mod$$
Compute $$M^{-1}_cam$$
Compute $$M_{modelview} = M^{-1}_{cam} M_{mod}$$
Compute $$M_O$$
Compute $$M_P$$
Compute $$M_{proj} = M_OM_P$$
Compute $$M_{vp} = M_OM_P$$
Compute $$M = M_{vp}M_{proj}M_{modelview}$$

for each line segment $$i$$ between $$P_i$$ and $$Q_i$$ do
    $$P = MP, Q=MQ$$
    drawline($$P,Q$$)
```

## Line Rasterization
We want to convert everything into pixels, including lines.

**Bresenham Midpoint Algorithm** - maps line to pixels using the implicit form of the line and provides a fast way of choosing the next pixel.

$$ F(x, y) = x dy - y dx + c$$

```c
void drawLine(int x1, int y1, int x2, int y2)
{
    int x, y;
    y = y1;
    for (x=x1; x<=x2; x++)
    {
        SetPixel(x,y);
        if (F(x+1, y+0.5) > 0) //if >0, then M is below line, so pick the NE pixel
            y = y+1;
        //otherwise, M is on/above line so just choose the E pixel
    }
}
```




## Ray Tracing
The light that point $$P_A$$ emits comes from

+ light sources
+ reflection from other objects
+ refraction from other objects

Diffuse objects only receive light from light sources

#### Ray Tracing Algorithm
It is easiest to trace rays backwards from eye to scene.
```
  For each pixel on screen:
  	determine ray from eye through pixel
  	find closest intersection of ray with an object
  	cast shadow rays to light sources
  	recursively cast reflected and refracted ray
    return color
```

1. Setting the camera and the image plane
Use parametric form $$P(t)  = P_0 + \textbf{v}t$$

World coordinate system: $$P(r, c) = eye - N\textbf{n} + u_c\textbf{u} + v_r\textbf{v}$$

$$ray(t) = S + t\textbf{c}$$

$$Sphere(P) = \left\vert{P}\right\vert - 1 = 0$$

$$\left\vert{\textbf{c}}\right\vert^2t^2 + 2(S \cdot \textbf{c}) + \left\vert{S}\right\vert^2 -1 = 0$$

How to deal with transformed primitives?
$$P = MP'$$

$$F(P') = F(T^{-1}(P))$$

##### Shadow Rays
For each light source,intersect shadow ray with all objects. If no intersection is found, apply local illumination. If in shadow, no contribution from that light ray.
