---
title: Encrypted FMUs
classes: wide

research:
  institute: Georgia Institute of Technology
  PI: Dr. Jun Ueda
  date: "2021"
  grant: National Science Foundation Grant Nos. CMMI 2112793 and ERC 2124319
  publication:
    title:
    url:

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

{% include load-mathjax %}

This research was conducted at the {{page.research.institute}} under the supervision of {{page.research.PI}} in {{page.research.date}} resulting in the following publication: [{{page.research.publication.title}}]({{page.research.publication.url}})

The goal of this project was to create a simulation testbed on which to evaluate the effectiveness of different [homomorphic ciphers](/learning/what-is-a-homomorphic-cipher.md) in realtime encrypted control scenarios.
To do this the encapsulation provided by FMUs was leveraged to create a simulation environment isolating encrypted calculations to the controller, exactly how a real encrypted cyber-physical system would be organized.

First we created a standalone FMU for the nonlinear [*duffing oscillator*](), to show that HE calculations could be successfully integrated into the FMU.
Then we created a more complex teleoperation system, consisting of three FMUs, but encryption isolated to the controller FMU alone.


### Encrypted Duffing Oscillator
{% include figure 
    popup=true 
    image_path="/assets/fmu/fmu-duffing.svg"
    caption="All parameters in the duffing equations were encrypted and ran in FMU, where \\(F=\cos(\omega t)\\) is the forcing function, and \\(x_k = x(k T_s)\\)." %}

{% include gallery 
    id="duffing_trajectories"
    caption="This is a sample gallery with **Markdown support**." %}

### Encrypted Teleoperation
In the following configuration the `local.fmu` and `remote.fmu` represent a controller and manipulator in a [teleoperation configuration](/learning/teleoperation.md) and will be refered to as the *plants*.
State information is collected from both plants and sent to the controller, which encrypts the inputs, computes control commands in cipher-space, and then decrypts and broadcasts the commands back to the plants.

{% include figure 
    popup=true 
    image_path="/assets/fmu/fmu-teleop.svg"
    caption="The teleoperation system consists of three separate FMU: local plant, controller, and remote plant. The controller makes calls to `cypher.dll` to perform cryptographic operations." %}

Using this simulation setup we can judge how a set of cipher security parameters will impact the performance of the control system.
Choosing incorrect parameters can introduce additional error, or in the worst case, cryptographic overflow resulting in complete loss of data.

As part of this work a tracking experiment was carried out, where the leader \\( x_m \\) follows a *step input*, meant to model a sudden rapid sudden change.
The controller must compensate by sending commands to the follower \\( x_s \\) so that the follower tracks the leader's position.
The results of these experiments are shown below:


{% include gallery 
    id="teleop_trajectories"
    caption="Tracking performance of the FMU teleoperation simulation for a step input. Ideally the differeance between \\( x_m \\) and \\( x_s \\) is as small as possible. **From left to right: unencrypted, encrypted (pass), encrypted (fail)**." %}

In the above figures, the unencrypted case (left) is the performance we hope our encrypted controllers can emulate.
We can see that one of the encrypted controllers (middle) is a close approximation to unencrypted case, but why didn't the other encrypted controller (right) produce the expected behavior?

The answer is due to poor choice of security parameters, which resulted in bad cipher behavior. 
The details of what constitute "good" parameters can be complex and will depend on your goals,
 making it difficult to state any universal rule or strategy for their selection.
However parameters the result in a failure to complete the control objective, like the above example (right), are clearly "bad" parameters.
This motivates the need for the test-bed procedure outlined in the following section.

### Test Bed
{% include figure 
    popup=true 
        image_path="/assets/fmu/testbed-staging.svg"
caption="" %}

{% include figure 
    popup=true 
    image_path="/assets/fmu/testbed-flow.svg"
    caption="" %}

### Pass/Fail Maps
{% include gallery 
    id="PassFail_maps"
    caption="This is a sample gallery with **Markdown support**." %}

