---
title: FPGA Key Gen
sidebar:
  - title: Test Side Bar
    image: \assets\personal-photo.jpg
    image_alt: "image"
    text: "some text"
---


{% include figure popup=true image_path="/assets/fpga-keygen/system-overview.svg" 
caption="Autonomous system employing dedicated cryptographic circuits with periodic key re-generation. By embedding a `Enc` circuit into the sensor node, the system can conceal the sensor signal from the cloud. The embedded Dec circuit in the actuator node then retrieves the control signal calculated by the cloud. The KeyGen circuit creates a new *active key* periodically replacing the key used by the other circuits." %}




<!-- ![image-left](/assets/fpga-keygen/system-overview.png){: .align-left} -->



<!-- <object data="/assets/fpga-keygen/system-overview.svg" type="image/svg+xml" width=50%> -->
  <!-- <img src="/path-to/your-fallback-image.png" /> -->
<!-- </object> -->

<!-- <img src="/assets/fpga-keygen/system-overview.svg" width=50%/> -->

<!-- <picture>  -->
<!-- <source srcset="/assets/fpga-keygen/system-overview.svg" media="(min-width: 600px)">  -->
<!-- <img src="/assets/fpga-keygen/system-overview.svg" alt="landing image" height=“600” width=“600” loading=“lazy” decoding=“async”>  -->
<!-- </picture> -->