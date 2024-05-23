---
title: VR Teleoperation
classes: wide

research:
  institute: Georgia Institute of Technology
  PI: Dr. Jun Ueda
  date: June 2023
  funding:
    body:  National Science Foundation
    grant: "2112793"
  publication:
    title: Encrypted Coordinate Transformation via Parallelized Somewhat Homomorphic Encryption for Robotic Teleoperation
    url: https://doi.org/10.1109/AIM46323.2023.10196122

coordinate_frames:
  - image_path: /assets/vr-teleop/coordinate-frame-vr.svg
    url: /assets/vr-teleop/coordinate-frame-vr.svg
    title: "Coordinate frame of VR wand"

  - image_path: /assets/vr-teleop/coordinate-frame-arm.svg
    url: /assets/vr-teleop/coordinate-frame-arm.svg
    title: "Coordinate frame of remote robot"

timing_histograms:
  - image_path: /assets/vr-teleop/seriesTimeHistogram.svg
    url: /assets/vr-teleop/seriesTimeHistogram.svg
    title: "Computation time histogram for series method"

  - image_path: /assets/vr-teleop/parallelTimesHistogram.svg
    url: /assets/vr-teleop/parallelTimesHistogram.svg
    title: "Computation time histogram for parallel method"

tracking_results:
  - image_path: /assets/vr-teleop/tracking-performance-position-x.svg
    url: /assets/vr-teleop/tracking-performance-position-x.svg
    title: "Tracking performance $x$-direction"
  
  - image_path: /assets/vr-teleop/tracking-performance-position-y.svg
    url: /assets/vr-teleop/tracking-performance-position-y.svg
    title: "Tracking performance $y$-direction"
  
  - image_path: /assets/vr-teleop/tracking-performance-position-z.svg
    url: /assets/vr-teleop/tracking-performance-position-z.svg
    title: "Tracking performance $z$-direction"



---

{% include load-mathjax %}
{% include research-funding-disclosure-statement %}

# Research Objective
The goal of this research was to create and maintain an encrypted coordinate space which couples a VR headset to a remote robotic arm via [teleoperation](/learning/teleoperation.md).
The system will have a user wear a VR headset and input wand which will map to a remote robotic manipulator.
The headset will allow the user to visualize their environment and the wand will allow the user to command the manipulator.

The system works by constructing a virtual coordinate system which links the VR wand's movement to the movement of the arm.
This research seeks to increase the security of such a configuration by encrypting this virtual coordinate space and preforming all control calculations with respect to this encrypted space.

The high-level idea is illustrated below:

{% include figure 
popup=true 
image_path="/assets/vr-teleop/encrypted-teleop.svg"
caption="" %}

To system revolves around this virtual coordinate system which is constructed as follows.
On system startup a global origin is established in the virtual coordinate system and the robot manipulator and VR wand's initial position is mapped into this virtual coordinate space.
In practice it is often fine to map the arm and wand's initial position to the virtual origin.

{% include gallery 
    id="coordinate_frames"
    caption="The coordinate frame of the VR wand and robot arm are synced at system start-up. Any movement of the wand from its origin results in an equivalent movement of the robot arm from its origin. In this way both wand and arm are synced to a *virtual coordinate system* with the arm always following the wand." %}

An *encrypted teleoperation controller* was built to facilitate leader/follower behavior, where the controller will take action to ensure that the wand and arm maintain the same position in the virtual coordinate space.

{% include figure 
popup=true 
image_path="/assets/vr-teleop/vr-demo.gif"
caption="Video demonstration of the encrypted teleoperation control system. The robot arm tracks the VR wand in the user's hand, and the user has a virtual (simplified) view of the environment." %}

# Control Procedure

The control procedure is characterized by an initialization handshake between the remote and local machines.
During this handshake cryptographic information is exchanged and a consistent cipher context is constructed allowing encrypted data from one machine to be added/multiplied with encrypted data from the other.
After the initialization there is a control loop that runs indefinitely.

The whole control procedure is outlined in the sequence diagram below:

{% include figure 
popup=true 
image_path="/assets/vr-teleop/sequence.svg"
caption="" %}

Using this procedure our encrypted controller was able to overcome the computation burden and achieve satisfactory control objectives.
This is captured by the below reference/response plots.
In these plots $x_r$, $y_r$, $z_r$ are the computed control commands and $x_c$, $y_c$, $z_c$ is the tracking achieved by the robot arm.
{% include gallery 
    id="tracking_results"
    caption="" %}
The tracking procedure is decent but there is an obvious phase lag.
This is likely do with the limitations of the specific robot manufacture's programming interface which was not designed for real-time control.

# Experimental Conclusion

# Setup
The experimental procedure is complicated by the need to provide a repeatable yet sufficiently complex trajectory to test the tracking behavior.
A human operator is not able to provide a consistent enough movement for repeated experiments and constructing a mechanical rig would not provide complex enough movement.
To resolve this trajectory issue a the wand was attached to a Universal Robot 5 (UR5) which was programmed to produce a consistent experimentation path.

The experimental setup is outlined by the following figure:
{% include figure 
popup=true 
image_path="/assets/vr-teleop/experimental-setup.png"
caption="" %}

Homomorphically encrypted calculations are always plagued by painfully slow computation, which if not resolved make any real-time control impossible.
Additionally, it is well observed that stronger cipher security parameters lead to slower computation, thus if we want our encrypted controller to be secure we must overcome this additional burden.

# Parallelization
Thankfully there are many ways to parallelize homomorphic calculations such as [Karatsuba Multiplication](https://mathworld.wolfram.com/KaratsubaMultiplication.html) or the [Number Theoretic Transform](https://mathworld.wolfram.com/NumberTheoreticTransform.html).
With parallelization we can use larger keys and mitigate the increased computation cost of using larger more secure keys.

Since refresh rate is one of the main hurdles to realizing *encrypted controllers* this research analyzed the refresh rate achievable for increasingly larger cipher keys, the results of which are in the histograms below.

{% include gallery 
    id="timing_histograms"
    caption="" %}

In the above figure you will see that without parallelization the reported refresh frequency make distinct jumps with each key, however with parallelization the difference in refresh rates is not as distinct.
This indicates that parallelization is an effective measure to alleviate computation burden imposed by large keys.

The parallelization of this research realize heavily on maintain a large [thread pool](https://en.wikipedia.org/wiki/Thread_pool) to distribute the workload.
However their is an overhead to managing a large collection of threads and thus for small enough keys there is a crossover point where threading is no longer helpful.
This will be system dependant but the behavior should look similar to the research result below:
{% include figure 
popup=true 
image_path="/assets/vr-teleop/lambda-time.svg"
caption="" %}

# Conclusion

Overall this research was able to successfully couple a VR headset to a remote robotic manipulator via an encrypted teleoperation controller.
The controller was able to achieve real-time performance without leaking intermediate calculations or results.