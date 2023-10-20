# Knapsack

The 0/1 Knapsack problem can be described as follows. We are given a set of $n$ items, labelled by the index $i$ for $i = 1,..,n$. The $i^{th}$ item has value $v_i$ and weight $w_i$, and the goal is to choose a subset of the items such that the sum of the values of the chosen items is maximized, subject to the constraint that the sum of their weights does not exceed a specified capacity $W$. The quantity $W$ is typically called the **weight capacity** or the **knapsack capacity**. Clearly, for the problem to be non-trivial it must be the case that $W$ is less than the sum of the weights of all items, otherwise we would choose the entire set.

## The Cost Function

Mathematically, the problem may be stated as follows:

$$\begin{equation}
    \begin{split}
    &\text{maximise} \ \ \sum_{i=1}^n v_i x_i \\ 
    & \text{subject to} \ \ \sum_{i=1}^n w_i x_i \leq W, \ \ \ \ x_i = \{0,1\},
    \end{split}
\end{equation}$$

where a variable with value 1 indicates that the corresponding item is chosen to be in the knapsack.

In order to turn such a formulation into a QUBO one, we must first make the inequality constraint an equality. This is done by introducing extra variable (sometimes called "slack variables") $y_1, \dots, y_k$ where $k=\lfloor\log W\rfloor +1$, so that one can rewrite the inequality constraint as $\sum_{i=1}^n w_i x_i + \sum_{i=0}^{k}2^iy_i = W$. This way, if the sum of the weights of the chosen items is less than or equal to $W$ (which would mean the inequality constraint is satisfied), there is then some configuration of the slack variables that will allow the equality to be satisfied. In fact, the slack variables form the binary representation of the value needed to meet equality, which is why $\sum_{i=0}^{k}2^iy_i$ appears. The new problem is thus

$$\begin{equation}
    \begin{split}
    &\text{maximise} \ \ \sum_{i=1}^n v_i x_i \\ 
    & \text{subject to} \ \ \sum_{i=1}^n w_i x_i + \sum_{i=0}^{k}2^iy_i = W, \\ 
    & x_i = \{0,1\}, \ \ y_i = \{0, 1\}.
    \end{split}
\end{equation}$$

Now that we have an equality constraint, the corresponding QUBO formulation is retrieved by turning the constraint into a so-called quadratic penalty. That is, the cost function is

$$
C(\textbf{x}, \vec{y}) = -\sum_{i=0}^n v_i x_i + P\Big(\sum_{i=1}^n w_i x_i + \sum_{k=0}^k 2^i y_i- W\Big)^2,
$$

where $\textbf{x} = \{0,1\}^n, \ \ \vec{y} = \{0, 1\}^k$. 

This cost function can be converted in the form of an Ising formulation (so that variables take values in $\{-1, 1\}$) in order to be used with the QAOA algorithm (see [what-is-a-qubo](/problems/what-is-a-qubo)).

## Knapsack in OpenQAOA

The knapsack problem can be defined by the value of each item, the weight of each item, the total capacity of the knapsack and the penalty associated with overloading the knapsack. We can create the Knapsack problem easily:

```Python
from openqaoa.problems import Knapsack

knapsack_prob = Knapsack.random_instance(n_items=4, seed=42)
knapsack_qubo = knapsack_prob.qubo
```

We can then access the underlying cost hamiltonian 

```Python
knapsack.qubo.hamiltonian.expression
```

$$
-17.0Z_{5} - 18.0Z_{0} - 35.5Z_{3} - 36.0Z_{1} - 52.5Z_{4} - 53.0Z_{6} - 72.0Z_{2} + 116.0 + 12.0Z_{0}Z_{2} + 12.0Z_{1}Z_{3} + 12.0Z_{2}Z_{5} + 18.0Z_{1}Z_{4} + 18.0Z_{1}Z_{6} + 18.0Z_{3}Z_{4} + 18.0Z_{3}Z_{6} + 24.0Z_{1}Z_{2} + 24.0Z_{2}Z_{3} + 27.0Z_{4}Z_{6} + 3.0Z_{0}Z_{5} + 36.0Z_{2}Z_{4} + 36.0Z_{2}Z_{6} + 6.0Z_{0}Z_{1} + 6.0Z_{0}Z_{3} + 6.0Z_{1}Z_{5} + 6.0Z_{3}Z_{5} + 9.0Z_{0}Z_{4} + 9.0Z_{0}Z_{6} + 9.0Z_{4}Z_{5} + 9.0Z_{5}Z_{6}
$$

You may also check all details of the problem instance in the form of a dictionary:

```Python
> knapsack_qubo.asdict()

{'constant': 116.0,
 'metadata': {},
 'n': 7,
 'problem_instance': {'n_items': 4,
                      'penalty': 6,
                      'problem_type': 'knapsack',
                      'values': [1, 3, 2, 2],
                      'weight_capacity': 5,
                      'weights': [2, 3, 1, 3]},
 'terms': [[0, 1],
           [0, 2],
           [1, 2],
           [3, 4],
           [3, 5],
           [3, 6],
           [4, 5],
           [4, 6],
           [5, 6],
           [0, 3],
           [0, 4],
           [0, 5],
           [0, 6],
           [1, 3],
           [1, 4],
           [1, 5],
           [1, 6],
           [2, 3],
           [2, 4],
           [2, 5],
           [2, 6],
           [0],
           [1],
           [2],
           [3],
           [4],
           [5],
           [6]],
 'weights': [6.0, 12.0, 24.0, 18.0, 6.0, 18.0, 9.0, 27.0, 9.0, 6.0, 9.0, 3.0, 9.0, 12.0, 18.0, 6.0, 18.0, 24.0, 36.0, 12.0, 36.0, -18.0, -36.0, -72.0, -35.5, -52.5, -17.0, -53.0]}
```