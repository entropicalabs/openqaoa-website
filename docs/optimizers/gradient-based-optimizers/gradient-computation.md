# Gradient computation

When optimizing the parameters in the QAOA we don't know the analytical form of the cost function therefore we need some method to compute the Jacobian $\vec\nabla f(\vec\gamma)$. OpenQAOA offers different methods to compute it. Here you can find a list and a brief description of those:

| Name | Method | Description |
| :--- | :----- | :---------- |
| `finite_difference` | Finite Difference  | Computes the gradient by evaluating the function at points that are slightly perturbed from the current point. |
| `param_shift` | Parameter-shift | Compute exact derivative by evaluating the circuit at two points offset by $\pi/2$ from the parameter value using the parameter-shift rule.|
| `stoch_param_shift` | Stochastic Parameter-shift | Compute derivative by adding random components to the parameter-shift rule |
| `grad_spsa` | Gradient Simultaneous Perturbation Stochastic Approximation | A method for stochastic gradient descent that estimates the gradient using a finite-difference approximation with random perturbations of the parameters. |



## Finite Difference

The finite difference method is a numerical technique for approximating derivatives of a function. Given a function $f(\vec{\gamma})$, we want to approximate its Jacobian $\vec\nabla f(\vec{\gamma})=(\partial_1f, \partial_2f, ..., \partial_nf)$. The finite difference method uses the following formula to compute an approximation of the derivative:

$$ \frac{\partial f(\vec{\gamma})}{\partial\gamma_i} \approx \frac{f(\vec\gamma + \eta\,\vec{e}_i) - f(\vec\gamma - \eta\,\vec{e}_i)}{2\eta} $$

where $\eta$ is a small positive number, often called the step size, and $\vec{e}_i$ is the $i$ base vector: $\vec{e}_i = (0,...,0,1,0,...,0)$, where the $1$ is found the $i\text{-th}$ position. Note that the accuracy of the approximation depends on the choice of $\eta$.

### OpenQAOA example
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


## Parameter-shift
The parameter-shift rule is a technique that allows for the exact computation of gradients for quantum circuits with certain gate sets. It works by exploiting the fact that the derivative of a unitary gate $U(\theta)$ with respect to its parameter $\theta$ can be expressed in terms of the gate itself, read [Schuld et al. (2018)](https://doi.org/10.1103/PhysRevA.99.032331). 

As a quick explanation, if we have a cost function like:

$$ f(\vec\gamma) 
= \langle\psi(\vec\gamma)|\,\mathcal{H}\,|\psi(\vec\gamma)\rangle 
= \langle\psi'(\vec\gamma')|\,\,\mathcal{U}^\dagger(\gamma_j)\mathcal{H}\,\mathcal{U}(\gamma_j)\,|\psi'(\vec\gamma')\rangle,$$

where $\vec\gamma'=(\gamma_1, ..., \gamma_{j-1}, \gamma_{j+1}, ..., \gamma_n)$ and the parametric gate $\mathcal{U}$ has the following shape:

$$ \mathcal{U}(\gamma_j) = \exp{(-i\,r\,\gamma_j\, P)}, $$

where $r$ is a constant, and $P$ is a Pauli gate. Then it can be shown that the partial derivative of $\gamma_i$ can be exactly computed as:

$$ \frac{\partial f(\vec\gamma)}{\partial\gamma_i} 
= r \, \bigg[ f\left(\vec\gamma + \frac{\pi}{4r}\vec{e}_j\right) - f\left(\vec\gamma - \frac{\pi}{4r}\, \vec{e}_j\right) \bigg],$$

where $\vec{e}_j$ is the $j$ base vector: $\vec{e}_j = (0,...,0,1,0,...,0)$, where the $1$ is found the $j\text{-th}$ position. 

In the QAOA, it turns out that the cost function can be expressed as above for all the gates when we are using the extended parameters. This means that we can use the parameter-shift rule to compute the Jacobian when we are solving QAOA with extended parameters. However, we can also use standard parameters if we convert the standard parameters to extended before the gradient computation and after it we convert them back. To learn about the various parametrizations, please refer to the [Parametrization and Initialization page](../../parametrization/parametrization.md).

### OpenQAOA example
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

## Stochastic Parameter-shift

The Stochastic Parameter-Shift method is a gradient approximation technique that randomly samples a fixed number of parameters to compute the gradient using the Parameter-Shift method explained above. With this method, some of the partial derivatives will not be computed and will be left as zero. This method is only applicable to QAOA with standard parameterization since the gradient of each standard parameter is a combination of gradients of some extended parameters.

### OpenQAOA example
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

## Gradient Simultaneous Perturbation Stochastic Approximation
The Gradient Simultaneous Perturbation Stochastic Approximation is a method for stochastic gradient descent that estimates the gradient using a finite-difference approximation with random perturbations of the parameters. 

The parameters are perturbed by a random vector that takes values of +1 or -1 with equal probability, and the cost function is evaluated at two points: one where the parameters are perturbed in the positive direction, and another where they are perturbed in the negative direction. The gradient is then estimated using a finite-difference approximation:

$$ \vec\nabla f(\vec\gamma) \approx \frac{f(\vec\gamma + c \vec\Delta) - f(\vec\gamma - c \vec\Delta)}{2c} \odot  \vec\Delta $$

where $c$ is a constant that determines the step size, and $\vec\Delta$ is the perturbation vector: $\vec\Delta = (\delta_1, \delta_2, ..., \delta_n)$ where $\delta_i \in \{-1, 1\}$ are randomly generated.

The primary benefit of Gradient SPSA is that it requires only two evaluations of the cost function to estimate the Jacobian, unlike the finite difference method, which requires two evaluations for each parameter.

### OpenQAOA example
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
    const sections = document.getElementsByTagName("h2");

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
