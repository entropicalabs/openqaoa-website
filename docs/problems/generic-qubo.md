# QUBO

QAOA can solve binary optimization problems known as QUBOs. QUBO stands for _Quadratic unconstrained binary optimization_, and a QUBO problem represent, loosely speaking, a binary problem with at most quadratic terms. In general any optimization problem can be cast as a binary problem, but not all of them will be quadratic in the resulting binary variables. For a very nice paper showcasing most common QUBOs please check out Andrew Lucas's paper [Ising formulations of many NP problems](https://arxiv.org/abs/1302.5843)

## Binary optimization

Binary optimization is a type of combinatorial optimization problem in which the variables are limited to two values. For example, a variable $x$ can be either 0 or 1. A binary optimisation problem then reflect the effort of minimizing a cost function $C(\vec{x})$ of $N$ variables.

In its most general form a binary optimization problem can be written as

$$
\textit{min}_{\vec{x}} \{ \text{Cost}(\vec{x}) |x \in \{0,1\}^N \}
$$

Where $\text{Cost}(x)$ is the usually referred to as the cost function of the problem.

!!! note "{0,1} or {-1,+1}"
    Both choices are valid encodings for binary problems. QAOA, works best with the $\{-1, +1\}$ encoding (also known as the Ising encoding). 

## How to write a QUBO

For our needs, we will be limiting ourself to Quadratic Unconstrained Binary Optimization (QUBO) problems. These problems are binary optimization problems which features at most most polynomial of order 2.


The most general formulation of a QUBO is then

$$
\sum_i^n h_i x_i + \sum_{i,j} J_{i,j} x_ix_j,
$$

where $x_i \in{\pm1}$ are the binary variables and $h$ and $J$ represents the coefficients for the linear and quadratic coefficients respectively.

The following equation shows an example of a QUBO problem

$$
3x_1 + 2x_2 + 6x_1x_2 + 4x_1x_2 + 5x_1x_3
$$

as you can see, we only have linear and quadratic terms in $x$.

## From QUBOs to QAOA

As explained in the [what-is-the-qaoa](../what-is-the-qaoa.md) page, the QUBO can be directly cast into a Cost Hamiltonian. There are two important points worth mentioning explicitly:

1. The QUBO can be represented by a weighted graph $G(V,E)$ defined by $V$ vertexes and $E$ edges, where the linear part of the QUBO represent the weight of the vertexes and the quadratic is the weight associated to the edges
2. The QAOA cost function should be defined as an Ising model, that is $\{ -1, +1\} $



## QUBOs in OpenQAOA

A QUBO in OpenQAOA is simply described by two lists, and a number for the constant value. Using the example above, we have

$$ 3x_1 + 2x_2 + 6x_1x_2 + 4x_1x_2 + 5x_1x_3 $$ we have

```Python
terms = [[0], [1], [0,1], [1,2], [0,2]]
weights = [3, 2, 6, 4, 5]
qubo = QUBO(n = 3, terms=terms, weights=weights)
```

!!! note "Python index"
    Remember that in python indexes start at zero!

Creating the OpenQAOA QUBO is simple

```Python
from openqaoa.problems import QUBO
qubo = QUBO(n=3, terms=terms, weights=weights)
```

if we then check the dictionary representation of our `qubo` we get

```Python
{'terms': [[0], [1], [0, 1], [1, 2], [0, 2]],
 'weights': [3.0, 2.0, 6.0, 4.0, 5.0],
 'constant': 0,
 'n': 3,
 'problem_instance': {'problem_type': 'generic_qubo'},
 'metadata': {}}
```

QUBOs can also be nicely represented as weighted graphs: the `terms` represents the list of vertexes (the linear terms) and the edges (the quadratic terms), and the `weight` represent the weight of each vertex or edge.

This is simple to achieve within OpenQAOA. Simply type

```Python
import networkx as nx

networkx_graph = utilities.graph_from_hamiltonian(qubo.hamiltonian)
nx.draw(networkx_graph, with_labels=True, node_color='yellow')
```

![TriangleQubo](/img/triangle_qubo.png)

We also obtain the underlying cos hamiltonian by executing

```Python
qubo.hamiltonian.expression
```

$$
2.0Z_{1} + 3.0Z_{0} + 4.0Z_{1}Z_{2} + 5.0Z_{0}Z_{2} + 6.0Z_{0}Z_{1}
$$
