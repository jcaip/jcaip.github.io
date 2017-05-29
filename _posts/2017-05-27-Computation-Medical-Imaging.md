---
layout: post
title: Computational Medical Imaging

images:
    - url: https://upload.wikimedia.org/wikipedia/commons/thumb/e/e4/MeningiomaMRISegmentation.png/1024px-MeningiomaMRISegmentation.png
      alt: MRI segmentation
      title: medical imaging
---

## X-Rays
Oscillating current creates electromagnetic waves

**Larmor Formula** - $$P = \frac{2}{3}\frac{m_e r_e a^2}{c}$$
The power of an electromagnetic wave is in terms of it's radius, mass, acceleration, and the speed of light.

Light is created when electrons in a higher energy orbit fall to a lower energy orbit. The electron then releases the energy as light. 

**Bremsstrahlung** is radiation produced when a charged particle is deflected by another charged particle

![bremsstrahlung](/images/medimg/bremsstrahlung.png)

+ Bremsstrahlung output energy is determined by the deflection and therefore where the incident ray is headed
+ The max output energy is the input energy

![energy](/images/medimg/energy_spectrum.png)

**Charecteristic X-ray Energy** - Every atomic element has it's unique fingerprint that is the energy given off from dropping an energy level.

We use high voltage to accelerate electroms from a cathode to a rotating anode (for heat reasons), which produce X-rays with a small frequency.

![xray](/images/medimg/xray.png)

We use an antiscatter grid to block most of the off-axis scatter radiation.

There are two main medical uses for X-rays.

**Projection Radiography** - project Xray onto film and see results.

**Computed Tomography** - Generate volumetric information from a large serires of 2D Xrays around a single axis of rotation.

The attenuation of X-rays is given by **Beer-Lambert Law**
$$ N = N_0e^{-\mu t}$$, where $$N$$ is the number of photons, $$t$$ is the thickness of the object, and $$\mu$$ is the linear attenuation coefficient - which represents the probability the xray-photon will be attenuated.

The linear attenuation coefficient is a function of all the interaction probabilities of all different interaction types. High attenuation shows up lighter, while low attenuation shows up darker.

### Basic imaging equation

$$ I_d(x,y) = \int_0^{\varepsilon_{max}}{\nu (\varepsilon) I_0(\varepsilon) exp(- \int{\mu(x,y,z;\varepsilon) dz}) d\varepsilon} $$

$$I_d(x,y)$$ is the projection image at location $$(x,y)$$

$$\nu (\varepsilon)$$ is the quatum efficency at energy $$\varepsilon$$

$$I_0(\varepsilon)$$ is the incident X-ray spectrum

$$- \int{\mu(x,y,z;\varepsilon) dz}$$ is the attenuation at location $$(x,y)$$ for energy level $$\varepsilon$$

**Degeneracy** occurs when two different objeccts produce the same effective attenuation

**Hounsfield unit** is a measure of attenuation based on distilled water

Using **forward projection** at different angles gives us different profiles of the image, which we use to construct the image using **backprojection**.

We can filter each attenuation profile to get back a cleaner image using fourier transforms.

**CT Angiogram** - combines CT scan with an injection of contrast media to see blood vessels.

**Digital Subtraction Angiogram** - compares the difference of two images of a region of the body.

## MRI
composed of a magnet, RF coil, and gradient coil

Uses Nuclear Magnetic Resonance to generate images. Protons have their own particle spin, which creates a small magnetic field. 

When exponsed to a stong magnetic field, all the protone moments line upand start precessing ah the **Larmor frequencey** $$\omega = \gamma B_0$$

Exposing protons to a frequency equal to the Larmor frequency absorbs their energy and deflects the axis of rotation to the imaging plane and all rotate in phase.

After turning off the initial RF pulse, the protons gradually relax back, creating AC current at the Larmor frequency in the RF coil.

#### T1-Relaxtion: Recovery
The proton recovers it's prior longitudinal orientation.
#### T2-Relaxtion: Dephasing
Loss of transverse magnetization, and occurs much faster than T1

These relaxtions are different depending on the surrounding materials, which we can then measure.
![mri](/images/medimg/mri.png)

The gradient coils are used to encode spatial location by gradually shifting the magnetic field and therefore changing the Larmor frequency.

We can pick which slice of the patient we want to image by changing the RF requency.

**k-space** is the Fourier Transformation of the MRI image. The center of k-space is he basic image contrast, while the periphery of k-space contain edges and details of the image.

There are many different types of MRI techniques, including FLIAR, GRE, and DWI.

**Perfusion MRI** - A contrast agent is injected that shortens the relaxation times within body tissues.

![cctc](/images/medimg/cctc.png)
Where the concetration is given by $$C(t) = -\frac{k}{TE}ln(\frac{S(t)}{S_0})$$

**CBF** - tissue supply of blood 

**CBV** - volume of blood in tissue

**MTT** - Mean transit time = CBF/CBV

**Central volumen principle** - Volume = Flow x MTTAIF

**Perfusion Angiogram** - use a DSA run to show time to peak for different tissues, can be used to measure bloodflow.

## Linear Image Transformations
[Computer Graphics Notes](/Computer-Graphics) have information about types of transformations and homogenous coordinates.

#### Image Interpolation
Direct interpolation lead to black spots, so instead we use indirect interpolation.

![types of interpolation](/images/medimg/interpolation.png)

For Bilinear interpolation $$I_{x,y} = \omega_4I_{u,v} + \omega_3I_{u+1,v} + \omega_2I_{u,v+1} + \omega_1I_{u+1,v+1} $$ 

Cubic interpolation retains grey values but requires 10x more computation than NN and 2x more computation than bilinear interpolation.

## Image Filtering

There are two main types of filers - **high-pass** filters that detect change and **low-pass** filters that remove noise.

Can either be applied in the frequency domain via a Foruier transform or in the spatial domain via a convolution.

We can modify the pixels in the image either point-wise, locally, or globally.

![outputsize](/images/medimg/outputsize.png)

We can handle borders by either clipping, wrapping around, propogating the edge, or reflecting across an edge.

#### Filtering by Convolution
$$I'(x,y) = \sum_{i=-1}^{1}\sum_{j=-1}^{1}{I(x+i, y+j) \times filter(i,j)}$$

Convolutional filters are commutative, associative, and distributive.

![Types of low pass filters](/images/medimg/lowpass.png)

By subtracting the low-pass filter from the source image, we get a high-pass filter.


#### Using Graidents and Laplacians
To improve edge detection, we can take the derivative/2nd derivative of the signal via a convolution using the numerical approximation of the derivative.

![filters](/images/medimg/gradfilters.png)

Some filters are **seperable**, which means they can be expressed as the product of two vectors.
Since convolutions are associative, this means we can run in $$O(M)$$ instead of $$O(M^2)$$

We can sharpend images by applying a high pass filter and then adding this detail back into the image.

Filtering images via convolutions also lets us apply **deconvolutions** to images, which allow us to extract original images. 

However, the convolution matrix is not square so to inverse it we use the **Moore-Penrose Pseudoinverse** $$(A^TA)^{-1}A^T$$

## Feature Detection
We can use blob detection to find regions of interest. 

Ideally, we want **scale invariance** so that the scale of key features do not matter.

To do this, we can use edge detection using the Laplacian, or 2nd derivative. When the second derivative crosses 0, we know there is an edge. 

However, the max response occurse when the zeros of the laplacian line up with the circle, $$\sigma = r/\sqrt{2} $$. If the scale is too large we may not detect images, while if it is too small we may have too much noise.

To get around this, we can convolve the image with the Laplacian at several scales and take the max of the response to find blobs of all sizes.

#### Affine Covariance
Not all blobs are circular, must deal with affine transformations.

To deal with rotation, we can instead take a histogram of pixel values.

Alternatively, we can compute the average/max of the gradients for each portion of the image, and use this to align the images.

![gradient_align](/images/medimg/gradient_align.png)

#### SIFT
SIFT is a histogram of the gradients of the image across small patches.

![sift](/images/medimg/sift.png)


#### Corners and Interest Points
Humans naturally use corners and other points of interest to aligh ourselves. These points are usually represented by a large change in color, intensity, or texture.

Can be used for image registration, stereo matching, or panorama stitching.

Ideally, we should be able to use the gradient to detect corners.
!![corner](/images/medimg/corner.png)

**Harris Corner Detector** - $$A_W = \sum_{x \exists W, y \exists W}{w(x,y) \begin{bmatrix} f_x^2 & f_xf_y \\ f_xf_y & f_y^2 \end{bmatrix}}$$

where $$w(x,y)$$ is some smoothing function.

![harris](/images/medimg/harris.png)

This detection is rotation invariant, but sensitive to changes in scale, viewpoint, and contrast.

We need some way to find the **characteristic scale** of features. The best mthod to do so is the Laplacian of the Gaussian function, or 
$$\left\vert{\sigma^2(L_{xx}(\x, \sigma) +L_{yy}(\x, \sigma ))}\right\vert$$
