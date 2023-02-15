# Gradient-based optimizers

In OpenQAOA offers different gradient-based optimizers to solve QAOA. You can read more about optimization algorithms in the [optimizers landing page](/optimizers). Here you can find a list and a brief description of those:

| Name | Method      | Description                          |
| :-------------- | :--------- | :----------------------------------- |
|`vgd`            | Gradient Descent  | Vanilla gradient-descent optimizer. |
| `rmsprop`        | Root Mean Squared Propagation | Adaptive learning rate optimization method that uses a moving average of squared gradients to normalize the gradient. |
| `newton`         | Newton's method | Second-order optimization method that uses the Hessian matrix. |
| `natural_grad_descent` | Quantum Natural Gradient Descent | A quantum optimization method that leverages the geometry of the parameter space and the quantum Fisher information matrix to improve convergence speed. |
| `spsa`           | Simultaneous Perturbation Stochastic Approximation | A gradient-free optimization method that uses stochastic approximations of the gradient. |

## Optimizers description

### Gradient Descent

Gradient descent is a first-order iterative optimization algorithm. It works by repeatedly adjusting the parameters of the function in the direction of the negative gradient of the function. The size of the adjustment is controlled by a hyperparameter called the step size, which determines how far to step in the direction of the negative gradient. By repeating this process over many iterations, gradient descent can converge to a local minimum of the function, which may or may not be the global minimum.

The gradient descent optimizer updates the values in each iteration by applying the following rule:

$$ \vec\gamma^{(k+1)} = \vec\gamma^{(k)} - \alpha \vec\nabla f(\vec\gamma^{(k)}), $$

where $\vec\gamma^{(k)}$ is the set of parameters at the $k\text{th}$ iteration, $f$ the cost function and $\alpha$ is the step size.

#### OpenQAOA example
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

### Root Mean Squared Propagation

RMSProp (short for Root Mean Squared Propagation) is an adaptive learning rate optimization method that uses a moving average of the squared gradients to normalize the gradient. The idea behind RMSProp is to divide the learning rate by a running average of the magnitudes of recent gradients for a given parameter. This helps prevent the learning rate from being too high or too low, and can speed up convergence. The update rule for RMSProp is given by:

$$ \vec\gamma^{(k+1)} = \vec\gamma^{(k)} - \frac{\alpha}{\sqrt{\vec{v}^{(k)} + \epsilon}} \odot \vec\nabla f(\vec\gamma^{(k)}), $$

where $\odot$ denotes element-wise multiplication, $\alpha$ is the step size, $\epsilon$ is a small hyperparameter to avoid division by zero, and $\vec{v}^{(k)}$ is a running average of the squared gradients:

$$ \vec{v}^{(k+1)} = \rho \vec{v}^{(k)} + (1-\rho) (\vec\nabla f(\vec\gamma^{(k+1)}))^2, $$

where $\rho$ is a hyperparameter that controls the weight given to past gradients in the moving average. $\rho$ is also called decay.

#### OpenQAOA example
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

### Newton's method
Newton's method is a second-order optimization algorithm that uses the Hessian matrix of the function being optimized to compute the optimal direction to take in each iteration. Unlike gradient descent and RMSProp, which only use first-order information (the gradient), Newton's method also considers second-order information (the Hessian) about the shape of the function. The update rule for Newton's method is given by:

$$ \vec\gamma^{(k+1)} = \vec\gamma^{(k)} - \alpha [\vec{\vec{\nabla}} {}^2 f(\vec\gamma^{(k)})]^{-1} \vec\nabla f(\vec\gamma^{(k)}), $$

where $[\vec{\vec{\nabla}} {}^2 f(\vec\gamma^{(k)})]^{-1}$ is the inverse of the Hessian matrix of the cost function, and $\alpha$ is the step size.

#### OpenQAOA example
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

### Quantum Natural Gradient Descent

Quantum natural gradient descent is a quantum optimization method that leverages the geometry of the parameter space and the quantum Fisher information matrix to improve convergence speed. In QNGD, the cost function and the gradient are transformed in a way that preserves the geometry of the parameter space. The update rule for QNGD is given by:

$$ \vec\gamma^{(k+1)} = \vec\gamma^{(k)} - \alpha \,[\vec{\vec{\mathcal{F}}}(f(\vec\gamma^{(k)}))+\lambda]^{-1}\,\vec\nabla f(\vec\gamma^{(k)}), $$

where $\alpha$ is an adaptive step size, $\vec{\vec{\mathcal{F}}}(f(\vec\gamma^{(k)}))$ is the Fubini-Study metric tensor, and $\lambda$ is a small hyperparameter to avoid singularity of the inverse of the metric tensor. 

#### OpenQAOA example
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

### Simultaneous Perturbation Stochastic Approximation
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

#### OpenQAOA example
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


## Gradient computation

When optimizing the parameters in the QAOA we don't know the analytical form of the cost function therefore we need some method to compute the Jacobian $\vec\nabla f(\vec\gamma)$. OpenQAOA offers different methods to compute it. Here you can find a list and a brief description of those:

| Name | Method | Description |
| :--- | :----- | :---------- |
| `finite_difference` | Finite Difference  | Computes the gradient by evaluating the function at points that are slightly perturbed from the current point. |
| `param_shift` | Parameter-shift | Compute exact derivative by evaluating the circuit at two points offset by $\pi/2$ from the parameter value using the parameter-shift rule.|
| `stoch_param_shift` | Stochastic Parameter-shift | Compute derivative by adding random components to the parameter-shift rule |
| `grad_spsa` | Gradient Simultaneous Perturbation Stochastic Approximation | A method for stochastic gradient descent that estimates the gradient using a finite-difference approximation with random perturbations of the parameters. |



### Finite Difference

The finite difference method is a numerical technique for approximating derivatives of a function. Given a function $f(\vec{\gamma})$, we want to approximate its Jacobian $\vec\nabla f(\vec{\gamma})=(\partial_1f, \partial_2f, ..., \partial_nf)$. The finite difference method uses the following formula to compute an approximation of the derivative:

$$ \frac{\partial f(\vec{\gamma})}{\partial\gamma_i} \approx \frac{f(\vec\gamma + \eta\,\vec{e}_i) - f(\vec\gamma - \eta\,\vec{e}_i)}{2\eta} $$

where $\eta$ is a small positive number, often called the step size, and $\vec{e}_i$ is the $i$ base vector: $\vec{e}_i = (0,...,0,1,0,...,0)$, where the $1$ is found the $i\text{th}$ position. Note that the accuracy of the approximation depends on the choice of $\eta$.

#### OpenQAOA example
In the code below it is shown how to run QAOA with a gradient-based optimizer like gradient descent and approximating the Jacobian with finite difference method. Here we are specifying the step size $\eta$ of the finite difference method through the `jac_options` argument.

```Python hl_lines="6 7 8 9 10 11 12 13"
from openqaoa import QAOA 

# create the QAOA object
q = QAOA()

# set optimizer and properties
q.set_classical_optimizer(
    method='vgd', 
    jac="finite_difference", 
    jac_options=dict(
        stepsize=0.001,
    )
)

# compile and optimize using the chosen optimizer
q.compile(problem)
q.optimize()
```


### Parameter-shift
The parameter-shift rule is a technique that allows for the exact computation of gradients for quantum circuits with certain gate sets. It works by exploiting the fact that the derivative of a unitary gate $U(\theta)$ with respect to its parameter $\theta$ can be expressed in terms of the gate itself, read XX. 

As a quick explanation, if we have a cost function like:

$$ f(\vec\gamma) 
= \langle\psi(\vec\gamma)|\,\mathcal{H}\,|\psi(\vec\gamma)\rangle 
= \langle\psi'(\vec\gamma')|\,\,\mathcal{U}^\dagger(\gamma_j)\mathcal{H}\,\mathcal{U}(\gamma_j)\,|\psi'(\vec\gamma')\rangle,$$

where $\vec\gamma'=(\gamma_1, ..., \gamma_{j-1}, \gamma_{j+1}, ..., \gamma_n)$ and the parametric gate $\mathcal{U}$ has the following shape:

$$ \mathcal{U}(\gamma_j) = \exp{(-i\,r\,\gamma_j\, P)}, $$

where $r$ is a constant, and $P$ is a Pauli gate. Then it can be shown that the partial derivative of $\gamma_i$ can be exactly computed as:

$$ \frac{\partial f(\vec\gamma)}{\partial\gamma_i} 
= r \, \bigg[ f\left(\vec\gamma + \frac{\pi}{4r}\vec{e}_j\right) - f\left(\vec\gamma - \frac{\pi}{4r}\, \vec{e}_j\right) \bigg],$$

where $\vec{e}_j$ is the $j$ base vector: $\vec{e}_j = (0,...,0,1,0,...,0)$, where the $1$ is found the $j\text{th}$ position. 

In the QAOA, it turns out that the cost function can be expressed as above for all the gates when we are using the extended parameters. This means that we can use the parameter-shift rule to compute the Jacobian when we are solving QAOA with extended parameters. However, we can also use standard parameters if we convert the standard parameters to extended before the gradient computation and after it we convert them back. To learn about the various parametrizations, please refer to the [Parametrization and Initialization page](../parametrization/parametrization.md).

#### OpenQAOA example
In the code below it is shown how to run QAOA with a gradient-based optimizer like gradient descent and approximating the Jacobian with the parameter-shift method.

```Python hl_lines="6 7 8 9 10"
from openqaoa import QAOA 

# create the QAOA object
q = QAOA()

# set optimizer and properties
q.set_classical_optimizer(
    method='vgd', 
    jac="param_shift",
)

# compile and optimize using the chosen optimizer
q.compile(problem)
q.optimize()
```

### Stochastic Parameter-shift

The Stochastic Parameter-Shift method is a gradient approximation technique that randomly samples a fixed number of parameters to compute the gradient using the Parameter-Shift method explained above. With this method, some of the partial derivatives will not be computed and will be left as zero. This method is only applicable to QAOA with standard parameterization since the gradient of each standard parameter is a combination of gradients of some extended parameters.

#### OpenQAOA example
In the code below it is shown how to run QAOA with a gradient-based optimizer like gradient descent and approximating the Jacobian with the stochastic parameter-shift method. In the Jacobian options we are setting the number of sampled $\beta$ and $\gamma$ parameters will be $4$ and $6$ respectively. 

```Python hl_lines="6 7 8 9 10 11 12 13 14"
from openqaoa import QAOA 

# create the QAOA object
q = QAOA()

# set optimizer and properties
q.set_classical_optimizer(
    method='vgd', 
    jac="stoch_param_shift",
    jac_options=dict(
        n_beta_single=4,
        n_gamma_pair=6,
    ),
)

# compile and optimize using the chosen optimizer
q.compile(problem)
q.optimize()
```

### Gradient Simultaneous Perturbation Stochastic Approximation
The Gradient Simultaneous Perturbation Stochastic Approximation is a method for stochastic gradient descent that estimates the gradient using a finite-difference approximation with random perturbations of the parameters. 

The parameters are perturbed by a random vector that takes values of +1 or -1 with equal probability, and the cost function is evaluated at two points: one where the parameters are perturbed in the positive direction, and another where they are perturbed in the negative direction. The gradient is then estimated using a finite-difference approximation:

$$ \vec\nabla f(\vec\gamma) \approx \frac{f(\vec\gamma + c \vec\Delta) - f(\vec\gamma - c \vec\Delta)}{2c} \odot  \vec\Delta $$

where $c$ is a constant that determines the step size, and $\vec\Delta$ is the perturbation vector: $\vec\Delta = (\delta_1, \delta_2, ..., \delta_n)$ where $ \delta_i \in \{-1, 1\}$ are randomly generated.

The primary benefit of Gradient SPSA is that it requires only two evaluations of the cost function to estimate the Jacobian, unlike the finite difference method, which requires two evaluations for each parameter.

#### OpenQAOA example
In the code below it is shown how to run QAOA with a gradient-based optimizer like gradient descent and approximating the Jacobian with the gradient SPSA method. In the Jacobian options we are setting the step size $c$ to $0.0003$. 

```Python hl_lines="6 7 8 9 10 11 12 13"
from openqaoa import QAOA 

# create the QAOA object
q = QAOA()

# set optimizer and properties
q.set_classical_optimizer(
    method='vgd', 
    jac="grad_spsa",
    jac_options=dict(
        stepsize=0.0003,
    ),
)

# compile and optimize using the chosen optimizer
q.compile(problem)
q.optimize()
```


<style>
    td{ cursor:pointer }
</style>
<script>
    const rows = document.getElementsByTagName("tr");
    const sections = document.getElementsByTagName("h3");

    for (let i = 1; i < rows.length; i++) {
        rows[i].addEventListener("click", () => {
            const method = rows[i].getElementsByTagName("td")[1].textContent;

            console.log(method)

            for (let j = 0; j < sections.length; j++) {
                console.log(sections[j].textContent)
                if (sections[j].textContent.slice(0, -1) === method) {
                    const sectionId = sections[j].getAttribute("id");
                    window.location.href = `#${sectionId}`;
                    break;
                }
            }
        });
    }
</script>