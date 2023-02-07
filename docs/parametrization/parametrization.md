# Parametrization and Initialization

We currently offer 7 different parametrizations for QAOA, which can be found in
the `src.openqaoa-core.qaoa_components.variational_parameters` module. They fall broadly into three categories:

* The `Standard` classes are parametrizations that have the $\gamma$ 's and $\beta$ 's as free parameters, as defined in the seminal paper by Farhi et al. [1].
* The `Fourier` classes have the discrete cosine and sine transforms of the $\gamma$'s respective $\beta$'s as free parameters, as proposed by Zhou et al. [2].
* The `Annealing` class is based on the idea of QAOA being a form of discretized, adiabatic annealing. Here the function values $s(t_i)$ at equally spaced times $t_i$ are the free parameters.

Except for the `Annealing` parameters, each class also comes in three levels of detail: 

* `StandardParams` and `FourierParams` offer the $\gamma$ 's and $\beta$ 's as proposed in above papers. 
* `StandardWithBiasParams` and `FourierWithBiasParams` allow for extra $\gamma$'s for possible single-qubit bias terms and their discrete sine transform, respectively.
* `ExtendedParams` and `FourierExtendedParams` offer full control by having a separate set of rotation angles for each term in the cost and mixer Hamiltonians, respective having a seperate set of Fourier coefficients for each term.


!!! Tip
    You can always convert parametrizations with fewer degrees of freedom to ones with more using the `.from_other_parameters()` classmethod. The full type
    tree is shown below, where the arrows mark possible conversions:

```
   ExtendedParams   <--------- FourierExtendedParams
          ^                         ^
          |                         |
StandardWithBiasParams <------ FourierWithBiasParams
          ^                         ^
          |                         |
    StandardParams  <----------- FourierParams
          ^
          |
    AnnealingParams
```

We also support 3 different initializations at the moment:

* `rand` sets the values of $\beta$ and $\gamma$ uniformly at random in the range $[0, \pi]$. 
* `ramp` create evenly spaced timelayers at the centers of p intervals, setting $\beta$ and $\gamma$ according
            to a linear ramp schedule. If `time` is not specified, the annealing time is set to $0.7p$. For example, this specifies ($\beta$, $\gamma$) = (0.35, 0.35) as the optimization starting point in a single-layer QAOA.
* `custom` allows the user to initialise the angles with custom values.

The above can be combined to create a `Variational Parameter Class` with the Standard Parameterisation and Ramp Initialisation
```Python
create_qaoa_variational_params(qaoa_circuit_params = qaoa_circuit_params, params_type = 'standard', init_type = 'ramp')
```
or with the Extended Parameterisation and Random Initialisation
```Python
create_qaoa_variational_params(qaoa_circuit_params = qaoa_circuit_params, params_type = 'extended', init_type = 'rand')
```
or with 
```Python
create_qaoa_variational_params(qaoa_circuit_params = qaoa_circuit_params, params_type = 'fourier', init_type='custom', variational_params_dict={"betas":[0.26], "gammas":[0.42]})
```

**NOTE**: When using the custom initialization strategy, we have to provide the appropriate number of betas and gammas depending on the number of variational parameters, which depends on the p-value and parameterisation type.

References
----------
1. E. Farhi, J. Goldstone, S. Gutmann (2014). A Quantum Approximate Optimization Algorithm.
2. L. Zhou, S. Wang, S. Choi, H. Pichler, M. D. Lukin, Phys.Rev.X 10, 021067 (2020) Quantum Approximate Optimization Algorithm: Performance, Mechanism, and Implementation on Near-Term Devices