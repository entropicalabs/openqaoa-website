# Quantum natural gradient descent optimizer

Quantum natural gradient descent is a quantum optimization method that leverages the geometry of the parameter space and the quantum Fisher information matrix to improve convergence speed. In QNGD, the cost function and the gradient are transformed in a way that preserves the geometry of the parameter space. The update rule for QNGD is given by:

$$ \vec\gamma^{(k+1)} = \vec\gamma^{(k)} - \alpha \,[\vec{\vec{\mathcal{F}}}(f(\vec\gamma^{(k)}))+\lambda]^{-1}\,\vec\nabla f(\vec\gamma^{(k)}), $$

where $\alpha$ is an adaptive step size, $\vec{\vec{\mathcal{F}}}(f(\vec\gamma^{(k)}))$ is the Fubini-Study metric tensor, and $\lambda$ is a small hyperparameter to avoid singularity of the inverse of the metric tensor. 

## OpenQAOA example
In the example code below, the QAOA is configured to use the Quantum Natural Gradient Descent optimizer with a step size of 0.1 and the finite difference method for approximating the Jacobian. When using this optimizer, the Fubini-Study metric tensor is automatically computed by OpenQAOA.

```Python hl_lines="6 7 8 9 10 11 12 13"
from openqaoa import QAOA 

# create the QAOA object
q = QAOA()

# set optimizer and properties
q.set_classical_optimizer(
    method='natural_grad_descent', 
    jac="finite_difference",
    optimizer_options=dict(
        stepsize=0.1,
    )
)

# compile and optimize using the chosen optimizer
q.compile(problem)
q.optimize()
```