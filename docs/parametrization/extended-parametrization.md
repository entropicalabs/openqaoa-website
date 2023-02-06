# Extended parametrization

The `ExtendedParams` class implements the most general form of the QAOA where each operator in both the cost and mixer Hamiltonians has its own angle.
For example, for a depth-1 (p=1) circuit the unitary operator corresponding to the QAOA circuit would now take the form:

$e^{i\sum_{j}\beta_{j}^{(1)}X_j}e^{-i\sum_{j\in s} \gamma_{j}^{(1)}h_{j}Z_j} e^{-(i/2)\sum_{j,k \in \Pi}\Gamma_{jk}^{(1)}g_{jk}Z_jZ_k}$


**NOTE**: The number of parameters will depend on the number of edges and biases. In the worst case, the optimizer will have to optimize $(n(n-1) + n + n) p$ parameters. 

