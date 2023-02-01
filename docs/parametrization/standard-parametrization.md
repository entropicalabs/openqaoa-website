# Standard parametrization

The `StandardParams` class implements the original and conventional form of the QAOA. In every layer of the circuit, the mixer and cost Hamiltonians are applied with coefficients $\beta(q)$ and $\gamma(q)$, respectively. That gives a total of 2p
 parameters for a circuit of p layers to be optimised. For example, for a depth-2 (p=2) circuit, the unitary operator corresponding to the QAOA circuit would take the form

$U(\beta_1,\beta_2,\gamma_1,\gamma_2) = e^{-i\beta^{(2)} H_{M}} e^{-i\gamma^{(2)} H_{C}} e^{-i\beta^{(1)} H_{M}} e^{-i\gamma^{(1)} H_{C}}$.



The `StandardWithBiasParams` class 