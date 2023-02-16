# Maximum Cut

A maximum cut of a graph is a partition of its vertices into two sets, $S$ and $T$, such that the number of edges between them is maximized. The task of finding such a cut is known as the MaxCut Problem.


## The Cost Function

Given a graph $G=(V, E)$, the cost function to minimize for the MaxCut problem is given by

$$
C(\textbf{x}) = -\sum_{(i, j)\in E} w_{ij} (x_i+x_j - 2x_i x_j )
$$

where $w_{ij}$ is the weight corresponding to the edge $(i,j) \in E$, and $\textbf{x}\in \{0, 1\}^{|V|}$ is the binary variable indicating whether node $i$ is in set $S$ or $T$. This works because each term $x_i+x_j - 2x_i x_j$ is equal to 1 if an edge is in the cut, and 0 otherwise.

Note that the equivalent formulation in terms of Ising variables is the following

$$
C(\textbf{x}) = -\sum_{(i, j)\in E} w_{ij} x_ix_j,
$$

where this time $\textbf{x}\in \{-1, 1\}^{|V|}$.

## MaxCut in OpenQAOA

MaxCut being a graph problem, you can leverage the popular `networkx` to easily create a variety of graphs. For example, an Erdös-Rényi graph can be instantiated with

```Python
import networkx as nx

G = nx.generators.fast_gnp_random_graph(n=6, p=0.6, seed=42)
```

OpenQAOA has a nice wrapper to plot networkx graphs

```Python
from openqaoa.utilities import plot_graph
plot_graph(G)
```

![seed_42_graph](/img/seed_42_graph.png)

Once the graph is defined, creating a MaxCut problem class requires only a few lines of code:

```Python
from openqaoa.problems.problem import MaximumCut

maxcut_prob = MaximumCut(G)
maxcut_qubo = maxcut_prob.get_qubo_problem()
```

We can then access the underlying cost hamiltonian 

```Python
maxcut_qubo.hamiltonian.expression
```

$$
1.0Z_{0}Z_{2} + 1.0Z_{0}Z_{3} + 1.0Z_{0}Z_{4} + 1.0Z_{1}Z_{2} + 1.0Z_{1}Z_{3} + 1.0Z_{1}Z_{5} + 1.0Z_{2}Z_{4} + 1.0Z_{2}Z_{5} + 1.0Z_{3}Z_{5} + 1.0Z_{4}Z_{5}
$$


```Python
> maxcut_qubo.asdict()

{'constant': 0,
'metadata': {},
'n': 6,
'problem_instance': {'G': {'directed': False,
                            'graph': {},
                            'links': [{'source': 0, 'target': 2},
                                    {'source': 0, 'target': 3},
                                    {'source': 0, 'target': 4},
                                    {'source': 1, 'target': 2},
                                    {'source': 1, 'target': 3},
                                    {'source': 1, 'target': 5},
                                    {'source': 2, 'target': 4},
                                    {'source': 2, 'target': 5},
                                    {'source': 3, 'target': 5},
                                    {'source': 4, 'target': 5}],
                            'multigraph': False,
                            'nodes': [{'id': 0},
                                    {'id': 1},
                                    {'id': 2},
                                    {'id': 3},
                                    {'id': 4},
                                    {'id': 5}]},
                    'problem_type': 'maximum_cut'},
'terms': [[0, 2],
        [0, 3],
        [0, 4],
        [1, 2],
        [1, 3],
        [1, 5],
        [2, 4],
        [2, 5],
        [3, 5],
        [4, 5]],
'weights': [1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0]}
```