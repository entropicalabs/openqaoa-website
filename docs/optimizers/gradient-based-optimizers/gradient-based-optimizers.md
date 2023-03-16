# Gradient-based optimizers

In OpenQAOA offers different gradient-based optimizers to solve QAOA. You can read more about optimization algorithms in the [optimizers landing page](/optimizers). 

Gradient-based optimizers, at each iteration, update the parameters using a rule that looks like:

$$ \vec\gamma^{(k+1)} = \vec\gamma^{(k)} - A(\vec \nabla f(\vec\gamma^{(k)}), ...), $$

where at each iteration the new parameters are computed by updating the parameters found at the last iteration subtracting $A(...)$, which is a value that depends on the Jacobian, $\vec \nabla f(\vec\gamma^{(k)})$, and some hyperparameters among other things. The formula of $A$ depends on the optimization algorithm chosen.  

Here you can find a list and a brief description of the gradient-based optimizers implemented in OpenQAOA, with a link to a detailed description of the method:

| Name | Method      | Description                 |  |
| :-------------- | :--------- | :---------------- | :------------------ | 
|`vgd`            | Gradient Descent  | Vanilla gradient-descent optimizer. | [link](./vgd-optimizer.md) |
| `rmsprop`        | Root Mean Squared Propagation | Adaptive learning rate optimization method that uses a moving average of squared gradients to normalize the gradient. | [link](./rmsprop-optimizer.md)|
| `newton`         | Newton's method | Second-order optimization method that uses the Hessian matrix. | [link](./newton-optimizer.md)|
| `natural_grad_descent` | Quantum Natural Gradient Descent | A quantum optimization method that leverages the geometry of the parameter space and the quantum Fisher information matrix to improve convergence speed. | [link](./natural-gd-optimizer.md)|
| `spsa`           | Simultaneous Perturbation Stochastic Approximation | A gradient-free optimization method that uses stochastic approximations of the gradient. | [link](./spsa-optimizer.md)|

In addition, there are two more gradient-based optimizers available: `newton-cg`, `l-bfgs-b`. These are included in the library scipy.optimize.minimize, please refer to their documentation to learn more about them.

Also,the analytical form of the cost function is not known therefore we need some method to compute the Jacobian $\vec\nabla f(\vec\gamma)$. OpenQAOA offers different methods to compute it: `finite_difference`, `param_shift`, `stoch_param_shift`, and `grad_spsa`. To read about the these please refer to the [Gradient computation](./gradient-computation.md) page.

## OpenQAOA example
In the code below it is shown how to run QAOA with a gradient-based optimizer like gradient descent and approximating the Jacobian with finite difference method.

```Python hl_lines="6 7 8 9 10"
from openqaoa import QAOA 

# create the QAOA object
q = QAOA()

# set optimizer and properties
q.set_classical_optimizer(
    method='vgd', 
    jac="finite_difference"
)

# compile and optimize using the chosen optimizer
q.compile(problem)
q.optimize()
```




