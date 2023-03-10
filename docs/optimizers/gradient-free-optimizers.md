# Gradient-free optimizers

In OpenQAOA offers the possibility to use different gradient-free optimizers to solve QAOA. You can read more about optimization algorithms in the [optimizers landing page](/optimizers). The optimizers available are those that are included in the library `scipy.optimize.minimize`, please refer to [their documentation](https://docs.scipy.org/doc/scipy/reference/generated/scipy.optimize.minimize.html) to learn more about them. 

!!! info "List of `scipy.optimize.minimize` methods"

    Here you can find a list of the name of the current available optimizers: `nelder-mead`, `powell`, `cg`, `bfgs`, `tnc`, `cobyla`, `slsqp`, `trust-constr`, `dogleg`, `trust-ncg`, `trust-exact`, `trust-krylov`.

    Note that this list is subject to changes, as new optimizers could be implemented in future versions of the library.

We will now provide a brief overview and examples of two optimization methods that are commonly used for QAOA: COBYLA and Nelder-Mead. These methods have shown to be effective for optimizing QAOA, and by understanding their behavior and performance, we can gain insights into how to choose the most suitable optimization method for a given problem.


## COBYLA 
COBYLA (Constrained Optimization BY Linear Approximation) is a gradient-free optimization algorithm that uses a linear approximation of the function in the neighborhood of the current point to determine the next point to evaluate. The algorithm is based on the idea of "trust regions," which means that the algorithm only considers changes to the variables that are within a certain "trust region" around the current point.

COBYLA is particularly useful when the objective function is expensive to evaluate or is non-differentiable. It is also well-suited for problems with constraints, as it can handle both equality and inequality constraints.

The COBYLA algorithm has two important parameters: rhobeg and maxiter. rhobeg determines the size of the initial "trust region" around the starting point, and maxiter specifies the maximum number of iterations the algorithm can perform before terminating. In general, smaller values of rhobeg will lead to more accurate results, but may also increase the computational cost of the algorithm.

Here's an example of how to use COBYLA in OpenQAOA:

```Python hl_lines="6 7 8 9 10 11 12 13"
from openqaoa import QAOA 

# create the QAOA object
q = QAOA()

# set optimizer and properties
q.set_classical_optimizer(
    method='cobyla', 
    maxiter=200,
    optimizer_options=dict(
        rhobeg=0.5,
    )
)

# compile and optimize using the chosen optimizer
q.compile(problem)
q.optimize()
```

In this example, we set the method argument to `cobyla`, and then we set the maximum number of iterations to 200 and we customize the optimizer using the optimizer_options argument. In this case, we set we set `rhobeg` to 0.5.

## Nelder-Mead
The Nelder-Mead optimizer is another widely used gradient-free optimization algorithm, which is based on the idea of a simplex, a geometrical figure that in $n$-dimensions consists of $n+1$ vertices. The vertices of the simplex are iteratively adjusted based on the function evaluations to find the minimum.

At each iteration, the algorithm computes the function value at the $n+1$ vertices of the simplex. Then, it sorts the vertices by function value, and computes the center of mass of the $n$ best vertices. It then applies a reflection operation, which is similar to gradient descent, but in the direction from the worst to the center of mass. The algorithm then checks the function value at this new vertex. If it is better than the best vertex, the algorithm tries to expand further in this direction. Otherwise, it contracts the simplex in the direction of the worst vertex. If the contraction step still does not improve the function value, the algorithm shrinks the simplex around the best vertex.

In OpenQAOA, the Nelder-Mead optimizer can be used with the following code:

```Python hl_lines="6-14"
from openqaoa import QAOA 

# create the QAOA object
q = QAOA()

# set optimizer and properties
q.set_classical_optimizer(
    method='nelder-mead',
    maxiter=1000,
    optimizer_options=dict(
        xatol=1e-8,
        fatol=1e-8,
    )
)

# compile and optimize using the chosen optimizer
q.compile(problem)
q.optimize()
```

In the code above, we set the method argument to `nelder-mead`, and then we set the maximum number of iterations to 1000 and we customize the optimizer using the optimizer_options argument. In this case, we set tolerances for the optimization to stop. The `xatol` parameter sets the absolute tolerance for the position, while the `fatol` parameter sets the absolute tolerance for the function value.