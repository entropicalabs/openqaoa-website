# Customise the QAOA workflow

In this page, you will learn how to customize OpenQAOA workflows.

!!! tip
    As a rule of thumbs, customizing often times makes the quantum circuit more complex. This in turn, may lead to either longer execution times, or even worst performances! Many of the tools we will explore here

The simplest QAOA workflow implicit uses the default values


```Python
from openqaoa import QAOA  

q = QAOA()
q.compile(qubo_problem)
q.optimize()
```

A QAOA workflow is composed by 4 parts. this example, we have used the default values:

* **the circuit ansats** a 1-layer qaoa, with the `standard` parametrisation and the `x` mixer
* **the classical optimizer** `cobyla`, as implemented by the folks at SciPy
* **the device employed** `vectorized`, a very fast numpy-based QAOA simulator developed by Entropica Labs
* **the result of the algorithm**

All these properties can be easily accessed within the `QAOA` object. For example,

```Python
q.circuit_properties.asdict()
```

gives

```JSON
{'param_type': 'standard',
 'init_type': 'ramp',
 'qubit_register': [],
 'p': 1,
 'variational_params_dict': {'total_annealing_time': 0.7},
 'annealing_time': 0.7,
 'linear_ramp_time': 0.7,
 'mixer_hamiltonian': 'x'}
```

## Customizing a workflow

It is now time to customise the QAOA workflow.

Again, first things first: we need to create a problem instance. For example, an instance of vertex cover:

```Python
import networkx
from openqaoa.problems import MinimumVertexCover

g = networkx.circulant_graph(6, [1])
vc = MinimumVertexCover(g, field=1.0, penalty=10)
qubo_problem = vc.qubo
```

Now that we have the qubo problem, we can create a custom workflow as follows

```Python
#Create the QAOA
q = QAOA()


# circuit properties
q.set_circuit_properties(p=3, param_type='standard', init_type='ramp', mixer_hamiltonian='xy')

# backend properties
q.set_backend_properties(init_hadamard=True)

# classical optimizer properties
q.set_classical_optimizer(method='cobyla', maxiter=50, tol=0.05)

q.compile(qubo_problem)
q.optimize()
```

## The circuit properties

Let's break down the process. First, we create the `QAOA()` object and modify the circuit's ansatz. This can be done through the method `set_circuit_properties()`

```Python hl_lines="7"
from openqaoa import QAOA, create_device  

#Create the QAOA
q = QAOA()

# circuit properties
q.set_circuit_properties(p=3, param_type='standard', init_type='ramp', mixer_hamiltonian='xy')
```

This method allows to shape the properties of the ansats circuit. It is the place where you can select the number of layers `p`, the type desired parametrization (and its initialization), and the type of mixer that you want to use. 

!!! tip
    If you want to read more about parametrisations and initialisations, please refer to the [parametrization](../parametrization/parametrization.md) pages.

## The backend properties

The backend identifies the device where the computation will be executed. It is separate from the device object, because it is useful to define some of the features in a device-independent fashion.

Let's see an example

```Python hl_lines="10"
from openqaoa import QAOA, create_device  

#Create the QAOA
q = QAOA()

# circuit properties
q.set_circuit_properties(p=3, param_type='standard', init_type='ramp', mixer_hamiltonian='xy')

# backend properties
q.set_backend_properties(init_hadamard=True)
```

here we are applying an initial layer of Hadamard gates to all qubits.


## The classical optimizer

As QAOA is a classical-quantum hybrid computation, we can customise the classical part to. This is done through the classical optimizer attribute

```Python hl_lines="13"
from openqaoa import QAOA, create_device  

#Create the QAOA
q = QAOA()

# circuit properties
q.set_circuit_properties(p=3, param_type='standard', init_type='ramp', mixer_hamiltonian='xy')

# backend properties
q.set_backend_properties(init_hadamard=True)

# classical optimizer properties
q.set_classical_optimizer(method='cobyla', maxiter=200, tol=0.05)
```

Note that since we are running the computation over the cloud we set `maxiter=200` (that is, we cap the number of circuit evaluations to 200) and set a tolerance `tol=0.05`, roughly meaning that we will stop the optimization loop as soon as gains between consecutive cost values are smaller than the tolerance.

Currently OpenQAOA allows both for [gradient based](../optimizers/gradient-based-optimizers/gradient-based-optimizers.md) and [gradient free](../optimizers/gradient-free-optimizers.md) optimization methods, together with a wide selections of optimizers inherited both by SciPy and [PennyLane](../optimizers/pennylane-optimizers.md).

Extensive description of the optimizers available on OpenQAOA can be found on the [optimizer](../optimizers/optimizers.md) page. 