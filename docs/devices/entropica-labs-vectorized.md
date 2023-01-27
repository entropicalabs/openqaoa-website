# Vectorized - A fast QAOA simulator

The vectorized simulator is noiseless state vector simulator mostly used to prototype QAOAs. 

The vectorized simulator is designed using the well known python library [NumPy](https://numpy.org/) and it is an extremely fast for simulations up to 15 qubits. The speed advantages starts to wane off around 20, where qiskit becomes a valid option .

Due to its marked exploratory behavior, the vectorized simulator is the _default_ simulator in OpenQAOA. That is, when you create `q = QAOA()`, then the workflow is by default initialized using the vectorized device.

## Key features of the vectorized device

- Very fast up to 15 qubits
- Preferred choice for quick prototyping
- Noiseless simulations


## Explicitly create the vectorized device

If you want to explicitly set the vectorized device use the following snipped

```Python
qiskit_sv = create_device(location='local', name='vectorized')
q.set_device(qiskit_sv)
```

