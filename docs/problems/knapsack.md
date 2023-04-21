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

$$C(\textbf{x}, \vec{y}) = -\sum_{i=0}^n v_i x_i + P\Big(\sum_{i=1}^n w_i x_i + \sum_{k=0}^k 2^i y_i- W\Big)^2,$$
where $\textbf{x} = \{0,1\}^n, \ \ \vec{y} = \{0, 1\}^k$. 

This cost function can be converted in the form of an Ising formulation (so that variables take values in $\{-1, 1\}$) in order to be used with the QAOA algorithm (see [what-is-a-qubo](/problems/what-is-a-qubo)).

## Knapsack in OpenQAOA

TODO