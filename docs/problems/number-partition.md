# Number Partitioning

In the Number Partitioning problem, the goal is to find, given a set of $k$ positive numbers $S=\{n_1, \dots , n_k\}$, whether there exists a partition of this set of numbers into two disjoint sets $R$ and $S \setminus R$ such that the sum of the elements in both sets is as close as possible.

For example, given the set of numbers $S=\{1, 2, 3, 6, 10\}$, one can find that there is a perfect number partition since we can take $R=\{2, 3, 6\}$ and $S\setminus R =\{1, 10\}$, so that $|R|=|S\setminus R |=11$.

!!! note "A Kind of Knapsack Problem"
    The Number Partitioning problem can be seen as a special case of the [Knapsack problem](/docs/problems/knapsack.md), where there are $n$ items with same value, and the weight capacity is $W=\frac12 \sum_{i}^k n_k$.

## The Cost Function

The function to minimize can be defined as

$$C(\textbf{x}) = \left(\sum_{i=1}^k n_ix_i\right)^2 = \sum_{i, j=1}^kn_in_jx_ix_j,$$

where $\textbf{x}\in \{-1, 1\}^k$. That is, a variable $x_i$ is attached to each number $n_i$, and the variable's value determines on which side of the partition the number is assigned to. The smallest value $C(\cdot)$ can take is 0, which happens when $\textbf{x}$ is a perfect partition.

Since this formulation is already in terms of Ising variables, it can be used directly in QAOA.

## Number Partitioning in OpenQAOA

TODO
