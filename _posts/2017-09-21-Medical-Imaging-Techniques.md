---
layout: post
tags: [misc]
images: 
  - url: /images/medimg/cctc.png
    alt: some picture
    title: cctc graph

---
## X-Rays
Oscillating current creates electromagnetic waves

**Larmor Formula** - $$P = \frac{2}{3}\frac{m_e r_e a^2}{c}$$
The power of an electromagnetic wave is in terms of it's radius, mass, acceleration, and the speed of light.
<!--more-->

Light is created when electrons in a higher energy orbit fall to a lower energy orbit. The electron then releases the energy as light. 

**Bremsstrahlung** is radiation produced when a charged particle is deflected by another charged particle

![bremsstrahlung](/images/medimg/bremsstrahlung.png){: .center}

+ Bremsstrahlung output energy is determined by the deflection and therefore where the incident ray is headed
+ The max output energy is the input energy

![energy](/images/medimg/energy_spectrum.png){: .center}

**Charecteristic X-ray Energy** - Every atomic element has it's unique fingerprint that is the energy given off from dropping an energy level.

We use high voltage to accelerate electroms from a cathode to a rotating anode (for heat reasons), which produce X-rays with a small frequency.

![xray](/images/medimg/xray.png){: .center}

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
![mri](/images/medimg/mri.png){: .center}

The gradient coils are used to encode spatial location by gradually shifting the magnetic field and therefore changing the Larmor frequency.

We can pick which slice of the patient we want to image by changing the RF requency.

**k-space** is the Fourier Transformation of the MRI image. The center of k-space is he basic image contrast, while the periphery of k-space contain edges and details of the image.

There are many different types of MRI techniques, including FLIAR, GRE, and DWI.

**Perfusion MRI** - A contrast agent is injected that shortens the relaxation times within body tissues.

![cctc](/images/medimg/cctc.png){: .center}
Where the concetration is given by $$C(t) = -\frac{k}{TE}ln(\frac{S(t)}{S_0})$$

**CBF** - tissue supply of blood 

**CBV** - volume of blood in tissue

**MTT** - Mean transit time = CBF/CBV

**Central volumen principle** - Volume = Flow x MTTAIF

**Perfusion Angiogram** - use a DSA run to show time to peak for different tissues, can be used to measure bloodflow.

