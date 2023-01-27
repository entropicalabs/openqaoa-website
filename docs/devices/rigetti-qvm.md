# Rigetti Quantum Virtual Machine

The Rigetti Quantum Virtual Machine (QVM) is a flexible and efficient simulator for Quil.


From the [official documentation](https://pyquil-docs.rigetti.com/en/stable/qvm.html): _The QVM simulates the unitary evolution of a wavefunction with classical control. The QVM has a plethora of other features, including:_

- _Stochastic pure-state evolution, density matrix evolution, and Pauli noise channels;_
- _Shared memory access to the quantum state, allowing direct NumPy access to the state without copying or transmission delay; and_
- _A fast just-in-time compilation mode for rapid simulation of large programs with many qubits._


## Installation and Getting Started

In order to use the QVM backend, you will either need to connect [Rigetti Quantum Cloud Service](rigetti-qcs.md), or start the QVM locally.

Instructions on how to install the QVM and quilc vary from operative system. Please, refer to the [official documentation](https://pyquil-docs.rigetti.com/en/stable/start.html#downloading-the-qvm-and-compiler).


## Explicitly create the QVM device

In order to instantiate a QVM backend, we need to run the following snipped of code

```Python
q_qvm = QAOA()

rigetti_args ={
    'as_qvm':True,
    'execution_timeout':10,
    'compiler_timeout':10
}

n_qubits = np_qubo.n
rigetti_device = create_device(location='qcs', name=f'{n_qubits}q-qvm', **rigetti_args)

q_qvm.set_device(rigetti_device)
```

First, this device requires extra arguments. The source code can be viewed [here](https://github.com/entropicalabs/openqaoa/blob/dev/openqaoa/devices.py#L278)

```Python
rigetti_args ={
    as_qvm:True,
    noisy: bool = None,
    compiler_timeout: float = 20.0, # In seconds
    execution_timeout: float = 20.0, # In seconds
    client_configuration: QCSClientConfiguration = None,
    endpoint_id: str = None,
    engagement_manager: EngagementManager = None
}
```

For most basic usage you will only need to specify the timeouts (to cut the computation short, in case it takes too long), and set `as_qvm` to True. It is an optional flag to force construction of a QVM (instead of a QPU). 

!!! Note
    The QVM `5q-qvm` can simulate up to 5 qubits. The QVM `Nq-qvm` will simulate up to `N` qubits. Be careful! 