---
title: Rewrite Rules
classes: wide

research:
  institute: Georgia Institute of Technology
  PI: Dr. Jun Ueda
  date: July 2022
  funding:
    body:  National Science Foundation
    grant: "2112793"
  publication:
    title: Rewrite Rules for Automated Depth Reduction of Encrypted Control Expressions with Somewhat Homomorphic Encryption
    url: https://doi.org/10.1109/AIM52237.2022.9863395

arithmetic_circuits:
    - image_path: /assets/rewrites/arithmetic-circuit-unweighted.svg
      url: /assets/rewrites/arithmetic-circuit-unweighted.svg
      title: "Arithmetic circuit with unweighted multiplication gates"

    - image_path: /assets/rewrites/arithmetic-circuit-weighted.svg
      url: /assets/rewrites/arithmetic-circuit-weighted.svg
      title: "Arithmetic circuit with weighted multiplication gates"

associative_rewrite_simple:
    - image_path: /assets/rewrites/simple-associative-before-circled.svg
      url: /assets/rewrites/simple-associative-before-circled.svg
      title: "Associative Rewrite Before: Simple"

    - image_path: /assets/rewrites/simple-associative-after-circled.svg
      url: /assets/rewrites/simple-associative-after-circled.svg
      title: "Associative Rewrite After: Simple"

associative_rewrite:
    - image_path: /assets/rewrites/associative-before-circled.svg
      url: /assets/rewrites/associative-before-circled.svg
      title: "Associative Rewrite Before"

    - image_path: /assets/rewrites/associative-after-circled.svg
      url: /assets/rewrites/associative-after-circled.svg
      title: "Associative Rewrite After"
---

{% include load-mathjax %}
{% include research-funding-disclosure-statement %}

# Research Goal
* Construct Encrypted Controller
  - Since we are using SHE we have a limited *noise budget*
  - Need to take proactive steps to "sort" the expression to find optimal form $(x+y)^2$ vs $x^2 + 2xy + y^2$.
* Develope algorithmic procedures to optimize an *expression tree/arithmetic circuit* to maximize width
  - Deep circuits represent large chains of consecutive multiplications, thus width is preferred
  - Wide circuits can easily be parallelized so it is okay if the solution is larger id width then depth

{% include figure 
    popup=true 
    image_path="/assets/rewrites/encrypted-controller-cartoon.svg"
    caption="" %}

{% include figure 
    popup=true 
    image_path="/assets/rewrites/plaintext-cipherspace-homomorphism.svg"
    caption="" %}


# What is an Arithmetic Circuit?
An *arithmetic circuit* is a directed acyclic graph, where leaf nodes are inputs and internal nodes are arithmetic operations such as addition or multiplication.
In these graphs nodes which only have incoming connection but no outgoing connections are termed *circuit outputs*
An example circuit is shown below:

{% include figure 
    popup=true 
    image_path="/assets/rewrites/simple-sum.svg"
    caption="Arithmetic circuit of $(a+b)$" %}

Creating functional encrypted calculations revolves around managing *cipher noise*, an intrinsic aspect of HE where arithmetic operations inject noise into the ciphertext.
If enough noise accumulates the calculation can no longer be decrypted and the data is lost.
Multiplication produces orders of magnitude more noise than addition, so much so that many choose to treat addition as "free" and focus only on optimization for fewer multiplications.

While multiplication is bad, *consecutive multiplication* is far worse.
This is because the amount of noise injected by a multiplicative operation increases each time that value is multiplied.
For this reason we keep track of the multiplicative depth of each node with $\Delta$.
That is, all inputs start with a $\Delta = 1$ and for each multiplication gate the circuit passes through the $\Delta$ will increment by $1$.
This can be seen in the simple example below:

{% include figure 
    popup=true 
    image_path="/assets/rewrites/simple-product.svg"
    caption="Arithmetic circuit of $(a+b) \times c$ equation. The $\Delta$ label on each node denotes that node's *multiplicative depth*, notice the output depth is $2$ while all other nodes have a depth of $1$." %}

This research will explore automated *rewrite rules* that can be applied to reduce the depth of an arithmetic circuit.
Two major rewrites--the *associative rewrite* and the *distributive rewrite*--are explored in the following section.

# Rewrites
* You need to explain them both at a high level
* Show the depth decreasing rewrites:
  - single associative
  - associative plus distributive
## Associative Rewrite
{% include gallery 
    id="associative_rewrite_simple"
    caption="" %}

{% include gallery 
    id="associative_rewrite"
    caption="" %}

## Distributive Rewrite

# Video Presentation

<video width="320" height="240" controls>
  <source src="/assets/rewrites/recorded-presentation.mp4">
</video>

# Experimental Outcome (RP Manipulator Dynamics)

* Possibly might not include this to simply the page
{% include figure 
    popup=true 
    image_path="/assets/rewrites/rp-manipulator.svg"
    caption="" %}

## Weighted vs Unweighted Circuits
{% include gallery 
    id="arithmetic_circuits"
    caption="" %}