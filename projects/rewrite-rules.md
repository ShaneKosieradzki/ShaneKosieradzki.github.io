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

distributive_rewrite:
    - image_path: /assets/rewrites/distributive-before-circled.svg
      url: /assets/rewrites/distributive-before-circled.svg
      title: "Distributive Rewrite Before"

    - image_path: /assets/rewrites/distributive-after-circled.svg
      url: /assets/rewrites/distributive-after-circled.svg
      title: "Distributive Rewrite After"
---

{% include load-mathjax %}
{% include research-funding-disclosure-statement %}

<p style="display:none">

$\newcommand{\associativeExpressionBefore}{\left( \left( \left( a \cdot b \right) \cdot c \right) \cdot \left( d \cdot e \right) \right)  \cdot \left( f \cdot g \right)}$
$\newcommand{\associativeExpressionAfter}{\left( \left( f \cdot g \right) \cdot \left( d \cdot e \right) \right)  \cdot \left( \left( a \cdot b \right) \cdot c \right)}$

$\newcommand{\distributiveExpressionBefore}{\left( \left( \left( a \cdot b \right) \cdot c \right) \cdot \left( d \cdot e \right)  + f + g \right) \cdot \left( h \cdot i\right)}$
$\newcommand{\distributiveExpressionAfter}{\left( \left( a \cdot b \right) \cdot c \right) \cdot \left( d \cdot e \right)  \cdot \left( h \cdot i\right) + \left(f + g \right) \cdot \left( h \cdot i\right) }$

</p>


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

## Associative Rewrite
The associative rewrite represents a regrouping of parameters as in $(a \cdot b) \cdot c = a \cdot (b \cdot c)$, 
 and is only applicable to two adjacent `MUL` gates.
For example, if $a$ has a greater depth than both $b$ and $c$, i.e.

\\[\text{depth}(b), \text{depth}(c) < \text{depth}(a)\\]

then, an associative rewrite will decrease the overall depth of the circuit.

This is depicted below, note however that $a$ represents some subcircuit with depth $3$ and is not a circuit input as in the previous example.
{% include gallery 
    id="associative_rewrite_simple"
    caption="A demonstration of the rewrite $(a \cdot b) \cdot c \to a \cdot (b \cdot c)$. Before the rewrite the node $a$ (blue dashed circle) has higher depth than the other nodes. It is identified that if $a$ swaps places with $c$ (red dot-dashed circle) then the overall depth of the circuit will decrease by one." %}
Since $\text{depth}(a)$ is the greatest of all nodes considered, it is counterproductive to have node $a$ deep in the graph.
By swapping node $a$ and $c$, we reduce the number of future `MUL` gates that $a$ will have to pass through by one.

Consider the more complex example $\associativeExpressionBefore$.
Looking at this expression we can observe that it is *left-heavy*, meaning the left side of the tree is significantly deeper than the right side.
This is exactly the same as the first associative rewrite example we considered above.
{% include gallery 
    id="associative_rewrite"
    caption="Associative rewrite taking $\associativeExpressionBefore$ and converting it into the more balanced $\associativeExpressionAfter$. The orange colored nodes represent elements of the *critical path* which is the path of maximum depth." %}

Notice the associative rewrite swapped $\left( \left( a \cdot b \right) \cdot c \right)$ with the $\left( f \cdot g \right)$ term.
This is because associative rewrite has the tendency of convert graph depth to graph breadth, thus steering the graph towards a more *balanced* configuration.

The path shown in orange is the "critical path" that results in the maximum depth of the circuit.
A well balanced tree will have almost all its nodes in the critical path.

## Distributive Rewrite
The distributive rewrite is not able to lower circuit depth, unlike the associative rewrite, and in fact adds an additional `MUL` gate into the circuit.
In spite of this, the distributive rewrite is advantageous by moving a `MUL` gates higher up in the circuit to be adjacent to a different `MUL` gate.
When these newly neighbored gates satisfy the pattern described by <span style="color:red">\eqref{eq:associative-rewrite-requierment}</span>, the distributive rewrite will facilitate a future associative rewrite with depth reducing ability at the cost of net one additional `MUL` gate.

This is shown in the figure below:
{% include gallery 
    id="distributive_rewrite"
    caption="Distributive rewrite rule takin in $\distributiveExpressionBefore$ and converting it to $\distributiveExpressionAfter$. The circled regions show sub-graphs that were rearranged, while the uncircled represent newly created gates." %}

The distributive rewrite represents the distribution of an outside factor to a term in a sum, such as $(((x+y_1)+...)+y_k) \cdot z$.
The na&#239;ve approach would be to distribute the $z$ to every addend, such as: $(x z) + (y_1 z) + ... + (y_k z)$.
This however, will add a `MUL` gate for every `ADD` gate on the path, which is often more than is necessary.
Instead, one can selectively distribute $z$ in a form such as: $(x \cdot z) + (y_1 + ... + y_k) \cdot z$.

A larger number of `MUL` gates negatively impacts the execution time of the resulting circuit; however, if the distributive rewrite is able to facilitate a future associative rewrite--and thus facilitate future depth reduction--then it is worth the few additional `MUL` gates for the depth reduction gained.

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

<!-- UNSORTED EQUATION -->

$$ 
\begin{equation}
    \label{eq:unsorted-rp-torque}
    \begin{split}
    	\tau_{1}[k]
    	&=(m_{1}l_{1}^{2}+I_{1}+I_{2})(K_{p1}+\alpha K_{v1})r_{1}[k] \\
    	&-(m_{1}l_{1}^{2}+I_{1}+I_{2})(K_{p1}+\alpha K_{v1})\theta_{1}[k] \\
    	&-(m_{1}l_{1}^{2}+I_{1}+I_{2})\alpha K_{v1}e_{1}[k-1] \\
    	&+(m_{1}l_{1}^{2}+I_{1}+I_{2})\beta K_{v1}\dot{e}_{1}[k-1] \\
    	&+m_{2}(K_{p1}+\alpha K_{v1})r_{1}[k]d_{2}^{2}[k] \\
    	&-m_{2}(K_{p1}+\alpha K_{v1})\theta_{1}[k]d_{2}^{2}[k] \\
    	&-m_{2}\alpha K_{v1}e_{1}[k-1]d_{2}^{2}[k] \\
    	&+m_{2}\beta K_{v1}\dot{e}_{1}[k-1]d_{2}^{2}[k] \\
    	&+2m_{2}d_{2}[k]\dot{\theta}_{1}[k]\dot{d}_{2}[k] \\
    	&+m_{1}l_{1}g\cos(\theta_{1}[k])  \\
    	&+m_{2}g\cos(\theta_{1}[k])d_{2}[k] 
    \end{split}
\end{equation}
$$

<!-- SORTED EQUATION -->
$$
\begin{equation}
    \label{eq:sorted-rp-torque}
    \begin{split}
       \tilde{\tau}_{1}[k] 
             &=d_2[k] (\cos(\theta_1[k]) m_2 g) \\
             &+ \cos(\theta_1[k]) (g l_1 m_1) \\
             &+ (2 d_2[k] m_2) (\dot{d}_2[k] \dot{\theta}_1[k]) \\
             &+ (K_{v1} \beta m_2) (\dot{e}_1[k-1] (d_2[k] d_2[k])) \\
             &- (K_{v1} \alpha m_2) ((d_2[k] d_2[k]) (r_1[k-1] - \theta_1[k-1])) \\
             &- (m_2 (K_{p1} + K_{v1} \alpha)) (\theta_1[k] (d_2[k] d_2[k])) \\
             &+ (m_2 (K_{p1} + K_{v1} \alpha)) (r_1[k] (d_2[k] d_2[k])) \\
             &+ (\beta (K_{v1} \dot{e}_1[k-1])) (I_2 + I_1 + l_1 (l_1 m_1)) \\
             &- (K_{v1} (r_1[k-1] - \theta_1[k-1])) (\alpha (I_2 + I_1 + l_1 (l_1 m_1))) \\  % BLUE TERM NEEDS TREE UPDATE
             &+ r_1[k] (K_{p1} + K_{v1} \alpha) (I_2 + I_1 + l_1 (l_1 m_1)) \\
             &- \theta_1[k] (K_{p1} + K_{v1} \alpha) (I_2 + I_1 + l_1 (l_1 m_1))
    \end{split}
\end{equation}
$$
