# Shortest Path Problem

## Mathematical Formulation
An encoding of the problem can be achieved with $|V| + |E| - 2$ binary variables representing each state $x$ in the state space \cite{krauss2020solving} (We will use $x_i$'s and $x_{ij}$'s interchangeably with $x$ to denote states when the context is clear). 

The first $|V|-2$ variables $x_1, x_2, ..., x_{|V|-2}$ correspond to the nodes in the graph excluding the source and destination nodes, while the remaining $|E|$ variables $x_{ij}$, where $(i,j) \in E$, corresponds to the edges.

With this encoding, we consider objectives that take the following forms:

- **1 - Node cost**: Associates each node in the graph with a cost, depending on the node's weight $V_i$.

$$
E_{node}(x) = \sum_{i \in V} V_{i} x_{i}
$$    

- **2 - Edge cost**: Associates each edge in the graph with a cost, depending on the edge's weight $E_{ij}$. 

$$
E_{edge}(x) = \sum_{(i,j) \in E} E_{ij} x_{ij}
$$


With only one objective (for instance, the minimisation of the distance between two points of a graph, which is an edge cost), the problem reduces to a single-objective shortest path problem which can be solved efficiently in polynomial time by Djikstra's well-known algorithm, which takes $O(|E|+|V|\log|V|)$ steps. The presence of multiple objectives constitute a Multi Objective Optimisation Problem (MOOP).

The cost and the edge equations above allow us to compute the cost vector associated with a state $x$. However, of the $2^{|V| + |E|-2}$ possible states, not all of them represent actual paths.

A valid path is one that

1. starts from a specified source node $s$, 
2. ends at a specified destination node $d$, and 
3. has no broken links or branches along the path from $s$ to $d$. 

States that satisfy these criteria are called _feasible_, and _infeasible_ otherwise. These 3 constraints can be enforced as quadratic penalty terms added to our cost function, so that infeasible solutions have higher total energies than feasible ones:

- **3 : Source constraint**: Penalises paths that do not have exactly one edge connected to the source node:
    
$$
E_s(x) = -x^2_s + (x_s - \sum_j x_{sj})^2,
$$

&ensp;where $s$ is the index of the source node, and the sum is over all edges that are connected to node $d$.  This term has a minimum value of $-1$, which occurs for states where the source node is used ($x_s = 1$), and there is only one way of leaving the source node (the bracketed term is equal to zero).

- **4 : Destination constraint**: Penalises paths that do not have exactly one edge connected to the destination node:
    
$$
E_d(x) = -x^2_d + (x_d - \sum_j x_{dj})^2,
$$

&ensp;where $d$ is the index of the destination node, and the sum is over all edges that are connected to node $s$. This term has a minimum value of $-1$, similar to the source constraint.

- **5 : Path constraint**: Penalises paths that do not have exactly 2 edges connected to intermediate nodes: $E_{path} = \sum_{i \in V} E_i$, such that for each intermediate node $i$:
    
$$
E_i(x) = (2x_i - \sum_j x_{ij})^2,
$$

&ensp;where the sum is over all edges that are connected to node $i$. This has a minimum value of 0, which occurs for states where for all intermediate nodes used (for which $x_i = 1$), the node degree is equal to 2.

A solution of the multi-objective shortest path problem is then a path $x$ that achieves the minimum of all the penalties  and is Pareto-optimal with respect to the node and edge costs.