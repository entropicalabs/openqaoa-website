# Contributing to OpenQAOA - Challenges 
This section of the website contains a set of feature questions that we encourage our community to work on.

Some of them are software-related, others more physics-oriented, while others have to do with benchmarks and documentation.

We believe some of these challenges may be very interesting for any young quantum scientist out there who is interested in growing some hands-on experience with quantum software! 

## Software
**Cirq Backend**: currently, OpenQAOA does not support Cirq! This is a highly software-based task, but the good news is that most of the code layout is already prepared (see, for inspiration, the AWS Braket backend)
**Error mitigation - Mitiq** Can we make OpenQAOA compatible with MitiQ?

## Quantum Computing & Research
**Optimizers and Noise**: Investigate the connection between optimisers and noise. For example, if we assume only depolarising noise is present, does one type of classical optimiser prevail over others?
**Error Mitigation**: Is a particular error mitigation technique well suited to QAOA? 
**Mixers and Initialisation**: Find a good mixer and initialisation strategy for the 1D knapsack problem.
**Midcircuit measurements and Mixers**: Come up with an example of using mid-circuit measurements within a QAOA mixer for a constrained optimisation problem of your choice.

## Benchmarking & Documentation
**CVAR and QAOA**: Take a problem class (say MaxCut) and assess the improvement on ordinary QAOA when tweaking the CVAR value


