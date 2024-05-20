---
title: Dyer's vs BFV
classes: wide

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

cipher_timing:
  - image_path: /assets/dyers-vs-bfg/operation-time-bfg.png
    url: /assets/dyers-vs-bfg/operation-time-bfg.png
    title: "Operation times for the BFG cipher."

  - image_path: /assets/dyers-vs-bfg/operation-time-dyers.png
    url: /assets/dyers-vs-bfg/operation-time-dyers.png
    title: "Operation times for the Dyer's cipher."

---

{% include load-mathjax %}
{% include research-funding-disclosure-statement %}

## Teleoperation

{% include figure 
    popup=true 
    image_path="/assets/dyers-vs-bfg/teleoperation-operator-manipulator.svg"
    caption="" %}

{% include figure 
    popup=true 
    image_path="/assets/dyers-vs-bfg/virtual-spring-diagram2.svg"
    caption="" %}


## Encrypted Controller
{% include figure 
    popup=true 
    image_path="/assets/dyers-vs-bfg/encrypted-controller-cartoon-proposal.svg"
    caption="" %}

### PHE Controller

{% include figure 
    popup=true 
    image_path="/assets/dyers-vs-bfg/encrypted-controller-cartoon-phe.svg"
    caption="" %}


{% include figure 
    popup=true 
    image_path="/assets/dyers-vs-bfg/encrypted-controller-schematic-phe.svg"
    caption="" %}



### SHE Controller
{% include figure 
    popup=true 
    image_path="/assets/dyers-vs-bfg/encrypted-controller-cartoon-she.svg"
    caption="" %}

{% include figure 
    popup=true 
    image_path="/assets/dyers-vs-bfg/encrypted-controller-schematic-she.svg"
    caption="" %}

## Timing Results
{% include gallery 
    id="cipher_timing"
    caption="" %}