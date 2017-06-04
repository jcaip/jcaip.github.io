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
The homogenous representation also makes it easy to represent affine transformations as matrix multiplication $$Q = \textbf{M}P$$.
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
    int x, y, dx, dy, d, dE, dNE;
    dx = x2-x1;
    dy = y2-y1;
    d = 2*dy-dx; //start point
    dE = 2*dy; //if we move one pixel east, the distance from the line is increased by dy
    dNE = 2*(dy-dx) //if we move one pixel northeast, the distance from the line is  increased by dy - dx
    y = y1;
    for (x=x1; x<=x2; x++)
    {
        SetPixel(x,y);
        if (d > 0) // next closest pixel is the NE pixel
            d = d + dNE;
            y = y + 1;
        else //next pixel is the E pixel
            d = d + dE
    }
}
```

To fill simple polygons, use the recursive **Flood-Fill** Algorithm.

```c
public void floodFill(int x, int y, int fill, int boundary)
{
    if (x<0 || x>=width)) return;
    if (y<0 || y>=height)) return;

    if (getPixel(x,y) != fill && getPixel(x,y) != boundary)
    {
        setPixel(x,y,fill);
        floodFill(x+1,y,fill,boundary);
        floodFill(x-1,y,fill,boundary);
        floodFill(x,y+1,fill,boundary);
        floodFill(x,y-1,fill,boundary);
    }
}
```

## Texture Mapping
$$(S_x, S_y) = T_{ws}(T_{tw}(s,t))$$

Textures are always images paramaterized by $$(s,t)$$ 

A better approach is to go from the screen to texture to avoid calculating pixel coverage.
$$(s,t) = T_{wt}(T_{sw}(S_x,S_y))$$

![texture](/images/cg/texturemapping.png)

This approach only works for vertices, so we can calculate texture coordinates by interpolation along interior points.

However, when coupled with a prespective projection, this causes distortion, since the steps are not all equal.

To make it work properly, we must do the perspective division after the interpolation across the polygon.

## Visibility
In order to improver performance, we don't want to draw polygons we can't see. An object is not visible if
+ Outside the view volume
+ Self-occlusion
+ Object-object occlusion

#### Clipping and Culling
Clipping is done in the CCS, we clip anything not in the 4D projection volume by finding the intersection between lines and planes and using that to restrict the paramtaterization paramter $$t$$.

Culling occurs when an entire polygon is outside of the view volume.

#### Backface Culling
Any backfacing polygons won't be seen by the camera.
We can efficiently do this by using the z-coordinate of the normal vector if it is an orthographic projection.

Culling in the VCS
+ Calculate the normal $$\textbf{n} = [a,b,c]^T$$ and the plane equation $$P$$
+ if $$P(eye) >0$$ that means that the eye is above the plane - front facing 
+ if $$P(eye) < 0$$ that means that the eye is below the plane - back facing 

#### Visibility Algorithms
**Image-space Algorithms** - operate on pixels/scan-lines e.g. Z-buffer

**Object-space Algorithms** - BSP, variations on the Painter's Algorithm. $$O(n^2)$$ time

#### Z-Buffer Algorithm
we generate z values along a scan line from either the plane equation or bilinear interpolation

```c
for all i,j
{
    Depth[i,j] = MAX_DEPTH
    Image[i,j] = BACKGROUND_COLOR
}
for all polygons P 
{
    for all pixels in P 
    {
        if (Z_pixel < Depth[i,j]) 
        {
            Image[i,j] = Color_pixel
            Depth[i,j] = Z_pixel
        }
    }
}
```
One drawback with Z-buffer is that there is only one visible surface per pixel, which makes it hard to do transparency.

We can use A-buffer instead, which contains assigns the average value of a linked-list of surfaces .

#### Binary Space Partition Trees
```c
BSPtree *BSPmaketree(polygon_list) {
    choose a polygon as the tree root
    for all other polygons
        if polygon is in front, add to front list
        if polygon is behind, add to behind list
        else split polygon and add one part to each list

BSPtree = BSPcombinetree(
    BSPmaketree(front_list),
    root,
    BSPmaketree(behind_list) )
}
```

#### Depth Sorting Algorithms
+ sort polygons by z
+ resolve ambiguities where z-extents overlap
+ scan-convert polygons in back-to-front order

Ambiguities are resolved by exchanging the order of surfaces or by creating new polygons that split up ambiguities.

#### Scanline Algorithms
```c
for each scanline (row) in image
    for each pixel in scanline
        determine closest object
        calculate pixel color, draw pixel
    end
end 
```
Less memory intensive and can handle multiple polygons.

## Shading
There are blocked, wireframe, and shaded rendering styles.

Light is usually emitted from light sources and reflects off of surfaces.
Some light is absorved, some reflected, and some refractable

Light is characterized as RGB triples, and we operate on each channel independently.

There are two types of reflection: **Diffuse** and **Specular** reflection.

#### Diffuse Reflection
Models dull, matte surfaces like chalk.

Calculated via **Lambert's Law**.
$$I =  I_L k_d cos\theta = I_L k_d(\textbf{n} \cdot \textbf{L})$$
Where $$I_L$$ is the initial light source intensity and $$k_d$$ is the diffuse coefficient.

#### Specular Reflection
Models shiny surfaces, most surfaces are imperfect specular reflectors (a combination of diffuse and specular reflection).

#### The Phong Model
Purely empirical model.
$$I =  I_L k_d cos\theta + I_L k_s {cos}^n \phi = I_L k_d(\textbf{n} \cdot \textbf{L}) + I_L k_s (\textbf{r} \cdot \textbf{v})^n$$

Where $$n$$ is the "shininess factor", $$k_s$$ is the specular coefficient, and $$\phi$$ is the angle between viewing and reflection.

$$\textbf{r} = 2\textbf{n}(\textbf{n}\cdot\textbf{L})- \textbf{L}$$

#### Blinn-Torrance Specular Model
This agress better with experimental results.

$$I_S = I_Lk_s(H \cdot V)^n$$ where $$H$$ is the halfway vector, or $$\frac{L+V}{\|L+V\|}$$

However, areas that are not directly illuminated appear black, which is not natrual. 

We can add a **ambient glow** term to our equation 
$$I =  I_L k_d cos\theta + I_L k_s {cos}^n \phi + I_ak_a$$ where $$I_a$$ is the ambient light intensity and $$k_a$$, which is a reflectance coefficient.

![lighting](/images/cg/lighting.png)

#### Types of Shading
Illumination equations are evaluated at surface locations, so we can evaluate at once at each polygon, which is called **flat shading**.

We can also evaluage it at every vertex and interpolate over the polygon, which is **Gouraud Shading**.

**Phong shading** takes this one step further by interpolating normals over each polygon and evaluating the illumination at each pixel.

We can deal with perspective distortion by using byperbolic interpertation. Furthermore, if we don't have smooth normals, colors may change if the polygon order changes.

This model also fails to account local phenomena, like shadows, attenuation, and transparent objects. Furthermore, global illumination is just approximated but not represented in our code.

#### Global Illumination
To approximate global illumination, we can use **Radiosity**. 

First we break the scene down into small patches, and assume unifrom reflection and emission per patch.
Since *Light Leaving = Emitted Light + Reflected Light*

$$B_iA_i = E_iA_i + R_i \sum_j{B_jF_{j,i}A_j}$$ where $$B_i$$ is the flux density.

We can then compute form factors $$F_{i,j} for 1 \leq i, j \leq n$$ and then solve a system of linear equations.

![radiosity_equation](/images/cg/radiosity.png)

![radiosity](/images/cg/radiosity_ex.png)

## Ray Tracing
The light that point $$P_A$$ emits comes from

+ light sources
+ reflection from other objects
+ refraction from other objects

![rt](/images/cg/rt.png)

Diffuse objects only receive light from light sources. Ray tracking works best for specular and transparent objects. 

It is easiest to trace rays backwards from eye to scene (eye-based), but it is possible to trace from source to eye (light-based).
```
  For each pixel on screen:
  	determine ray from eye through pixel
  	find closest intersection of ray with an object
  	cast shadow rays to light sources
  	recursively cast reflected and refracted ray
        calculate pixel color
        paint pixel
```

#### Setting the camera and the image plane
A pixel in the near plane $$P(r,c) = (u_c, v_r)$$ where
$$u_c = -W + W \frac{2c}{N_c-1}$$ and $$v_r = -H + H\frac{2c}{N_r-1}$$

To find the ray through a pixel, use the parametric form of a line.
$$ray(r,c,t) = eye + t(P(r,c) - eye)$$

We know that in the world coordinate system: 
$$P(r, c) = eye - N\textbf{n} + u_c\textbf{u} + v_r\textbf{v}$$

hence $$P(r,c) = eye + t(-N\textbf{n} +u_c\textbf{u}+v_r\textbf{v})$$

#### Finding Ray-Object Intersections
$$ray(t) = S + t\textbf{c}$$

$$Sphere(P) = \left\vert{P}\right\vert - 1 = 0$$

$$Sphere(ray(t)) = 0$$

$$\left\vert{\textbf{c}}\right\vert^2t^2 + 2(S \cdot t\textbf{c}) + \left\vert{S}\right\vert^2 -1 = 0$$

We can solve this for the two solutions (if two exist) and take the one with the smaller $$t$$ value to get the first intersection.

#### Transformed Primitives

$$P = MP' \implies P' = M^{-1} P$$

$$F(P') = F(T^{-1}(P))$$

Intersect the inverse-transformed ray with the untransformed primitive.

#### Shadow Rays
Intersect each shadow ray with all objects, apply illumination if not intersection.

![shdow_ray](/images/cg/refractedray.png)

#### Reflected Rays
$$Ray_{reflected} = P + t\textbf{v}$$ where $$\textbf{v} = -2(\textbf{n} \cdot \testbf{c})\textbf{n} + \textbf{c}$$

![reflected](/images/cg/reflectedray.png)

#### Refracted Rays
Calculate this using Snell's Law.

![refracted](/images/cg/refractedray.png)

Can be improved upon with participating media, translucency, sub-surface scattering, aperture effects, depth of field, or photon mapping.

## Sampling and Reconstruction

#### Aliasing in Graphics
One way to handle aliasing is to blur the image. 
Either prefiltering (before sampling) or postfiltering (after sampling)

**Prefiltering** - use average intensity of square pixel area.

**Bresenham's Algorithm** helps improve pixel coverage.

#### Filtering
Filter the step function using a convolutional kernel $$g$$.

$$F(x) = \int_{-s}^s{f(x+u)g(u)du}$$

We can use a **box filter**, but the problem is that area coverage is independent of position.

Instead, we can use a **Barlett Window**.

#### Supersampling
Take a lot of samples, and then combine them

We can do this **stochastically** 
![stochastic](/images/cg/stochastic.png)

We can also weight each sample to give them relative importance.
![importance](/images/cg/importance.png)

## Color
We see color via cones in our eye. There are three types of cones (SML) that correspond to RGB respectively.

![color](/images/cg/color.png)

**hue** is determined by the dominant wavelength.

**luminance** determined by total power of the light.

**saturation** is the percentage of luminance in the dominant wavelength.

#### RGB color model.

Represent colors a vector of three weights, $$w_r, w_b, w_g$$ that are all from 0 to 1.

![rgb](/images/cg/rgb_cube.png)

If we intersect this cube with the $$r+g+b=1$$ plane, we get the saturated color curve.

#### Color Matching
Aren't able to produce all wavelengths of light, so we try to match the distriution as best as possible. However we cannot keep negative coefficients, so we restrict this to an affine combination of (r,g,b)

![chromacity](/images/cg/CIE.png)

However, we cannot create all of the color scope with RGB. We are limited to the RGB **Color gamut**.

![gamut](/images/cg/color_gamut.png)

#### Additive and Subtractive Color
![rgbcmy](/images/cg/rgbcmy.png)

CRTs and other computer displays are an additive model, while printer ink (CMY) is a subtractive model.

#### Color conversion
We can use the CIE conversion to convert colors between two screens.

$$\begin{bmatrix}R' \\ G'\\B'\end{bmatrix} = 
\begin{bmatrix} X_R & X_G & X_B \\ Y_R & Y_G & Y_B \\ Z_R & Z_G & Z_B \end{bmatrix}
\begin{bmatrix} R \\ G \\ B \end{bmatrix}$$ 

 $$C_2 = M_2^{-1}M_1C_1$$

#### HSV Color Model
Hue, Saturation, and Value

![hsv](/images/cg/HSV.png)

## Geometric Modeling



