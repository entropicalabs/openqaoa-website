# Parametrize the circuit

We currently offer 7 different parametrizations for QAOA, which can be found in
the `openqaoa.qaoa_parameters` module. They fall broadly into three categories:

* The `Standard` classes are parametrizations that have the $\gamma$ 's and $\beta$ 's as free parameters, as defined in the seminal paper by Farhi `et al` in [A Quantum Approximate Optimization Algorithm](https://arxiv.org/abs/1411.4028).
* The `Fourier` classes have the discrete cosine and sine transforms of the $\gamma$ 's respective $\beta$'s as free parameters, as proposed by Zhou et al. in [Quantum Approximate Optimization Algorithm: Performance, Mechanism, and Implementation on Near-Term Devices](https://arxiv.org/abs/1812.01041).
* The `Annealing` class is based on the idea of QAOA being a form of discretized, adiabatic annealing. Here the function values $s(t_i)$ at equally spaced times $t_i$ are the free parameters.

Except for the `Annealing` parameters, each class also comes in three levels of detail: 

* `StandardParams` and `FourierParams` offer the $\gamma$ 's and $\beta$ 's as proposed in above papers. 
* `StandardWithBiasParams` and `FourierWithBiasParams` allow for extra $\gamma$'s for possible single-qubit bias terms, resp. their discrete sine transform. 
* `ExtendedParams` and `FourierExtendedParams` offer full control by having a separate set of rotation angles for each term in the cost and mixer Hamiltonians, respective having a seperate set of Fourier coefficients for each term.

!!! Tip
    You can always convert parametrizations with fewer degrees of freedom to ones with more using the `.from_other_parameters()` classmethod. The full type
    tree is shown below, where the arrows mark possible conversions:

.. code-block::

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