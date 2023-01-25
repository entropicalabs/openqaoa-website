# Customise the QAOA workflow

The simples QAOA workflow makes use of the OpenQAOA default values


```Python title="simples_qaoa_workflow.py"
from openqaoa.workflows.optimizer import QAOA  
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
qubo_problem = vc.get_qubo_problem()
```

Now that we have the qubo problem, we can create a custom workflow as follows

```Python
from openqaoa.workflows.optimizer import QAOA  
from openqaoa.devices import create_device

#Create the QAOA
q = QAOA()

# Create a device
qiskit_sv = create_device(location='qcs', name='aspen-m3')
q.set_device(qiskit_sv)

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

```Python hl_lines="8 9"
from openqaoa.workflows.optimizer import QAOA  
from openqaoa.devices import create_device

#Create the QAOA
q = QAOA()

# Create a device
aspen_m3_device = create_device(location='qcs', name='aspen-m3')
q.set_device(aspen_m3_device)
```

With these two lines of code we first create a device, and then set the it within the QAOA workflow.

!!! Tip 
    To check all available backends go to the [devices page](devices/device.md)


## The circuit Ansatz

Next, we can modify the circuit's ansatz. This can be done through the method `set_circuit_properties()`

```Python hl_lines="12 13"
from openqaoa.workflows.optimizer import QAOA  
from openqaoa.devices import create_device

#Create the QAOA
q = QAOA()

# Create a device
qiskit_sv = create_device(location='qcs', name='aspen-m3')
q.set_device(qiskit_sv)

# circuit properties
q.set_circuit_properties(p=3, param_type='standard', init_type='ramp', mixer_hamiltonian='xy')
```

This method allows to shape the properties of the ansats circuit. It is the place where you can select the number of layers `p`, the type desired parametrization (and its initialization), and the type of mixer that you want to use. 


## The backend properties

The backend properties 


## The classical optimizer