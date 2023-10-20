# Number Partitioning

In the Number Partitioning problem, the goal is to find, given a set of $k$ positive numbers $S=\{n_1, \dots , n_k\}$, whether there exists a partition of this set of numbers into two disjoint sets $R$ and $S \setminus R$ such that the sum of the elements in both sets is as close as possible.

For example, given the set of numbers $S=\{1, 2, 3, 6, 10\}$, one can find that there is a perfect number partition since we can take $R=\{2, 3, 6\}$ and $S\setminus R =\{1, 10\}$, so that $|R|=|S\setminus R |=11$.

!!! note "A Kind of Knapsack Problem"
    The Number Partitioning problem can be seen as a special case of the [Knapsack problem](/problems/knapsack), where there are $n$ items with same value, and the weight capacity is $W=\frac12 \sum_{i}^k n_k$.

## The Cost Function

The function to minimize can be defined as

$$
C(\sigma) = \left(\sum_{i=1}^k n_i\sigma_i\right)^2 = \sum_{i, j=1}^kn_in_j\sigma_i\sigma_j,
$$

where $\sigma \in \{-1, 1\}^k$. That is, a variable $\sigma_i$ is attached to each number $n_i$, and the variable's value determines on which side of the partition the number is assigned to. The smallest value $C(\cdot)$ can take is 0, which happens when $\sigma$ is a perfect partition.

Since this formulation is already in terms of Ising variables, it can be used directly in QAOA.

## Number Partitioning in OpenQAOA

The Number Partitioning problem is defined by a set of integers, you can thus define the problem as follows:

```Python
from openqaoa.problems import NumberPartition

int_set = [1,2,3,4,10]

np_prob = NumberPartition(numbers=int_set)
np_qubo = np_prob.qubo
```

We can then access the underlying cost hamiltonian 

```Python
np_qubo.hamiltonian.expression
```

$$
12.0Z_{1}Z_{2} + 130 + 16.0Z_{1}Z_{3} + 20.0Z_{0}Z_{4} + 24.0Z_{2}Z_{3} + 4.0Z_{0}Z_{1} + 40.0Z_{1}Z_{4} + 6.0Z_{0}Z_{2} + 60.0Z_{2}Z_{4} + 8.0Z_{0}Z_{3} + 80.0Z_{3}Z_{4}
$$

You may also check all details of the problem instance in the form of a dictionary:

```Python
> np_qubo.asdict()

{'constant': 130,
 'metadata': {},
 'n': 5,
 'problem_instance': {'n_numbers': 5,
                      'numbers': [1, 2, 3, 4, 10],
                      'problem_type': 'number_partition'},
 'terms': [[0, 1],
           [0, 2],
           [0, 3],
           [0, 4],
           [1, 2],
           [1, 3],
           [1, 4],
           [2, 3],
           [2, 4],
           [3, 4]],
 'weights': [4.0, 6.0, 8.0, 20.0, 12.0, 16.0, 40.0, 24.0, 60.0, 80.0]}
```
