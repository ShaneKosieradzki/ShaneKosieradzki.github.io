---
title: FPGA Key Gen
classes: wide
---

<!-- FIGURE: Key Generation + Sensor + Actuator -->
{% assign PATH__system_overview =  site.data.asset_paths.fpga | append: '/' | append: 'system-overview.svg' %}
{% include figure popup=true image_path=PATH__system_overview
caption="Autonomous system employing dedicated cryptographic circuits with periodic key re-generation. By embedding a `Enc` circuit into the sensor node, the system can conceal the sensor signal from the cloud. The embedded `Dec` circuit in the actuator node then retrieves the control signal calculated by the cloud. The `KeyGen` circuit creates a new *active key* periodically replacing the key used by the other circuits." %}

<!-- FIGURE: Experimental setup (board + wires) -->
{% assign PATH__exp_setup =  site.data.asset_paths.fpga | append: '/' | append: 'exp-setup.png' %}
{% include figure popup=true image_path=PATH__exp_setup
caption="FPGA board setup" %}

<!-- FIGURE: Linear Feedback Shift Register (LFSR) -->
{% assign PATH__LFSR =  site.data.asset_paths.fpga | append: '/' | append: 'LFSR.png' %}
{% include figure popup=true image_path=PATH__LFSR
caption="Linear Feedback Shift Register" %}

<!-- FIGURE: One Big Key vs Many Small Keys -->
{% assign PATH__LFSR =  site.data.asset_paths.fpga | append: '/' | append: 'Motivation.png' %}
{% include figure popup=true image_path=PATH__LFSR
caption="" %}

<!-- FIGURE: CPU vs FPGA result -->
{% assign PATH__results =  site.data.asset_paths.fpga | append: '/' | append: 'results.png' %}
{% include figure popup=true image_path=PATH__results
caption="" %}