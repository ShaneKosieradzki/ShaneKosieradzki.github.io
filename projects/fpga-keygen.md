---
title: FPGA Key Gen
classes: wide

research:
  institute: Georgia Institute of Technology
  PI: Dr. Jun Ueda
  date: January 2024
  funding:
    body:  National Science Foundation
    grant: "2112793"
  publication:
    title: Encrypted Sensor and Actuator Interface for Encrypted Control Signals via Embedded FPGA Key Generation
    url: https://doi.org/10.1109/SII58957.2024.10417179

---

{% include research-funding-disclosure-statement %}

Homomorphic ciphers are instantiated with a set of security parameters.
Beyond security these parameters also affect the performance of an encrypted calculation, with stronger parameters resulting in slower computations.
This presents a design challenge for *encrypted controllers* as a designer must balance security against controller refresh speed.

A high refresh rate is a must for a controller to operate, thus an encrypted control system tends to prefer small keys to allow fast encrypted calculations.
However, small keys are more vulnerable to cryptographic attack as compared to large keys.

This research proposes a solution to the above problem.
If we can periodically switch the small key, at a rate faster than any attacker could break the current small key, then the system would gain cryptographic security comparable to a large key.

{% include figure 
popup=true 
image_path="/assets/fpga-keygen/single-large-key-vs-many-small-keys.png"
caption="" %}

One of the challenges of using this *many-small-key* approach is that generating cryptographic keys is itself a burdensome computation.
To overcome the computational burden the research produced an FPGA based design with the following architecture.
First, each sensor/actuator in the system has an `Enc`/`Dec` circuit attached directly to its output/input, reducing the time the signal spends unencrypted to increase security.
Additionally there is a `KeyGen` circuit which periodically generates new keys and broadcast the keys to the `Enc`/`Dec` circuits, who then update themselves with the new key.

This design is depicted in the figure below:

{% include figure 
    popup=true 
    image_path="/assets/fpga-keygen/system-overview.svg"
    caption="Autonomous system employing dedicated cryptographic circuits with periodic key re-generation. By embedding an `Enc` circuit into the sensor node, the system can conceal the sensor signal from the cloud. The embedded `Dec` circuit in the actuator node then retrieves the control signal calculated by the cloud. The `KeyGen` circuit creates a new *active key* periodically replacing the key used by the other circuits." %}

The encrypted signal is sent to the encrypted controller as input, which produces an encrypted output per a typical encrypted controller setup.


## Implementation
The exact key implementation details will depend on the cipher chosen, since this research considered an integer based cipher prime generation will be the primary problem to overcome.



{% include figure 
popup=true 
image_path="/assets/fpga-keygen/LFSR.png"
caption="Linear Feedback Shift Register" %}


## Results

{% include figure 
    popup=true 
    image_path="/assets/fpga-keygen/exp-setup.png"
    caption="FPGA board setup" %}


{% include figure 
popup=true 
image_path="/assets/fpga-keygen/results.png"
caption="" %}