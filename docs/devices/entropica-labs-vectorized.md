# Vectorized - A fast QAOA simulator

The vectorized simulator is noiseless state vector simulator mostly used to prototype QAOAs. 

The vectorized simulator is designed using the well known python library [NumPy](https://numpy.org/) and it is an extremely fast for simulations up to 15 qubits. The speed advantages starts to wane off around 20, where qiskit becomes a valid option .

Due to its marked exploratory behavior, the vectorized simulator is the _default_ simulator in OpenQAOA. That is, when you create `q = QAOA()`, then the workflow is by default initialized using the vectorized device.

## Key features of the vectorized device

- Very fast up to ~18 qubits
- Preferred choice for quick prototyping
- Noiseless simulations


## Explicitly create the vectorized device

If you want to explicitly set the vectorized device use the following snippet

```Python
vect_device = create_device(location='local', name='vectorized')
q.set_device(vect_device)
```


## Rapid Protyping

The vectorized simulator is the perfect choice if you are looking to run a QAOA, accurately and quickly for small qubit counts, lesser than 15 qubits or so. Unlike shot-based simulators that are prone to shot noise, the final outputs of the vectorized simulator is said to be noiseless. 

The vectorized simulator provided within OpenQAOA utilizes optimizations to the matrix multiplication routines to achieve fast computations without much sacrifice in memory or speed. Furthermore, since hybrid algorithms like QAOA require a repetition of circuit runs, in the form of iterations, one can see how such a vectorized simulator can be useful for rapid protyping. 

## How does the Vectorized Simulator Work?

The vectorized simulator takes advantage of a couple of properties that are unique to solving problems with QAOA.

Firstly, the cost hamiltonian used in QAOAs are diagonal, since they can only consist of ZZs interaction terms. This allows us to "compress" the matrix and represent the cost hamiltonian as a 1-dimensional vector

Secondly, the simulator converts the actions of single and two-pauli rotation gates into permutations of wavefunction coefficients.

If you're interested in the exact procedure used to achieve this, you can check out the vectorized backend docstring [here](https://el-openqaoa.readthedocs.io/en/main/backends.html#openqaoa.backends.simulators.qaoa_vectorized.QAOAvectorizedBackendSimulator).

Lastly, since the operations are performed directly on the wavefunction, the memory usage of the simulator scales with $2^{n}$, where n is the number of qubits in the circuit. As compared to $2^{2n}$ if the matrices were not compressed.

### Example of Rotation about Z on the i-th Qubit

When a RZ Operation of angle, $\theta_{i}$, is performed on the i-th qubit, the simulator searches through the wavefunction for states where the i-th qubit is in the state 0 and state 1, the simulator then multiplies the wavefunction coefficients of those states by $e^{-j\frac{\theta_{i}}{2}}$ and $e^{j\frac{\theta_{i}}{2}}$ respectively.

### Example of Rotation about ZZ on the i-th and k-th Qubit

When a RZZ Operation of angle, $\theta$, is performed on the i-th and k-th qubit, the simulator searches through the wavefunction for states where the i-th and k-th qubit are in the 00 and 11 states, the simulator then multiplies the wavefunction coefficients of those states by $e^{-j{\theta}}$. After which, the simulator multiplies the entire wavefunction by $e^{j\frac{\theta}{2}}$.