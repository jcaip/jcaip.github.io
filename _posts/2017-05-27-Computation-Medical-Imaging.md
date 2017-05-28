---
layout: post
title: Computational Medical Imaging

images:
    - url: https://en.wikipedia.org/wiki/Medical_image_computing#/media/File:MeningiomaMRISegmentation.png
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

Projection Radiography - project Xray onto film and see results.

Computed Tomography - Generate volumetric information from a large serires of 2D Xrays around a single axis of rotation.

The attenuation of X-rays is given by **Beer-Lambert Law**
$$ N = N_0e^{-\micro t}$$, where $$N$$ is the number of photons, $$t$$ is the thickness of the object, and $$\micro$$ is the linear attenuation coefficient - which represents the probability the xray-photon will be attenuated.
