# Root mean squared propagation optimizer

RMSProp (short for Root Mean Squared Propagation) is an adaptive learning rate optimization method that uses a moving average of the squared gradients to normalize the gradient. The idea behind RMSProp is to divide the learning rate by a running average of the magnitudes of recent gradients for a given parameter. This helps prevent the learning rate from being too high or too low, and can speed up convergence. The update rule for RMSProp is given by:

$$ \vec\gamma^{(k+1)} = \vec\gamma^{(k)} - \frac{\alpha}{\sqrt{\vec{v}^{(k)} + \epsilon}} \odot \vec\nabla f(\vec\gamma^{(k)}), $$

where $\odot$ denotes element-wise multiplication, $\alpha$ is the step size, $\epsilon$ is a small hyperparameter to avoid division by zero, and $\vec{v}^{(k)}$ is a running average of the squared gradients:

$$ \vec{v}^{(k+1)} = \rho \vec{v}^{(k)} + (1-\rho) (\vec\nabla f(\vec\gamma^{(k+1)}))^2, $$

where $\rho$ is a hyperparameter that controls the weight given to past gradients in the moving average. $\rho$ is also called decay.

## OpenQAOA example
In the code below it is shown how to run QAOA with RMSProp optimizer, using a step size $\alpha=0.001$, decay $\rho=0.9$, constant $\epsilon=10^{-7}$ and approximating the Jacobian with finite difference method.

```Python hl_lines="6 7 8 9 10 11 12 13 14 15"
from openqaoa import QAOA 

# create the QAOA object
q = QAOA()

# set optimizer and properties
q.set_classical_optimizer(
    method='rmsprop', 
    jac="finite_difference", 
    optimizer_options=dict(
        stepsize=0.001,
        decay=0.9,
        eps=1e-07
    )
)

# compile and optimize using the chosen optimizer
q.compile(problem)
q.optimize()
```