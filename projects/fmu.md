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

teleop_trajectories:
  - image_path: /assets/fmu/traj-tele-unenc.svg
    title: "Teleoperation Unencrypted Trajectory"

  - image_path: /assets/fmu/traj-tele-enc.svg
    title: "Teleoperation Encrypted Trajectory (Pass)"

  - image_path: /assets/fmu/traj-tele-fail.svg
    title: "Teleoperation Encrypted Trajectory (Fail)"

PassFail_maps:
    - image_path: /assets/fmu/PassFail-delta-nu.svg
      title: Security Parameter vs Encoding Parameter

    - image_path: /assets/fmu/PassFail-lambda-nu.svg
      title: Security Parameter vs Security Parameter

abstract: "The research presented in this paper aims to establish functional mockup units (FMU) co-simulation methods to simulate and evaluate encrypted dynamic systems using somewhat homomorphic encryption (SHE). The proposed approach encrypts the entire dynamic system expressions, including: model parameters, state variables, feedback gains, and sensor signals, and perform computation in the ciphertext space to simulate dynamic behaviors or generate motion commands to servo systems. The developed FMU co-simulation helps analyze the relationship between security parameters and performance. Two illustrative examples are presented and analyzed: 1) encrypted Duffing oscillator and 2) encrypted teleoperation. How the time delay due to FMU co-simulation affects the refresh rate is also reported."
---

{{page.abstract}}

# Encrypted Duffing Oscillator
{% include figure 
    popup=true 
    image_path="/assets/fmu/fmu-duffing.svg"
    caption="" %}

{% include gallery 
    id="duffing_trajectories"
    caption="This is a sample gallery with **Markdown support**." %}

# Encrypted Teleoperation
{% include figure 
    popup=true 
    image_path="/assets/fmu/fmu-teleop.svg"
    caption="" %}

{% include gallery 
    id="teleop_trajectories"
    caption="This is a sample gallery with **Markdown support**." %}

# Test Bed
{% include figure 
    popup=true 
        image_path="/assets/fmu/testbed-staging.svg"
caption="" %}

{% include figure 
    popup=true 
    image_path="/assets/fmu/testbed-flow.svg"
    caption="" %}

# Pass/Fail Maps
{% include gallery 
    id="PassFail_maps"
    caption="This is a sample gallery with **Markdown support**." %}

