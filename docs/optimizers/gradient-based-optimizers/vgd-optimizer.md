# Gradient descent optimizer

Gradient descent is a first-order iterative optimization algorithm. It works by repeatedly adjusting the parameters of the function in the direction of the negative gradient of the function. The size of the adjustment is controlled by a hyperparameter called the step size, which determines how far to step in the direction of the negative gradient. By repeating this process over many iterations, gradient descent can converge to a local minimum of the function, which may or may not be the global minimum.

The gradient descent optimizer updates the values in each iteration by applying the following rule:

$$ \vec\gamma^{(k+1)} = \vec\gamma^{(k)} - \alpha \vec\nabla f(\vec\gamma^{(k)}), $$

where $\vec\gamma^{(k)}$ is the set of parameters at the $k\text{-th}$ iteration, $f$ the cost function and $\alpha$ is the step size.

## OpenQAOA example
In the code below it is shown how to run QAOA with the gradient decent optimizer, using a step size $\alpha=0.001$ and approximating the Jacobian with finite difference method.

```Python hl_lines="6 7 8 9 10 11"
from openqaoa import QAOA 

# create the QAOA object
q = QAOA()

# set optimizer and properties
q.set_classical_optimizer(
    method='vgd', 
    jac="finite_difference", 
    optimizer_options=dict(stepsize=0.001)
)

# compile and optimize using the chosen optimizer
q.compile(problem)
q.optimize()
```