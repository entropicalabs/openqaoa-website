# Maximum Cut

A maximum cut of a graph is a partition of its vertices into two complementary sets, $S$ and $T$, such that the number of edges between them is maximized. This problem, known as the Max-Cut Problem, is the task of finding such a cut.



## MaxCut in OpenQAOA

The cost function for a MaxCut is given by

$$
H_C = \frac{1}{2} \sum_{\langle i, j\rangle} w_{ij} (1 - Z_i Z_j )
$$

where $\langle i, j\rangle$ denotes qubits connected by an edge, $w_{ij}$ is the weight corresponding to the edge $ij$, and $Z_i$ is the Pauli-Z operator acting on the i-th qubit.

## MaxCut from Graphs

Max cut is a graph problem, and as such you can leverage the popular `networkx` to create the graph. 

```Python
import networkx as nx

g = nx.generators.fast_gnp_random_graph(n=6, p=0.6, seed=42)
```

OpenQAOA has a nice wrapper to plot networkx graphs

```Python
from openqaoa.utilities import plot_graph
plot_graph(g)
```

![seed_42_graph](/img/seed_42_graph.png)

Then, creating a Max Cut problem class is a one liner:

```Python
maxcut_prob = MaximumCut(g)

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