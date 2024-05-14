---
title: FPGA Key Gen
classes: wide
---

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