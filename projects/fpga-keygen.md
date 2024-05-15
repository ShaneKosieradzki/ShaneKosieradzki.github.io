---
title: FPGA Key Gen
classes: wide

abstract: "This paper presents an investigation into the improvement of security and operation time in homomorphically encrypted systems using Field Programmable Gate Array (FPGA) technology. The primary objective is to generate keys efficiently, minimizing key sizes while maintaining security. By leveraging FPGA capabilities for key generation and key switching, smaller ciphertext sizes can be achieved, ultimately improving operation time. The paper focuses on the development of a sensor data encryption system implemented on an FPGA board. The proposed approach enables simultaneous key generation and encryption of incoming sensor data using generated keys. The developed system implemented fixed-size random number generation and prime number checking in hardware, subsequently expanding these capabilities to produce arbitrarily sized prime numbers."
---

{{page.abstract}}

<!-- FIGURE: Key Generation + Sensor + Actuator -->
{% include figure 
    popup=true 
    image_path="/assets/fpga-keygen/system-overview.svg"
    caption="Autonomous system employing dedicated cryptographic circuits with periodic key re-generation. By embedding a `Enc` circuit into the sensor node, the system can conceal the sensor signal from the cloud. The embedded `Dec` circuit in the actuator node then retrieves the control signal calculated by the cloud. The `KeyGen` circuit creates a new *active key* periodically replacing the key used by the other circuits." %}

<!-- FIGURE: Experimental setup (board + wires) -->
{% include figure 
    popup=true 
    image_path="/assets/fpga-keygen/exp-setup.png"
    caption="FPGA board setup" %}

<!-- FIGURE: Linear Feedback Shift Register (LFSR) -->
{% include figure 
popup=true 
image_path="/assets/fpga-keygen/LFSR.png"
caption="Linear Feedback Shift Register" %}

<!-- FIGURE: One Big Key vs Many Small Keys -->
{% include figure 
popup=true 
image_path="/assets/fpga-keygen/Motivation.png"
caption="" %}

<!-- FIGURE: CPU vs FPGA result -->
{% include figure 
popup=true 
image_path="/assets/fpga-keygen/results.png"
caption="" %}