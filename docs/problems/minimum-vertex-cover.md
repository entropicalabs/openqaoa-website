# Minimum Vertex Cover

The Minimum Vertex Cover (MVC) problem is a common constrained optimization problem. Given a graph $G=(V, E)$, the goal is to find a set of vertices $S \subseteq V$ of minimum cardinality with the constraint that it "covers" all edges in $G$. For a given $S$, an edge $(i, j) \in E$ is said to be covered if at least one of its constituent vertices is part of $S$.

## The Cost Function

The corresponding function to minimize can be derived as:
$$\begin{equation*}
    C(\textbf{x}) = \sum_{i \in V}x_i + P\sum_{(i, j) \in E}\left(1-x_i\right)\left(1-x_j\right),
\end{equation*}$$
where $\boldsymbol{x}\in \{0, 1\}^{|V|}$ and $P>1$ is a parameter controlling the strength of penalization of configurations which are not covers. 

Interpreting a variable $x_i$ being equal to 1 as being part of the vertex cover, the first summation counts the size of the selected cover while the second summation ensures that the selected cover indeed covers all edges. Hence, minimizing it ensures we find the minimum vertex cover.

Note that this cost function can be conveniently converted to the form of an Ising problem in order to be usable with QAOA (see [what-is-a-qubo](/docs/problems/what-is-a-qubo.md)).

## Minimum Vertex Cover in OpenQAOA

MVC being a graph problem, you can leverage the popular `networkx` to easily create a variety of graphs. For example, an Erdös-Rényi graph can be instantiated with

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

Once the graph is defined, creating a MVC problem class requires only a few lines of code:

```Python
from openqaoa.problems.problem import MinimumVertexCover

mvc_prob = MinimumVertexCover(G)
mvc_qubo = maxcut_prob.qubo
```

We can then access the underlying cost hamiltonian 

```Python
mvc_qubo.hamiltonian.expression
```

TBA: OUTPUT OF THIS command
