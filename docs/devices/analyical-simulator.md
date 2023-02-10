# Analytical Simulator - unlock larger circuits

The analytical simulator computes the expectation value of the cost Hamiltonian with respect to the given circuit and it is mostly used to study QAOAs with a single layer only, p=1. 

## Key features of the vectorized device

- Only works for a single-layer circuits, p=1, `standard` parametrization, `x_mixer_hamiltonian` 
- Preferred choice for large circuits, $n \geq 20$.
- Noiseless simulations


## Explicitly create the analytical simulator device

If you want to explicitly set the analytical simulator device use the following snippet

```Python
annalytical_device = create_device(location='local', name='annalytical_simulator')
q.set_device(annalytical_device)
```

