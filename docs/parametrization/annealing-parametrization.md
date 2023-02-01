# Annealing Parametrization

The `QAOAVariationalAnnealingParams` class implements a discretised form of quantum annealing, with a schedule function  𝑠(𝑡)
 ; the coefficient of the mixer Hamiltonian is  (1−𝑠(𝑡))
 , and the coefficient of the cost Hamiltonian is  𝑠(𝑡)
 . Unlike QAOA, therefore, the coefficients of the two Hamiltonians are necessarily related to one another.

**NOTE**: The 'total_annealing_time' must be specified.