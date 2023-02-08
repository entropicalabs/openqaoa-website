# Fourier Parametrization

The class `QAOAVariationalFourierParams` implements a heuristic proposed by Zhou et al. [1] which instead of optimizing for $\beta$ and $\gamma$ at each layer aims at finding the amplitudes of their discrete Fourier transforms given by:

$\beta_i^{(p)} = \sum_{k=0}^{q-1} v_k \cos\left[(2k+1)i\frac{\pi}{2q}\right]$

$\gamma_i^{(p)} = \sum_{k=0}^{q-1} u_k \sin\left[(k+1/2)(i+1)\frac{\pi}{q}\right]$

where $i = 0,...,p$.

The idea comes from the empirical observation that the optimal parameters are often found to be smoothly varying functions for certain problems (such as the one implemented by the `QAOAVariationalAnnealingParams` class). Hence, it should be possible to use a *reduced* parameter set consisting of only the lowest $q$ frequency components of those functions. Clearly, for $q\geq p$ we have the full expressivity of the original parameter set (i.e. the $\beta$ and $\gamma$). In this parametrisation, for fixed $q$, the optimisation problem therefore becomes that of finding the optimal Fourier components $v_k$ and $u_k$. 

In [1], the authors show that certain variants of this basic Fourier parametrisation can perform significantly better than a randomised brute force search through parameter space, for MaxCut problems on 3-regular and 4-regular graphs. 

Using once again the example cost Hamiltonian $H_C = 2.5 (Z_0Z_1 + Z_1Z_2 + Z_0Z_2) + 3.5 (Z_0 + Z_1 + Z_2)$ and setting the parameters using

```Python
q.set_circuit_properties(p=4, q=2, param_type = 'fourier', init_type='custom', variational_params_dict={"u":[0.1, 0.2], "v":[0.9, 0.8]})
```

As a circuit, this looks like:
![kamada_kawai_layout](/img/circuit_fourier.png)

**NOTE**: When the fourier parametrization is initialized with `ramp`, the optimization starts from $\vec{v} = \vec{u} = [0.35, 0, 0 ... 0]$ where the length of the vectors is $q$. This is because at a single layer, $p=1$, the standard parameters $\beta$ and $\gamma$ are equal to 0.35 (for a linear time of 0.7). 


**NOTE**: The depth of the circuit is still linear in the number of layers, $p$. The advantage is in the smaller number of variational parameters to be optimized, as long as $q \leq p$.

The class `QAOAVariationalFourierWithBiasParams` extends upon the above by allowing for an additional parameter controlling the single qubit rotations around the Z axis, analogous to what `StandardWithBiasParams` implements. 


References
----------
1. L. Zhou, S. Wang, S. Choi, H. Pichler, and M. D. Lukin, Phys. Rev. X 10, 021067 (2020)