# Customise the QAOA workflow

The simples QAOA workflow makes use of the OpenQAOA default values


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
from openqaoa import QAOA, create_device
from qiskit import IBMQ

#Create the QAOA
q = QAOA()

# Create a device
ibmq_device = create_device(location='ibmq', name='ibm_perth')
q.set_device(ibmq_device)

# circuit properties
q.set_circuit_properties(p=3, param_type='standard', init_type='ramp', mixer_hamiltonian='xy')

# backend properties
q.set_backend_properties(init_hadamard=True, n_shots=8000, cvar_alpha=0.85)

# classical optimizer properties
q.set_classical_optimizer(method='nelder-mead', maxiter=200)

q.compile(qubo_problem)
q.optimize()
```

## The device

Let's break down the process. First, we create the `QAOA()` object and set a device

```Python hl_lines="11 12"
from openqaoa import QAOA, create_device  
from qiskit import IBMQ

#Create the QAOA
q = QAOA()

# Load account from disk
IBMQ.load_account() 

# Create a device
ibmq_device = create_device(location='ibmq', name='ibm_perth')
q.set_device(ibmq_device)
```

With these two lines of code we first create a device, and then set the it within the QAOA workflow.

!!! Tip 
    For the above code to work, you need to have saved your IBMQ credentials on your device (see the use of `IBMQ.save_account()` on the [official IBMQ docs](https://quantum-computing.ibm.com/lab/docs/iql/manage/account/ibmq) for more info!  To check all available backends go to the [devices page](/devices/device.md)


## The circuit Ansatz

Next, we can modify the circuit's ansatz. This can be done through the method `set_circuit_properties()`

```Python hl_lines="15 16"
from openqaoa import QAOA, create_device  
from qiskit import IBMQ

#Create the QAOA
q = QAOA()

# Load account from disk
IBMQ.load_account() 

# Create a device
ibmq_device = create_device(location='ibmq', name='ibm_perth')
q.set_device(ibmq_device)

# circuit properties
q.set_circuit_properties(p=3, param_type='standard', init_type='ramp', mixer_hamiltonian='xy')
```

This method allows to shape the properties of the ansats circuit. It is the place where you can select the number of layers `p`, the type desired parametrization (and its initialization), and the type of mixer that you want to use. 

!!! tip
    If you want to read more about parametrisations and initialisations, please refer to the [parametrization](/docs/parametrization/parametrization.md) pages.

## The backend properties

The backend identifies the device where the computation will be executed. It is separate from the device object, because it is useful to define some of the features in a device-independent fashion.

Let's see an example

```Python hl_lines="18"
from openqaoa import QAOA, create_device  
from qiskit import IBMQ

#Create the QAOA
q = QAOA()

# Load account from disk
IBMQ.load_account() 

# Create a device
ibmq_device = create_device(location='ibmq', name='ibm_perth')
q.set_device(ibmq_device)

# circuit properties
q.set_circuit_properties(p=3, param_type='standard', init_type='ramp', mixer_hamiltonian='xy')

# backend properties
q.set_backend_properties(init_hadamard=True, n_shots=8000, cvar_alpha=0.85)
```

here we are fixing:
- The number of shots: this is a fundamental value when running computations on a QPU or on a shot-based simulator
- The initial round of Hamiltonians is set to True
- Setting the value for the [Conditional Value-at-Risk](https://research.ibm.com/publications/improving-variational-quantum-optimization-using-cvar), a trick employed during the calculation of the expectation value

## The classical optimizer

As QAOA is a classical-quantum hybrid computation, we can customise the classical part to. This is done through the classical optimizer attribute

```Python hl_lines="21"
from openqaoa import QAOA, create_device  
from qiskit import IBMQ

#Create the QAOA
q = QAOA()

# Load account from disk
IBMQ.load_account() 

# Create a device
ibmq_device = create_device(location='ibmq', name='ibm_perth')
q.set_device(ibmq_device)

# circuit properties
q.set_circuit_properties(p=3, param_type='standard', init_type='ramp', mixer_hamiltonian='xy')

# backend properties
q.set_backend_properties(init_hadamard=True, n_shots=8000, cvar_alpha=0.85)

# classical optimizer properties
q.set_classical_optimizer(method='nelder-mead', maxiter=200)
```

Currently OpenQAOA allows both for [gradient based](/docs/optimizers/gradient-based-optimizers.md) and [gradient free](/docs/optimizers/gradient-free-optimizers.md) optimization methods, together with a wide selections of optimizers inherited both by SciPy and [PennyLane](/docs/optimizers/pennylane-optimizers.md).

Extensive description of the optimizers available on OpenQAOA can be found on the [optimizer](/docs/optimizers/optimizers.md) page. 