# Analytical Simulator

The analytical simulator computes the expectation value of the cost Hamiltonian, $H_C$, with respect to the given circuit, using the `energy_expectation_analytical` function in openqaoa.utilities. The formula can be found in the appendices of [[1]](#references). It is mostly used to study QAOAs with a single layer, that is it only works for $p=1$. 

!!! note "For the curious"
    Since the expectation value of the quantum state prepared with a p=1 QAOA circuit cna be computed efficiently on a classical computer, $O(n^4)$, there's no quantum speedup for such circuits. Nevertheless, those are very interesting from research perspective.
## Key features:

- Only works for a single-layer circuits, $p=1$, `standard` parametrization, `x_mixer_hamiltonian` 
- Preferred choice for large circuits, $n \geq 20$.
- Noiseless simulations


## Explicitly create the analytical simulator device

If you want to explicitly set the analytical simulator device use the following snippet

```Python
from openqaoa import QAOA, create_device

analytical_device = create_device(location='local', name='analytical_simulator')
q.set_device(analytical_device)
```

!!! note "Expectation values of RQAOA"
    Recall that [RQAOA](/workflows/recursive-qaoa.md) requires computing the expectation values of pair and single spins. When using the analytical device, the expectation values will also be computed analytically, using the `exp_val_single_analytical` and `exp_val_pair_analytical` functions from the `openqaoa.utilities`. 

## References
----------
1. S. Bravyi, A. Kliesch, R. Koenig, and E. Tang, (2022) Quantum 6, 678, [https://arxiv.org/abs/2011.13420](https://arxiv.org/abs/2011.13420).