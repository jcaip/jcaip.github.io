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
