# Contributing to OpenQAOA - Challenges  
This section of the website contains a set of feature questions that we encourage our community to work on.

Some of them are software-related, others more physics-oriented, while others have to do with benchmarks and documentation.

We believe some of these challenges may be very interesting for any young quantum scientist out there who is interested in growing some hands-on experience with quantum software! 

## Software
 - **Cirq Backend**: currently, OpenQAOA does not support Cirq! This is a highly software-based task, but the good news is that most of the code layout is already prepared (see, for inspiration, the AWS Braket backend)
 - **Error mitigation - Mitiq** Can we make OpenQAOA compatible with MitiQ?
 - **NVIDIA backend** Can we add some of the nvidia simulators to the OpenQAOA stack?
 - **TensorFlow Quantum** Add tensorflow quantum as a valid simulator to OpenQAOA
 - **Make OpenQAOA compatible with Qiskit Runtime** Qiskit runtime is an amazing tool to streamline and improve the execution of variational quantum algorithm. Can we make OpenQAOA compatible with it?
 - **Make OpenQAOA available through Qbraid** [Qbraid](https://qbraid.com/) is a fully hosted ecosystem for computing and it would be very nice to have OpenQAOA within it.
 

## Quantum Computing & Research
 - **Optimizers and Noise**: Investigate the connection between optimisers and noise. For example, if we assume only depolarising noise is present, does one type of classical optimiser prevail over others?
 - **Error Mitigation**: Is a particular error mitigation technique well suited to QAOA? 
 - **Mixers and Initialisation**: Find a good mixer and initialisation strategy for the 1D knapsack problem.
 - **Midcircuit measurements and Mixers**: Come up with an example of using mid-circuit measurements within a QAOA mixer for a constrained optimisation problem of your choice.
 - **Global-Local optimizers**: Given a budget of optimization steps, the optimizer uses a fraction to perform a global and broad optimization and the remaining fraction to perform a narrow, local optimization.
 - **Adaptive QAOA** Implement adaptive QAOA as specified in the paper by [Zhu et al.](https://journals.aps.org/prresearch/abstract/10.1103/PhysRevResearch.4.033029)
 - **Altenarive Encoding and Qubit Reuse** Can we design a workflow to construct a QAOA circuit on hardware of $N$-qubits, but for a problem size $>N$, through the use of methods like alternative encoding, and qubit-reuse.

## Benchmarking & Documentation
 - **CVAR and QAOA**: Take a problem class (say MaxCut) and assess the improvement on ordinary QAOA when tweaking the CVAR value


