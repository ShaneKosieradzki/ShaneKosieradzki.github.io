---
title: Dyer's vs BFV
classes: wide

# Project portfolio page
header:
  teaser: /assets/dyers-vs-bfg/encrypted-controller-cartoon-proposal.svg
excerpt: "World's first proposed encrypted control scheme based on Somewhat Homomorphic Ciphers. Allowing for complete concealment of controller operation."

# Funding Disclosure Statement
research:
  institute: Georgia Institute of Technology
  PI: Dr. Jun Ueda
  date: November 2022
  funding:
    body:  National Science Foundation
    grant: 2112793
  publication:
    title: Secure Teleoperation Control Using Somewhat Homomorphic Encryption
    url: https://doi.org/10.1016/j.ifacol.2022.11.247

# Minimal-Mistakes::Gallery
cipher_timing:
  - image_path: /assets/dyers-vs-bfg/operation-time-bfg.png
    url: /assets/dyers-vs-bfg/operation-time-bfg.png
    title: "Operation times for the BFG cipher."

  - image_path: /assets/dyers-vs-bfg/operation-time-dyers.png
    url: /assets/dyers-vs-bfg/operation-time-dyers.png
    title: "Operation times for the Dyer's cipher."

---

{% include load-mathjax %}


$\newcommand{\coloneqqf}{\mathrel{\vcenter{:}}=}$

{% include research-funding-disclosure-statement %}

This project was the first of my graduate studies, and developed foundational ideas used in the remainder of my graduate research projects. As such, it is recommended to read this page before the other *project pages*. 
{: .notice--success}



<!-- \\[
    u[k] = \Phi \xi[k] + \Psi \sigma[k] \coloneqqf f^{\text{SHE}}\left( \Phi, \xi[k], \Psi, \sigma[k]  \right)
\\] -->

# Encrypted Controller
An *encrypted controller* is a digital controller running inside the encrypted-space of a homomorphic cipher--a type of cipher that allows computation on encrypted data.
Given the great computational burden imposed by encrypted calculation and the real-time requirements of digital controllers, encrypted controllers are extremely difficult to construct.

This research dives head first into this challenge and attempts to construct a digital controller using **Somewhat Homomorphic Encryption (SHE)**--a class of ciphers that support both addition and multiplication but has a finite number of allowed operations before the data corrupts.

Using SHE we can completely conceal all sensor signals, model parameters, feedback gains, and perform computations in the ciphertext space to generate motion commands without a security hole.
This allows us to keep every aspect of our control system secret from a cloud provider hosting our controller.

{% include figure 
    popup=true 
    image_path="/assets/dyers-vs-bfg/encrypted-controller-cartoon-proposal.svg"
    caption="" %}

As of time of writing most of the encrypted controller designs proposed in the academic literature have used so called **Partially Homomorphic Encryption (PHE)**, which only allows either multiplication or addition but not both.

## PHE Controller
PHE ciphers such as *RSA* (multiplicative), *ElGamal* (multiplicative), *Paillier* (additive) have been around for awhile and are fast enough under the single encrypted operation they offer.
Because of this they have been the primary ciphers of study in encrypted controls research.

Since a PHE cipher can only perform a single operation the other operation must be done at the plant.
Typically the multiplications are performed in the cloud and the additions are performed plant-side.
This however leaves partial information exposed when the plant decrypt the received products from the cloud.
The result is that information about the controller's operations are leaked plant-side when using PHE.

This information leak from using PHE is depicted below:

{% include figure 
    popup=true 
    image_path="/assets/dyers-vs-bfg/encrypted-controller-cartoon-phe.svg"
    caption="" %}

This research seeks to address the security holes offered by PHE based encrypted controllers by constructing a SHE based encrypted controller.
Note that SHE in general is also known to be computationally expensive and its application to real-time control is considered to be infeasible. 
However, this and the other research projects which compose my MS thesis, will show that with proper parameter selection and accompanying support circuits, SHE based encrypted control is possible.

## SHE Controller
The use of SHE in encrypted control systems has been considered impossible in past years due to their operation limit and slow computation time, however this research shows the feasibility of a SHE encrypted controller.

Since SHE supports both addition and multiplication we do not need to worry about information leak as we did in the PHE based system.
This is because **all** operations (i.e. both addition and multiplication) are being done in cipher space, so no intermediate computation is revealed.

We can see the increased security configuration offered by SHE in the diagram below:

{% include figure 
    popup=true 
    image_path="/assets/dyers-vs-bfg/encrypted-controller-cartoon-she.svg"
    caption="" %}

While SHE solves our information leak problem it imposes its own technical challenges which must be overcome.
Since the number of operations are limited we must take great care to make sure our control equation can be computed within the allowed *operation budget*, something addressed more in [another project](/projects/rewrite-rules.md).

# Teleoperation

<span style="color:red">TODO: Link to dedicated teleoperation page (teleoperation is used in 3 different projects)</span>.


## Timing Results
{% include gallery 
    id="cipher_timing"
    caption="" %}

