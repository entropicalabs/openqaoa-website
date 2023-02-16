# Parametrization and Initialization

There are many different ways in which a QAOA circuit can be parametrized, and even more the ways in which these parameters can be initialized.

## Basic Parametrizations


We currently offer 7 different parametrizations for QAOA. They fall broadly into three categories:

* The [Standard](/docs/parametrization/parametrization.md) classes are parametrization that have the $\gamma$ 's and $\beta$ 's as free parameters, as defined in the seminal paper by Farhi et al. [1].
* The [Fourier](docs/parametrization/fourier-parametrization.md) classes have the discrete cosine and sine transforms of the $\gamma$'s respective $\beta$'s as free parameters, as proposed by Zhou et al. [2].
* The [Annealing](docs/parametrization/annealing-parametrization.md) class is based on the idea of QAOA being a form of discretized, adiabatic annealing. Here the function values $s(t_i)$ at equally spaced times $t_i$ are the free parameters.

Except for the `Annealing` parameters, each class also comes in three levels of detail: 

* `StandardParams` and `FourierParams` offer the $\gamma$ 's and $\beta$ 's as proposed in references given above. 
* `StandardWithBiasParams` and `FourierWithBiasParams` allow for extra $\gamma$'s for possible single-qubit bias terms and their discrete sine transform, respectively.
* `ExtendedParams` and `FourierExtendedParams` offer full control by having a separate set of rotation angles for each term in the cost and mixer Hamiltonians, respective having a seperate set of Fourier coefficients for each term.

## Basic initializations

Before starting the algorithm, the parameters need to be initialized! That is, we need to give them some initial value. The choice of this value is arbitrary, and the act of coming up with good initial parameters is an ard on its own! 

OpenQAOA supports three different initialization strategies:

* **rand** (for random) sets the values of $\beta$ and $\gamma$ uniformly at random in the range $[0, \pi]$. 
* **ramp** create evenly spaced timelayers at the centers of p intervals, setting $\beta$ and $\gamma$ according
            to a linear ramp schedule. If `time` is not specified, the annealing time is set to $0.7p$. For example, this specifies ($\beta$, $\gamma$) = (0.35, 0.35) as the optimization starting point in a single-layer QAOA.
* **custom** allows the user to initialize the angles with custom values.

The above can be combined to create a `Variational Parameter Class` with the Standard Parametrisation and Ramp Initializations

```Python
create_qaoa_variational_params(qaoa_circuit_params=qaoa_circuit_params,
                                params_type='standard',
                                init_type='ramp')

```
or with the Extended Parametrisation and Random Initializations
```Python
create_qaoa_variational_params(qaoa_circuit_params=qaoa_circuit_params,
                                params_type='extended',
                                init_type='rand')
```

or with 

```Python
create_qaoa_variational_params(qaoa_circuit_params=qaoa_circuit_params,
                                params_type='fourier',
                                q=1,
                                init_type='custom',
                                variational_params_dict={
                                    "betas":[0.26],
                                    "gammas":[0.42]
                                    }
                                )
```

Note that when using the fourier type, we need to specify the `q` value! For more details check out the [fourier-parametrization](docs/parametrization/fourier-parametrization.md) page.

!!! warning "Using `custom`"
    When using the `custom` initialization strategy, we have to provide the appropriate number variational parameters in the form of a dictionary, `variational_params_dict`. The keys of the this dictionary are specific for the chose parametrization and the values depend on the number of values, p. Examples for how to use `custom` can be found in the separate pages.

References
----------
1. E. Farhi, J. Goldstone, S. Gutmann (2014). A Quantum Approximate Optimization Algorithm, [https://arxiv.org/abs/1411.4028](https://arxiv.org/abs/1411.4028).
2. L. Zhou, S. Wang, S. Choi, H. Pichler, M. D. Lukin, Phys.Rev.X 10, 021067 (2020) Quantum Approximate Optimization Algorithm: Performance, Mechanism, and Implementation on Near-Term Devices, [https://arxiv.org/abs/1812.01041](https://arxiv.org/abs/1812.01041).