# Local Qiskit simulators

This set of simulators encompasses simulators available through the well known python SDK [Qiskit](https://qiskit.org/), developed by IBM

## Key features of the vectorized backend

- Simulations are run locally
- Supports: 
    - `qiskit.qasm_simulator`
    - `qiskit.shot_simulator`
    - `qiskit.statevector_simulator`

## Explicitly create the qiskit device

If you want to explicitly set the vectorized device use the following snipped

```Python
q = QAOA()

qiskit_sv = create_device(location='local', name='qiskit.statevector_simulator')
q.set_device(qiskit_sv)
```




