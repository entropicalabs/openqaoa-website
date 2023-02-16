# Shortest Path Problem

Given an undirected graph $G(V,E)$, the shortest path problem for a given source $s \in V$ and destination node $d \in V$ seeks a path between them that has minimal distance. We consider the weighted variant of the problem, where nodes and edges of $G$ are assigned node and edge weights. The distance of a path is then equal to the sum of node and edge weights along the path.

## Mathematical Formulation

A QUBO encoding of the (weighted) shortest path problem can be constructed with $|V| + |E| - 2$ binary variables. This is achieved by using linear terms to represent the node and edge weights, and quadratic penalty terms to penalize states that not valid paths, where a valid path is one that:

1.  starts from a specified source node $s$,
2.  ends at a specified destination node $d$, and
3.  has no broken links or branches along the path from $s$ to $d$.

In terms of binary variables $\{0, 1\}$, the first $|V|-2$ variables (denoted as $x_i$, where $i \in V$ corresponds to a node) correspond to nodes in the graph excluding the source and destination nodes, while the remaining $|E|$ variables (denoted as $x_{ij}$, where $(i,j) \in E$ represents an edge that is present in $G$ between nodes $i$ and $j$) represent the edges $E$. A state $\textbf{x}$ is then denoted as $\textbf{x} = (x_1,..., x_{|V|-2},...,x_{ij},...)$. Note that this encoding implicitly specifies the starting and ending nodes, since they are always in the path.

!!! note "Binary encoding"
    In this case it is much easier to write the cost function in terms onf $\{0,1\}$ binary variables, and convert them to OpenQAOA ising encoding only at the end!


With this encoding, the QUBO cost function $C(\textbf{x})$ can be written in the form: 

$$
C(\textbf{x}) = C_w(\textbf{x}) + C_P(\textbf{x}). 
$$

$C_w(\textbf{x})$ contains linear terms that encode the node and edge weights $w_i$ and $w_{ij}$ respectively:

$$
C_w(\textbf{x})= \sum_{i \in V} w_i x_i+ \sum_{(i,j) \in E} w_{ij} x_{ij},
$$

while 

$$
C_P(\textbf{x}) = C_s(\textbf{x}) + C_d(\textbf{x}) + C_{path}(\textbf{x})
$$ 

contains linear and quadratic terms that enforce the conditions (1), (2), and (3) above. Explicitly, they are:

-   **1 : Source constraint**: Penalises paths that do not have exactly one edge connected to the source node with

$$
C_s(\textbf{x}) = -x^2_s + (x_s - \sum_j x_{sj})^2,
$$

where $x_s = 1$, and the sum is over all edges that are connected to node $s$. This term has a minimum value of $-1$, which occurs for states where there is only one edge connected to the source node.

-   **2 : Destination constraint**: Penalises paths that do not have exactly one edge connected to the destination node with

$$
C_d(\textbf{x}) = -x^2_d + (x_d - \sum_j x_{dj})^2,
$$

where $x_d = 1$, and the sum is over all edges that are connected to node $d$. This term has a minimum value of $-1$, which occurs for states where there is only one edge connected to the destination node.

-   **3 : Path constraint**: Penalises paths that do not have exactly 2 edges connected to intermediate nodes with

$$
C_{path}(\textbf{x}) = \sum_{i \in V} C_i(\textbf{x})
$$

such that for every node $i \in V$:

$$
C_i(\textbf{x}) = (2x_i - \sum_j x_{ij})^2,
$$

where the sum is over all edges that are connected to node $i$. This has a minimum value of 0, which occurs for states where all nodes selected have exactly 2 edges connected to it.

States that minimize $C_P(\textbf{x})$ then constitute valid paths that start at $s$ and end at $d$.

!!! note
    In OpenQAOA, where we work with Ising variables $\sigma_i\in\{-1,1\}$, a transformation of variables $x_i \rightarrow (1-\sigma_i)/2$ transforms the cost function $C(\textbf{x})$ to  the QAOA cost Hamiltonian $H$.


## The Shortest Path Problem in OpenQAOA

