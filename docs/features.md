# Features
Key features of OpenQAOA:

* Simple yet customizable workflows for QAOA and RQAOA are deployable on
    * IBMQ devices
    * Rigettis' Quantum Cloud Services
    * Amazon Braket
    * Microsoft's Azure
    * Local simulators (including Entropica Labs' vectorized simulator)
* Multiple parametrization strategies:
    * `Standard`, `Fourier`, and `Annealing`
    * Each class can be further controlled by selecting standard or extended parameter configurations
* Multiple _initialisation strategies_:
    * `Linear ramp`, `random`, and `custom`
* Multiple _mixer Hamiltonians_:
     * `x`, `xy`, and `custom`
* The optimization loop includes:
    * SciPy optimizers
    * Custom gradient SciPy optimizers
    * PennyLane optimizers