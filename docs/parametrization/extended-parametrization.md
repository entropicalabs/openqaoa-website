# Extended parametrization

The `ExtendedParams` class implements the most general form of the QAOA where each operator in both the cost and mixer Hamiltonians has its own angle.
For example, for a depth-1 (p=1) circuit the unitary operator corresponding to the QAOA circuit would now take the form:

$e^{i\sum_{j}\beta_{j}^{(1)}X_j}e^{-i\sum_{j\in s} \gamma_{j}^{(1)}h_{j}Z_j} e^{-(i/2)\sum_{j,k \in \Pi}\Gamma_{jk}^{(1)}g_{jk}Z_jZ_k}$

!!! note "Number of parameters"
    The number of parameters will depend on the number of edges and biases. In the worst case for a fully connected graph with biases on each node, the optimizer will have to find $(n(n-1) + n + n) p$ parameters. 

Let us look again at the simple example of a classical cost hamiltonian on 3 qubits which we used to explain the standard parametrization:

$H_C = 2.5 (Z_0Z_1 + Z_1Z_2 + Z_0Z_2) + 3.5 (Z_0 + Z_1 + Z_2)$.

This time, however, the extended parametrization allows us to specify a different angle for every gate with the following:

```Python
q.set_circuit_properties(p=1, 
                         param_type='extended', 
                         init_type='custom', 
                         variational_params_dict={
                             'gammas_pairs':[1.1, 1.2, 1.3], 
                             'gammas_singles':[2.1, 2.2, 2.3], 
                             'betas_pairs': [], 
                             'betas_singles': [3.1, 3.2, 3.3]}
                         )
```

As a circuit, this `custom` initialization looks like:

![kamada_kawai_layout](/img/circuit_extended.png)

The rotation angles are still obtained by multiplying the weights with the gamma pair or single and the biases with the beta single. In particular, rotating the second qubit around the Z axis at an angle of 15.4 comes from multiplying the bias of the second qubit, 3.5 in this case, with the second initial parameter in the gammas_singles as specified above, 2.2: 15.4 = 3.5 * 2.2 * 2 where the last 2 is from the convention on how we write gates and angles. 

!!! note "When do we need `beta_pairs`?"
    `betas_pairs` will be relevant only for 2-qubit mixer hamiltonians, for example the XY mixer as implemented by the `XY_mixer_hamiltonian` in openqaoa-core/utilities.py. If not required, it can be omitted when specifying the initial values altogether.

Overall, this is the most versatile ansatz one can implement while still obeying the fundamental QAOA gate structure. However, this is also the hardest to optimize due to the larger number of variational parameters. 

!!! info "For the curious"
    It is important to realize that the gate angles can be different also without using the extended parametrization if the weights and biases of the Hamiltonian are different for each term. What the extended parametrization allows is varying each parameter during the classical optimization procedure. 
