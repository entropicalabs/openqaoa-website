# Newton's optimizer

Newton's method is a second-order optimization algorithm that uses the Hessian matrix of the function being optimized to compute the optimal direction to take in each iteration. Unlike gradient descent and RMSProp, which only use first-order information (the gradient), Newton's method also considers second-order information (the Hessian) about the shape of the function. The update rule for Newton's method is given by:

$$ \vec\gamma^{(k+1)} = \vec\gamma^{(k)} - \alpha [\vec{\vec{\nabla}} {}^2 f(\vec\gamma^{(k)})]^{-1} \vec\nabla f(\vec\gamma^{(k)}), $$

where $[\vec{\vec{\nabla}} {}^2 f(\vec\gamma^{(k)})]^{-1}$ is the inverse of the Hessian matrix of the cost function, and $\alpha$ is the step size.

## OpenQAOA example
In the example code below, the QAOA is configured to use the Newton's method with a step size of 0.1, and the finite difference method for approximating the Jacobian and the Hessian.

```Python hl_lines="6-14"
from openqaoa import QAOA 

# create the QAOA object
q = QAOA()

# set optimizer and properties
q.set_classical_optimizer(
    method='newton', 
    jac="finite_difference",
    hess="finite_difference",
    optimizer_options=dict(
        stepsize=0.1,
    )
)

# compile and optimize using the chosen optimizer
q.compile(problem)
q.optimize()
```