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

First we created a standalone FMU for the nonlinear [*duffing oscillator*](https://en.wikipedia.org/wiki/Duffing_equation), to show that HE calculations could be successfully integrated into the FMU.
Then we created a more complex teleoperation system, consisting of three FMUs, but encryption isolated to the controller FMU alone.


## Encrypted Duffing Oscillator
The [*duffing oscillator*](https://en.wikipedia.org/wiki/Duffing_equation) is a nonlinear differential equation regarded as one of the prototypes for systems of nonlinear dynamics.
This research began by attempting to encrypt the calculations of the duffing dynamics for any arbitrary homomorphic cipher.
To achieve this the following architecture was used: a `duffing.fmu` was created to capture the system dynamics taking as input \\( x\_k \\), \\( x\_k^3 \\), \\( \dot{x}\_k \\) , \\( F = \cos(\omega t) \\) and computing \\( x\_{k+1} \\) and \\( \dot{x}\_{k+1} \\) via Euler's Method.

The dynamics calculations themselves were written against an external interface which makes calls to the shared object named `cipher.dll` on windows and `cipher.so` on linux.
The shared object contains all the cryptographic methods expected from an homomorphic cipher i.e. encrypt, decrypt, encrypted add, and encrypted multiply.

{% include figure 
    popup=true 
    image_path="/assets/fmu/fmu-duffing.svg"
    caption="All parameters in the duffing equations were encrypted and ran in FMU, where \\(F=\cos(\omega t)\\) is the forcing function, and \\(x\_k = x(k T\_s)\\)." %}

This architecture is ideal for testing the computational feasibility of a proposed encrypted dynamics calculation as it completely decouples the simulation code from the encryption code.
This allows the user to quickly change between different ciphers or security parameters in case those chosen are insufficient or result in simulation failure.

As an initial test, this research used the above encrypted FMU architecture to simulate the [phase-plane trajectory](https://en.wikipedia.org/wiki/Phase_portrait) of the duffing oscillator.
These plots are quintessential in nonlinear system dynamics and can intuitively be understood as describing how the energy flows between the different states of the system.
{% include gallery 
    id="duffing_trajectories"
    caption="Phase-Plane Trajectories of the Duffing oscillator. **From left to right: unencrypted, encrypted (pass), encrypted (fail)**. The far right simulation failed due to *homomorphic overflow* caused by poor choice of security parameters" %}

In the above phase portraits the unencrypted portrait (left) is what we take as *ground truth* and is what we expect our encrypted simulations to mimic.
While one of the encrypted portraits (middle) seems close enough, the other encrypted portrait (right) does not.
This is due to a phenomenon known as [homomorphic overflow]() which occurs when too much error is introduced into a long running calculation, causing complete loss of data.

The noise growth that leads to overflow is intrinsic in homomorphic ciphers and cannot be avoided.
While it is possible to removed the accumulated noise through an an expensive operation known as [bootstrapping](), in practice the computational cost is too high to allow for real-time computation.
Instead the main method of avoiding overflow is to choose security parameters that allow the entire computation to be computed withing the cipher's *noise budget*.

## Encrypted Teleoperation
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

## Test Bed
The entire project was created around a *mix-and-match* design philosophy, that is try to allow for as much freedom in the choice of: cipher, simulation system, security parameters, etc.

The project was centered around keeping the choice of cipher and security parameters completely disjoint from the system to be simulated.
This way a designer may work with whatever homomorphic cipher they like, the idea being that you may have to select different ciphers to get best performance depending on the control problem they are solving.

{% include figure 
    popup=true 
        image_path="/assets/fmu/testbed-staging.svg"
caption="FMU/Cipher test-bed: System simulation is constructed by linking FMUs chosen from a remote repository. A cipher is then selected to be tested for compatibility with the given FMU system." %}

Cycling through different ciphers alone is not enough to construct a working encrypted controller, once a cipher is chosen we also need to find "good" security parameters.
As we have seen in the previous examples, wrong choice of security parameters can result in complete failure of the system.

This research produced a workflow to categorize any set of security parameters for any cipher into either "good" or "bad" based on the following user specifications: \\( \varepsilon_T \\) the error tolerance, \\( \tau_T \\) the time tolerance, and a reference model.
The reference model is the ideal controller performance and can usually be the unencrypted version of the controller in question.
The error tolerance \\( \varepsilon_T \\), defined how much error is allowable between the encrypted control performance and the reference model.
Finally, \\( \tau_T \\) is the allowable computation time for the encrypted controller to compute the next control command, give the real-time requirements of control systems \\( \tau_T \\) must be very small.

With these we can define the following parameter evaluation loop:


{% include figure 
    popup=true 
    image_path="/assets/fmu/testbed-flow.svg"
    caption="Testbed Flow: Evaluation scheme to find successful security parameters, where \\( \varepsilon \\) is the measured error, \\(\varepsilon_T \\) the error tolerance, \\( \tau \\) the measured simulation-cycle time, and \\( \tau_T \\) the simulation-cycle time tolerance." %}

Using the above workflow this research was restrict the parameter space from the choice of all possible security parameters to a bifurcated *pass* and *fail* regimes.
These pass/fail maps were obtained by exhaustive enumeration of feasible security parameters for the teleoperation system described above.
To fully understand these results as well as the parameters varied (i.e. \\( \nu \\), \\( \Delta \\), \\( \lambda \\)  ) one should read the [original paper]({{page.research.publication.url}}).
Nonetheless one can still observe a sharpe nonlinearity in the "\\( \nu \\) vs \\( \Delta \\)" plot while the "\\( \nu \\) vs \\( \lambda \\)" plot trends linearly.

{% include gallery 
    id="PassFail_maps"
    caption="Pass/fail results for the teleoperation system with. The results display a bifurcation between a *pass* and *fail* regime, where failure is defined based off of \\( \tau_T \\), and \\(\varepsilon_T \\) from the above workflow." %}

Overall this research successfully constructed and demonstrated a scalable encrypted simulation/control paradigm based off of existing technologies allowing for rapid integration into the existing technology stack as well as novel testing and parameter mapping procedures.  