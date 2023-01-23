# Customise the QAOA workflow

It is now time to customise the QAOA workflow. In the previous example, we used implemented the QAOA workflow using its default value. Now, we will go through how these values can be modified

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

Let's break down the process. First, we create the `QAOA()` object

```Python hl_lines="8 9"
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
```

The `QAOA()` object is a workflow and contains all the details required to successfully build the circuit and execute it. However, even while using the default values the `QAOA()` object needs to take as input the problem statement

!!! note "Compilation and QUBOs"
    To build the QAOA ansatz we need the cost function, and the cost function is encoded in the qubo problem! :-)

```Python hl_lines="3"
from openqaoa.workflows.optimizer import QAOA  
q = QAOA()
q.compile(qubo_problem)
q.optimize()
```

Compiling the problem is a crucial step: now the representation of the QAOA workflow includes all the key information to build the circuit. So, all we are left with is the last step: the optimization1

```Python hl_lines="4"
from openqaoa.workflows.optimizer import QAOA  
q = QAOA()
q.compile(qubo_problem)
q.optimize()
```

