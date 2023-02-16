# PennyLane optimizers

In addition to the gradient-based and gradient-free optimizers discussed in [the gradient-based optimizers](../optimizers/gradient-based-optimizers.md) and [the gradient-free optimizers](../optimizers/gradient-free-optimizers.md) pages, some of the optimizers from the PennyLane library have been adapted for use with OpenQAOA. The following table shows this adapted optimizers:

| OpenQAOA name | PennyLane name | Method | Description |
| :------------ | :------------- | :----- | :---------- |
|`pennylane_adagrad` | `AdagradOptimizer` | Adagrad | Gradient-descent optimizer with past-gradient-dependent learning rate in each dimension. |
|`pennylane_adam` | `AdamOptimizer` | Adam | Optimizer for building fully trained quantum circuits by adding gates adaptively. |
|`pennylane_vgd` | `GradientDescentOptimizer` | Gradient Descent | Vanilla gradient-descent optimizer. |
|`pennylane_momentum` | `MomentumOptimizer` | Momentum Gradient Descent | Gradient descent optimizer with momentum. |
|`pennylane_nesterov_momentum` | `NesterovMomentumOptimizer` | Nesterov Momentum Gradient Descent | Gradient descent optimizer with Nesterov momentum. |
|`pennylane_rmsprop` | `RMSPropOptimizer` | Root Mean Squared Propagation | Adaptive learning rate optimization method that uses a moving average of squared gradients to normalize the gradient.|
|`pennylane_rotosolve` | `RotosolveOptimizer` | Rotosolve | Gradient-free optimizer that updates circuit parameters by separately reconstructing the cost function for each parameter while holding all other parameters constant. |
|`pennylane_spsa` | `SPSAOptimizer` | Rotosolve | A gradient-free optimizer that uses stochastic approximations of the gradient. |

For more information on these optimizers and their hyperparameters, we recommend referring to the [PennyLane optimizers documentation](https://docs.pennylane.ai/en/stable/introduction/interfaces.html#optimizers).


!!! info 

    It's worth noting that some of the optimizers from the PennyLane library, namely Gradient Descent, RMSProp, and SPSA, are already directly implemented in OpenQAOA. This means that using `pennylane_vgd` is equivalent to using `vgd`, `pennylane_rmsprop` is equivalent to `rmsprop`, and `pennylane_spsa` is equivalent to `spsa`.


## Example in OpenQAOA

In this example, we demonstrate how to run a QAOA by using an optimizer adapted from the PennyLane library in OpenQAOA. Specifically, we use pennylane_nesterov_momentum optimizer, which updates the parameters according to the following formula:

$$ \vec\gamma^{(k+1)} = \vec\gamma^{(k)} - \vec a^{(t+1)}, $$

where

$$ \vec a^{(t+1)}=m \,\vec a^{(t)}+\alpha \vec \nabla f\left(\vec\gamma^{(k)}-m \,\vec a^{(t)}\right),$$

here, $\alpha$ denotes the step size, and $m$ denotes the momentum.

The following code snippet shows how to use pennylane_nesterov_momentum in OpenQAOA:

```Python hl_lines="6-15"
from openqaoa import QAOA 

# create the QAOA object
q = QAOA()

# set optimizer and properties
q.set_classical_optimizer(
    method='pennylane_nesterov_momentum',     
    jac='finite_difference',
    maxiter=200,
    optimizer_options=dict(
        stepsize=0.015,
        momentum=0.5,
    )
)

# compile and optimize using the chosen optimizer
q.compile(problem)
q.optimize()
```

In this example, we set the `method` argument to `pennylane_nesterov_momentum` and approximate the Jacobian using `finite_difference`. We then set the maximum number of iterations to 200 and customize the optimizer by setting the step size to 0.015 and the momentum to 0.5.