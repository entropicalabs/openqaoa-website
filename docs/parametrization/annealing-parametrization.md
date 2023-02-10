# Annealing Parametrization

The `QAOAVariationalAnnealingParams` class implements a discretised form of quantum annealing, with a schedule function  𝑠(𝑡); the coefficient of the mixer Hamiltonian is (1−𝑠(𝑡))
 , and the coefficient of the cost Hamiltonian is  𝑠(𝑡)

!!! note "Main difference"
    Unlike the other parametrizations for QAOA, in the annealing one the coefficients of the two Hamiltonians (mixer and cost) are necessarily related to one another.



!!! warning "The 'total_annealing_time' must be specified."

!!! warning "Custom initialization is not supported for this class and will result in an error."