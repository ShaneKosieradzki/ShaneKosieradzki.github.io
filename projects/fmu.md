---
title: Encrypted FMUs
classes: wide
duffing_trajectories:
  - image_path: /assets/fmu/traj-duff-unenc.svg
    title: "Duffing Unencrypted Trajectory"

  - image_path: /assets/fmu/traj-duff-enc.svg
    title: "Duffing Encrypted Trajectory (Pass)"

  - image_path: /assets/fmu/traj-duff-fail.svg
    title: "Duffing Encrypted Trajectory (Fail)"

PassFail_maps:
    - image_path: /assets/fmu/PassFail-delta-nu.svg
      title: Security Parameter vs Encoding Parameter

    - image_path: /assets/fmu/PassFail-lambda-nu.svg
      title: Security Parameter vs Security Parameter


---


<!-- FIGURE:  -->
{% include figure 
    popup=true 
    image_path="/assets/fmu/fmu-duffing.svg"
    caption="" %}

<!-- FIGURE:  -->
{% include figure 
    popup=true 
    image_path="/assets/fmu/fmu-teleop.svg"
    caption="" %}

<!-- FIGURE:  -->
{% include figure 
    popup=true 
        image_path="/assets/fmu/testbed-staging.svg"
caption="" %}

<!-- FIGURE:  -->
{% include figure 
    popup=true 
    image_path="/assets/fmu/testbed-flow.svg"
    caption="" %}

<!-- Gallery:  -->
{% include gallery 
    id="duffing_trajectories"
    caption="This is a sample gallery with **Markdown support**." %}

<!-- Gallery:  -->
{% include gallery 
    id="PassFail_maps"
    caption="This is a sample gallery with **Markdown support**." %}

