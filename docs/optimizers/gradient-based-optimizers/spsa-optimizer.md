# Simultaneous perturbation stochastic approximation optimizer
Simultaneous Perturbation Stochastic Approximation (SPSA) is a gradient-free optimization method that uses stochastic approximations of the gradient. Unlike real gradient-based methods like gradient descent, SPSA does not require knowledge of the gradient of the function being optimized. Instead, SPSA estimates the gradient by perturbing the parameters of the function in a random direction and observing the resulting change in the cost function. The update rule for SPSA is given by:

$$ \vec\gamma^{(k+1)} = \vec\gamma^{(k)} - a_k \vec\Delta_k, $$

where $a_k$ is the step size, which is calculated as:

$$a_k = \frac{a_0}{(A + k)^\alpha} ,$$

and can be controlled by the following hyperparameters:
 - scaling parameter $a_0$
 - decay rate parameter $\alpha$
 - decay constant $A$.

On the other hand, $\vec\Delta_k$ is an estimate of the gradient of the cost function, which is computed as:

$$ \vec\Delta_k = \frac{f(\vec\gamma^{(k)} + c_k \vec\Delta) - f(\vec\gamma^{(k)} - c_k \vec\Delta)}{2 c_k }\odot \vec\Delta, $$

where $\vec\Delta$ is a vector of random perturbations that are generated independently for each parameter at each iteration:

$$ \vec\Delta = (\delta_1, \delta_2, ..., \delta_n), \quad \delta_i \in \{-1, 1\} \text{ randomly generated}, $$

and $c_k$ is a perturbation sequence that ensures that the perturbations are small enough to avoid large changes in the cost function, but large enough to estimate the gradient accurately. It is computed as:

$$ c_k = \frac{c_0}{k^{\Gamma}}, $$

which can be controlled by the hyperparameters:
 - scaling parameter $c_0$
 - decay rate parameter $\Gamma$.

By iteratively applying the update rule with appropriately chosen hyperparameters, SPSA can converge to a local minimum of the function being optimized.

## OpenQAOA example
In the code below it is shown how to run QAOA with the SPSA optimizer, using the following hyperparameters: $a_0=0.01$, $c_0=0.01$, $A=1$, $\alpha=0.602$, and $\Gamma=0.101$.

```Python hl_lines="6 7 8 9 10 11 12 13 14 15 16"
from openqaoa import QAOA 

# create the QAOA object
q = QAOA()

# set optimizer and properties
q.set_classical_optimizer(
    method='spsa', 
    optimizer_options=dict(
        a0=0.01,
        c0=0.01,
        A=1,
        alpha=0.602,
        gamma=0.101,
    )
)

# compile and optimize using the chosen optimizer
q.compile(problem)
q.optimize()
```