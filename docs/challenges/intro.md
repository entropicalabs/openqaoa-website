# Contributing to OpenQAOA - Challenges
This section of the website contains a set of feature questions that we encourage our community to work on.

Some of them are software-related, others more physics-oriented, while others have to do with benchmarks and documentation.

We believe some of these challenges may be very interesting for any young quantum scientist out there who is interested in growing some hands-on experience with quantum software! 

## Software
 - **Cirq Backend**: Currently, OpenQAOA does not support [Cirq](https://quantumai.google/cirq)! This is a highly software-based task, but the good news is that most of the code layout is already prepared (see, for inspiration, the [AWS Braket backend](https://github.com/aws-samples/aws-dev-hour-backend))
 - **Error Mitigation with Mitiq**: Can we make OpenQAOA compatible with [Mitiq](https://unitary.fund/mitiq.html)?
 - **NVIDIA Backend**: Can we add some of the [NVIDIA](https://docs.nvidia.com/cuda/cuquantum/) simulators to the OpenQAOA stack?
 - **TensorFlow Quantum Simulator**: Add [TensorFlow Quantum](https://www.tensorflow.org/quantum) as a valid simulator to OpenQAOA
 - **Make OpenQAOA compatible with Qiskit Runtime**: [Qiskit Runtime](https://qiskit.org/documentation/partners/qiskit_ibm_runtime/) is an amazing tool to streamline and improve the execution of variational quantum algorithms. Can we make OpenQAOA compatible with it?
 - **Make OpenQAOA available through qBraid**: [qBraid](https://qbraid.com/) is a fully hosted ecosystem for computing and it would be very nice to have OpenQAOA available to simplify the execution of QAOA computations and enhance QAOA research!
 

## Quantum Computing & Research
 - **Optimisers and Noise**: Investigate the connection between optimisers and noise. For example, if we assume only depolarising noise is present, does one type of classical optimiser prevail over others?
 - **Error Mitigation**: Is a particular error mitigation technique well-suited to QAOA? 
 - **Mixers and Initialisation**: Find a good mixer and initialisation strategy for the 1D knapsack problem.
 - **Mid-Circuit Measurements and Mixers**: Come up with an example of using mid-circuit measurements within a QAOA mixer for a constrained optimisation problem of your choice.
 - **Global-Local Optimisers**: Given a budget of optimization steps, the optimiser uses a fraction to perform a global and broad optimisation and the remaining fraction to perform a narrow, local optimisation.
 - **Adaptive QAOA**: Implement adaptive QAOA as specified in the paper by [Zhu et al.](https://journals.aps.org/prresearch/abstract/10.1103/PhysRevResearch.4.033029)
 - **Alternative Encoding and Qubit Reuse**: Can we design a workflow to construct a QAOA circuit on hardware of $N$-qubits, but for a problem size $>N$, through the use of methods like alternative encoding, and qubit-reuse?

## Benchmarking & Documentation
 - **CVAR and QAOA**: Take a problem class (say MaxCut) and assess the improvement on ordinary QAOA when tweaking the CVAR value


