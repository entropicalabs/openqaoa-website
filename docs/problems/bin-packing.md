# The Bin Packing Problem

The Bin Packing Problem (BPP) involves the efficient packing of a collection of items into the minimum number of bins, where each item has an associated weight and the bins have a maximum weight capacity. This problem finds applications in various real-world scenarios, including truck loading with weight restrictions, container scheduling, FPGA chip design among others. The BPP is classified as an NP-hard problem due to its computational complexity. 

## The cost function

The problem can be formulated as follows, minimize the total number of bins used given by the following cost function:

$$\begin{equation}
\min \sum_{j=0}^{m-1} y_j\tag{1},
\end{equation}$$

subject to the following constraints. Each bin's weight capacity should not be exceeded

$$\begin{equation}
\sum_{i=0}^{n-1} w_i x_{ij} \le B y_j \quad \forall j=0,...,m-1\tag{2},
\end{equation}$$

and each item can only be assigned to one bin

$$\begin{equation}
\sum_{j=0}^{m-1} x_{ij} = 1 \quad \forall i = 0, ..., n-1.\tag{3}
\end{equation}$$

Binary variables indicating item-bin assignments and bin utilization

$$\begin{equation}
x_{ij} \in {0,1} \quad \forall i=0,..,n-1 \quad \forall j=0,..,m-1,\tag{4}
\end{equation}$$

$$\begin{equation}
y_j \in {0,1} \quad \forall j=0,..,m-1\tag{6}
\end{equation}$$

In the above equations, $n$ represents the number of items (nodes), $m$ represents the number of bins, $w_{i}$ is the weight of the $i$-th item, $B$ denotes the maximum weight capacity of each bin, and $x_{ij}$ and $y_j$ are binary variables representing the presence of item $i$ in bin $j$ and the utilization of bin $j$, respectively. The objective function in Eq.(1) aims to minimize the number of bins used, while Eq.(2) enforces the constraint on bin weight capacity. Eq.(3) ensures that each item is assigned to only one bin, and Eqs.(4) and (5) define the binary nature of variables $x_{ij}$ and $y_j$.

## Bin Packing in OpenQAOA

The bin packing problem can be defined by the number of items, the number of bins, the range of weight of the items and the weight capacity of the bins. We can create the Bin Packing problem easily:

```Python
import numpy as np
from openqaoa.problems import BinPacking

n_items = 3 # number of items
n_bins = 2 # maximum number of bins the solution will be explored on 
min_weight = 1 # minimum weight of the items
max_weight = 3 # maximum weight of the items
weight_capacity = 5 # weight capacity of the bins
weights = np.random.default_rng(seed=1234).integers(low=min_weight, high=max_weight, size=n_items) # random instance of the problem

bpp = BinPacking(weights, weight_capacity, n_bins=n_bins, simplifications=False)
bpp_qubo = bpp.qubo
```

We can then access the underlying cost hamiltonian

```Python
bpp.qubo.hamiltonian.expression
```

$$
-15.0Z_{0}Z_{10} - 15.0Z_{0}Z_{2} - 15.0Z_{0}Z_{4} - 15.0Z_{0}Z_{6} - 15.0Z_{0}Z_{9} - 15.0Z_{1}Z_{12} - 15.0Z_{1}Z_{13} - 15.0Z_{1}Z_{3} - 15.0Z_{1}Z_{5} - 15.0Z_{1}Z_{7} - 18.0Z_{10} - 18.0Z_{12} - 18.0Z_{13} - 18.0Z_{2} - 18.0Z_{3} - 18.0Z_{4} - 18.0Z_{5} - 18.0Z_{6} - 18.0Z_{7} - 18.0Z_{9} - 7.5Z_{0}Z_{8} - 7.5Z_{1}Z_{11} - 9.0Z_{11} - 9.0Z_{8} + 1.5Z_{2}Z_{3} + 1.5Z_{4}Z_{5} + 1.5Z_{6}Z_{7} + 128.5 + 3.0Z_{11}Z_{12} + 3.0Z_{11}Z_{13} + 3.0Z_{2}Z_{8} + 3.0Z_{3}Z_{11} + 3.0Z_{4}Z_{8} + 3.0Z_{5}Z_{11} + 3.0Z_{6}Z_{8} + 3.0Z_{7}Z_{11} + 3.0Z_{8}Z_{10} + 3.0Z_{8}Z_{9} + 44.5Z_{0} + 44.5Z_{1} + 6.0Z_{12}Z_{13} + 6.0Z_{2}Z_{10} + 6.0Z_{2}Z_{4} + 6.0Z_{2}Z_{6} + 6.0Z_{2}Z_{9} + 6.0Z_{3}Z_{12} + 6.0Z_{3}Z_{13} + 6.0Z_{3}Z_{5} + 6.0Z_{3}Z_{7} + 6.0Z_{4}Z_{10} + 6.0Z_{4}Z_{6} + 6.0Z_{4}Z_{9} + 6.0Z_{5}Z_{12} + 6.0Z_{5}Z_{13} + 6.0Z_{5}Z_{7} + 6.0Z_{6}Z_{10} + 6.0Z_{6}Z_{9} + 6.0Z_{7}Z_{12} + 6.0Z_{7}Z_{13} + 6.0Z_{9}Z_{10}
$$

You  may also chack all details of the problem isntance in the form of a dictionary

```Python
> bpp_qubo.asdict()

{'constant': 128.5,
 'metadata': {},
 'n': 8,
 'problem_instance': {'method': 'slack',
                      'n_bins': 2,
                      'n_items': 3,
                      'penalty': [],
                      'problem_type': 'bin_packing',
                      'simplifications': False,
                      'solution': {'x_0_0': -1,
                                   'x_0_1': -1,
                                   'x_1_0': -1,
                                   'x_1_1': -1,
                                   'x_2_0': -1,
                                   'x_2_1': -1,
                                   'y_0': -1,
                                   'y_1': -1},
                      'weight_capacity': 5,
                      'weights': [2, 2, 2]},
 'terms': [[2, 3],
           [4, 5],
           [6, 7],
           [0, 2],
           [0, 4],
           [0, 6],
           [0, 8],
           [0, 9],
           [0, 10],
           [2, 4],
           [2, 6],
           [8, 2],
           [9, 2],
           [2, 10],
           [4, 6],
           [8, 4],
           [9, 4],
           [10, 4],
           [8, 6],
           [9, 6],
           [10, 6],
           [8, 9],
           [8, 10],
           [9, 10],
           [1, 3],
           [1, 5],
           [1, 7],
           [1, 11],
           [1, 12],
           [1, 13],
           [3, 5],
           [3, 7],
           [3, 11],
           [3, 12],
           [3, 13],
           [5, 7],
           [11, 5],
           [12, 5],
           [5, 13],
           [11, 7],
           [12, 7],
           [13, 7],
           [11, 12],
           [11, 13],
           [12, 13],
           [0],
           [1],
           [2],
           [3],
           [4],
           [5],
           [6],
           [7],
           [8],
           [9],
           [10],
           [11],
           [12],
           [13]],
 'weights': [1.5, 1.5, 1.5, -15.0, -15.0, -15.0, -7.5, -15.0, -15.0, 6.0, 6.0, 3.0, 6.0, 6.0, 6.0, 3.0, 6.0, 6.0, 
             3.0, 6.0, 6.0, 3.0, 3.0, 6.0, -15.0, -15.0, -15.0, -7.5, -15.0, -15.0, 6.0, 6.0, 3.0, 6.0, 6.0, 6.0, 
             3.0, 6.0, 6.0, 3.0, 6.0, 6.0, 3.0, 3.0, 6.0, 44.5, 44.5, -18.0, -18.0, -18.0, -18.0, -18.0, -18.0, 
             -9.0, -18.0, -18.0, -9.0, -18.0, -18.0]}
```
