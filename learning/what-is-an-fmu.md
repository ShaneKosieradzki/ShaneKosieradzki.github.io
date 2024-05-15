---
title: "What is an FMU?"
---

The [Functional Mock-up Interface (FMI)](https://fmi-standard.org/) is a simulation standard for modeling complex *cyber-physical systems* (CPS).
The FMI links together **Function Mock-up Units (FMUs)**, each representing a distinct system component.

For example, to model a car's tire + suspension system you could construct `suspension.fmu` and link it with `tire.fmu`.
By linking these two components, changes in the state of one system is immediately transferred to the other system through appropriate coupling equations.

Thus with FMU we are able to build digital simulations by composing FMUs together in much the same that we build physical products by composing parts together.